# vim

## misc

 - set vertical line at column 80 `:set colorcolumn=80` or `:set cc=80`
 - unset vertical line`:set colorcolumn=` or `:set cc=`
 - format json `:%!python -m json.tool`
## netrw

### help

`:help netrw`

### explore

| lazy     | mnemonic      | file exlorer                                   |
|----------|---------------|------------------------------------------------|
| **:e.**  | **:edit .**   | at current working dir.                        |
| **:sp.** | **:split .**  | in split at current working dir.               |
| **:vp.** | **:vsplit .** | in vertical split at current working dir.      |
| **:E**   | **:Explore**  | at directory of current file                   |
| **:Se**  | **:Sexplore** | in split at directory of current file          |
| **:Vex** | **:Vexplore** | in vertical split at directory of current file |

### manipulate

| command | action           |
|---------|------------------|
| **%**   | new file         |
| **d**   | new dir.         |
| **R**   | rename file/dir. |
| **D**   | delete file/dir. |
