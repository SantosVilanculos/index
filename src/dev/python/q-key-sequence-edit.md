```py
from PySide6.QtCore import QKeyCombination, Slot
from PySide6.QtGui import QKeySequence
from PySide6.QtWidgets import QKeySequenceEdit

q_key_sequence = QKeySequence.fromString("Ctrl+Q"))
q_key_sequence_edit = QKeySequenceEdit(q_key_sequence)

@Slot(QKeySequence)
def key_sequence_changed(q_key_sequence: QKeySequence) -> None:
 print(q_key_sequence.toString(QKeySequence.SequenceFormat.PortableText))

q_key_sequence_edit.keySequenceChanged.connect(key_sequence_changed)
```
