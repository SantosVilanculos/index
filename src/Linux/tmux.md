<table class="inline">
 <thead>
 <tr class="row0">
  <th class="col0 leftalign"> Command         </th><th class="col1 leftalign"> Description  </th>
 </tr>
 </thead>
 <tbody><tr class="row1">
  <td class="col0 leftalign"> Ctrl+b, c       </td><td class="col1 leftalign"> Create a new window  </td>
 </tr>
 <tr class="row2">
  <td class="col0 leftalign"> Ctrl+b, n       </td><td class="col1 leftalign"> Switch to the next window  </td>
 </tr>
 <tr class="row3">
  <td class="col0 leftalign"> Ctrl+b, p       </td><td class="col1 leftalign"> Switch to the previous window  </td>
 </tr>
 <tr class="row4">
  <td class="col0 leftalign"> Ctrl+b, [0..9]  </td><td class="col1 leftalign"> Switch directly to window number 0 to 9  </td>
 </tr>
 <tr class="row5">
  <td class="col0 leftalign"> Ctrl+b, l       </td><td class="col1 leftalign"> Switch back to the last (most recently shown) window  </td>
 </tr>
 <tr class="row6">
  <td class="col0 leftalign"> Ctrl+b, d       </td><td class="col1 leftalign"> Detach from tmux session <br>
Puts tmux in the background  </td>
 </tr>
 <tr class="row7">
  <td class="col0 leftalign"> Ctrl+b, ,       </td><td class="col1 leftalign"> (Control+b, comma) <br>
Rename the current window  </td>
 </tr>
 <tr class="row8">
  <td class="col0 leftalign"> Ctrl+b, PageUp  <br>
Ctrl+b, ]  </td><td class="col1"> Switch to scroll-back mode. Exit by pressing Escape. </td>
 </tr>
 <tr class="row9">
  <td class="col0 leftalign"> Ctrl+b, &amp;       </td><td class="col1 leftalign"> Forcibly destroy the current window. <br>
Use only if the window doesn't respond. It's better to close the window by running the <code>logout</code> or <code>exit</code> shell command or pressing <code>Ctrl+d</code>.  </td>
 </tr>
 <tr class="row10">
  <td class="col0 leftalign"> Ctrl+b, Ctrl+b  </td><td class="col1 leftalign"> Tmux usually "swallows" the Ctrl+b keypress. To send an Ctrl+b keypress to the program running inside tmux, press Ctrl+b twice.  </td>
 </tr>
</tbody></table>

```sh
#!/usr/bin/env bash

tmux new-window -t=0 -d -k 'lazygit'
tmux new-window -t=1 -d -k 'pnpm run dev'
tmux new-window -t=2
```
