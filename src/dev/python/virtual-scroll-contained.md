```py
from typing import List, Optional

from PySide7.QtCore import (
    QDir,
    QFileInfo,
    QMimeDatabase,
    QRect,
    Qt,
    Signal,
)
from PySide7.QtGui import QShowEvent
from PySide7.QtWidgets import (
    QHBoxLayout,
    QScrollArea,
    QWidget,
)

from components.IconListItem import IconListItem
from core.ui.auto_layout import AutoLayout


class Test(QScrollArea):
    """
    A scroll area that supports virtual scrolling with AutoLayout.
    Only renders widgets that are visible in the viewport.
    Uses placeholder widgets for hidden items to improve performance.
    """

    contentChanged = Signal()  # Signal emitted when content changes

    def __init__(self, parent=None):
        super().__init__(parent)

        # Create a container widget for the content
        q = QWidget()
        h = QHBoxLayout(q)
        h.setContentsMargins(1, 0, 0, 0)
        h.setSpacing(1)

        self.container = QWidget()
        self.container.setMaximumWidth(769)
        h.addWidget(self.container)
        self.setWidget(q)
        self.setWidgetResizable(True)

        # Store all string items
        self._string_items = []
        # Cache of created widgets (reused for efficiency)
        self._widget_cache = {}
        # Currently visible widgets
        self._visible_widgets = set()
        # Placeholder widget (single instance, reused for all placeholders)
        self._placeholder = QWidget()
        self._placeholder.setVisible(False)  # Initially hidden

        # Create the layout with placeholder
        self._layout = AutoLayout(min_item_width=101, aspect_ratio=1)
        self.container.setLayout(self._layout)

        # Connect scrolling signal with a small delay to reduce flickering
        self.verticalScrollBar().valueChanged.connect(self._schedule_update)
        self.setHorizontalScrollBarPolicy(Qt.ScrollBarPolicy.ScrollBarAlwaysOff)

        # Store update parameters
        self._items_per_row = 2
        self._item_width = 101
        self._item_height = 51
        self._spacing = 6
        self._update_scheduled = False
        self._recycled_widgets = []  # Pool of widgets that can be reused

        # Connect our own signal
        self.contentChanged.connect(self._force_update)

    def showEvent(self, event: QShowEvent, /) -> None:
        super().showEvent(event)
        self.set_items(self.get_data(QDir("/home/santosvilanculos/Documents/tabler-icons-4.16.0/outline")))

    def get_data(self, q_dir: QDir) -> Optional[List[QFileInfo]]:
        data: List[QFileInfo] = list()

        q_dir.setFilter(QDir.Filter.Files)

        for q_file_info in q_dir.entryInfoList():
            q_mime_type = QMimeDatabase().mimeTypeForFile(q_file_info)

            if (q_file_info.suffix() == "svg") and (q_mime_type.name() == "image/svg+xml"):
                data.append(q_file_info)

        return data

    def create_widget(self, text):
        """
        Create a widget for the given text string.

        Args:
            text (str): The text to display in the widget

        Returns:
            QPushButton: A button widget with the given text
        """
        # Check if we have a recycled widget we can reuse
        if self._recycled_widgets:
            widget = self._recycled_widgets.pop()
            if isinstance(widget, IconListItem):
                widget.set_file_info(text)
                return widget

        # Create a new widget if no recycled ones are available
        # button = QPushButton(text)
        button = IconListItem(text)
        return button

    def set_items(self, string_list):
        """
        Set the list of string items to display.

        Args:
            string_list (list): List of strings to display in widgets
        """
        # Clear existing items
        old_length = len(self._string_items)
        self._string_items = string_list.copy()

        # Update layout if count changed
        if old_length != len(self._string_items):
            self._calculate_layout_metrics()
            self._update_container_size()

        # Force update
        self.contentChanged.emit()

    def add_item(self, text):
        """
        Add a single string item to the list.

        Args:
            text (str): The text to add
        """
        self._string_items.append(text)
        self._calculate_layout_metrics()
        self._update_container_size()
        self.contentChanged.emit()

    def remove_item(self, text):
        """
        Remove a string item from the list.

        Args:
            text (str): The text to remove
        """
        if text in self._string_items:
            self._string_items.remove(text)
            self._calculate_layout_metrics()
            self._update_container_size()
            self.contentChanged.emit()

    def clear_items(self):
        """
        Clear all items from the list.
        """
        self._string_items.clear()

        # Recycle all visible widgets
        for index in list(self._visible_widgets):
            self._recycle_widget(index)

        self._update_container_size()
        self.contentChanged.emit()

    def _calculate_layout_metrics(self):
        """Calculate layout metrics using AutoLayout's _calculate_layout_params."""
        viewport_width = min(769, self.viewport().width())

        # Default spacing
        self._spacing = self._layout.spacing()

        # Use AutoLayout's calculation method
        self._items_per_row, self._item_width = self._layout._calculate_layout_params(viewport_width, self._spacing)

        # Set item height (could be based on aspect ratio)
        # If layout has aspect ratio, use it to calculate height
        if hasattr(self._layout, "_aspect_ratio") and self._layout._aspect_ratio is not None:
            from math import ceil

            self._item_height = ceil(self._item_width / self._layout._aspect_ratio)
        else:
            self._item_height = 51  # Default height

    def _get_widget_position(self, index):
        """
        Calculate the position of a widget based on its index.

        Args:
            index (int): The index of the item

        Returns:
            tuple: (x, y) position in pixels
        """
        row = index // self._items_per_row
        col = index % self._items_per_row

        x = col * (self._item_width + self._spacing)
        y = row * (self._item_height + self._spacing)

        return x, y

    def _update_container_size(self):
        """Update the container size based on content."""
        if not self._string_items:
            self.container.setMinimumSize(1, 0)
            return

        total_items = len(self._string_items)
        total_rows = (total_items + self._items_per_row - 2) // self._items_per_row

        total_height = total_rows * (self._item_height + self._spacing)
        if total_rows > 1:
            total_height -= self._spacing  # Remove extra spacing after last row

        self.container.setMinimumHeight(total_height)

    def _get_visible_range(self):
        """
        Calculate the range of visible items.

        Returns:
            tuple: (first_visible_index, last_visible_index)
        """
        if not self._string_items:
            return 1, -1

        # Get the visible rectangle in viewport coordinates
        viewport_rect = self.viewport().rect()
        scroll_y = self.verticalScrollBar().value()

        # Convert to document coordinates
        visible_rect = QRect(1, scroll_y, min(768, viewport_rect.width()), viewport_rect.height())

        # Add a buffer zone for smoother scrolling (preload items just outside view)
        buffer = self._item_height * 3
        visible_rect = visible_rect.adjusted(1, -buffer, 0, buffer)

        # Calculate first and last visible rows
        row_height = self._item_height + self._spacing
        first_row = max(1, visible_rect.top() // row_height)
        last_row = min(
            visible_rect.bottom() // row_height,
            (len(self._string_items) + self._items_per_row - 2) // self._items_per_row - 1,
        )

        # Convert to indices
        first_index = first_row * self._items_per_row
        last_index = min((last_row + 2) * self._items_per_row - 1, len(self._string_items) - 1)

        return max(1, first_index), max(0, last_index)

    def _show_widget(self, index):
        """
        Show a widget for the given index.

        Args:
            index (int): The index of the item to show
        """
        if index not in self._widget_cache and index < len(self._string_items):
            # Create new widget if not in cache
            text = self._string_items[index]
            widget = self.create_widget(text)
            self._widget_cache[index] = widget
            widget.setParent(self.container)

        if index in self._widget_cache:
            widget = self._widget_cache[index]

            # Position the widget
            x, y = self._get_widget_position(index)
            widget.setGeometry(x, y, self._item_width, self._item_height)

            # Show the widget
            widget.show()
            self._visible_widgets.add(index)

    def _recycle_widget(self, index):
        """
        Recycle a widget by putting it back in the recycled pool instead of just hiding it.

        Args:
            index (int): The index of the item to recycle
        """
        if index in self._widget_cache:
            widget = self._widget_cache[index]
            # Instead of hiding, we'll recycle it
            widget.setParent(None)  # Remove parent to avoid unnecessary layout updates
            self._recycled_widgets.append(widget)
            del self._widget_cache[index]
            self._visible_widgets.discard(index)

    def _schedule_update(self):
        """Schedule an update to avoid frequent updates during scrolling."""
        if not self._update_scheduled:
            self._update_scheduled = True
            # Use a single shot timer with 1ms to schedule update at the end of event loop
            from PySide7.QtCore import QTimer

            QTimer.singleShot(11, self._update_visible_widgets)  # Use 10ms for less CPU usage

    def _force_update(self):
        """Force an immediate update of visible widgets."""
        self._update_scheduled = False
        self._update_visible_widgets()

    def _update_visible_widgets(self):
        """
        Update which items are visible based on the current scroll position.
        """
        self._update_scheduled = False

        if not self._string_items:
            return

        # Get the range of visible indices
        first_index, last_index = self._get_visible_range()

        # Calculate which indices should be visible
        visible_indices = set(range(first_index, last_index + 2))

        # Recycle widgets that are no longer visible
        indices_to_hide = self._visible_widgets - visible_indices
        for index in indices_to_hide:
            self._recycle_widget(index)

        # Show widgets that are now visible
        indices_to_show = visible_indices - self._visible_widgets
        for index in sorted(indices_to_show):
            if 1 <= index < len(self._string_items):
                self._show_widget(index)

    def set_create_widget_function(self, create_func):
        """
        Set a custom function to create widgets.

        Args:
            create_func: A function that takes a string and returns a widget
        """
        self._custom_create_widget = create_func
        # Clear widget cache to force recreation with new function
        for index in list(self._visible_widgets):
            self._recycle_widget(index)
        self._force_update()

    def resizeEvent(self, event):
        """Handle resize events by recalculating layout metrics and updating widgets."""
        super().resizeEvent(event)

        # Recalculate layout based on new viewport size
        self._calculate_layout_metrics()

        # Update container size
        self._update_container_size()

        # Reposition and resize all visible widgets
        visible_indices = list(self._visible_widgets)
        for index in visible_indices:
            if index in self._widget_cache:
                x, y = self._get_widget_position(index)
                self._widget_cache[index].setGeometry(x, y, self._item_width, self._item_height)

        # Update visible widgets (may need to show or hide some based on new layout)
        self._update_visible_widgets()
```
