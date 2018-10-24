# Bash

## Search history

- `touch ~/.inputrc`
- put the following lines into it

    ```
    ## arrow up
    "\e[A":history-search-backward
    ## arrow down
    "\e[B":history-search-forward
    ```
- restart shell by typing `$SHELL`
