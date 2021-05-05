# Terminal

## Terminologies
### Terminal
- A text-based interface (the actual application) to the computer.
- Analogy: ATM
### Shell
- The program/software running on the terminal.
- Analogy: the software running on the ATM.
- Ex: **Bash**, **Z Shell (zsh)**.

## Terminal Commands
- `~`
  - Refers to the home directory (Mac default: user).
- `/`
  - Refers to the root directory.
- `\ `
  - Indicates space between words.
  - Ex: `cd ~/Library/Mobile\ Documents/com~apple~CloudDocs` changes directory to iCloud.
- `ls`
  - List the contents of the current directory that I am in.
- `pwd`
  - Prints the path to the working(current) directory.
- `cd`
  - Change and move between directories(folders).
  - Into a folder (that is in the current directory)
    - `cd folder1`
  - Out of a folder (current directory)
    - `cd ..`
  - Straight to the home directory
    - `cd ~`
  - Straight to the root directory
    - `cd /`
  - Absolute vs. Relative Paths
    - Absolute path starts with `/`, starting from the root directory.
- `mkdir`
  - Make a new directory (or directories).
  - Ex: `mkdir folder1 folder2 folder3`
  - *Can also make a folder else where using absolute or relative paths*
- `touch`
  - Create a file (or files).
  - If the file already exists, it will change the access/modification time of it.
  - Ex: `touch index.html style.css app.js readme.md`
- `rm`
  - Delete file(s) *permanently* (does NOT got to trash).
  - Ex: `rm index.html style.css app.js readme.md`
- `rmdir`
  - Delete *empty* folder(s).
- `rm -rf`
  - Delete folder(s) *permanently* even if it has files in it.
  - `-r` flag: recursive (remove everything nested).
  - `-f` flag: force (don't ask for confirmation).
- `man`
  - Short for manual page.
  - Displays information about something.
  - Ex: `man ls` displays information on `ls`.
- `clear` or cmd+k
  - Clear the console.
- Flags
  - Different characters that can be passed in to alter/modify the behavior of the command.
  - Ex: `ls -a` displays all hidden files (files that start with (.)) in the current directory.
  - Ex: `mkdir -v folder1 folder2 folder3` lists the folders as they are created.
  - Multiple flags can be combined together.
    - Ex: `ls -la`
