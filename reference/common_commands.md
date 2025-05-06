 # Unix Command Reference

## File Navigation and Management

| Command | Description | Common Options |
|---------|-------------|---------------|
| `pwd` | Print working directory | |
| `ls` | List directory contents | `-l` (long format), `-a` (all files, including hidden), `-h` (human-readable sizes) |
| `cd` | Change directory | `cd ~` (home dir), `cd -` (previous dir), `cd ..` (parent dir) |
| `mkdir` | Create directory | `-p` (create parent dirs as needed) |
| `touch` | Create empty file or update timestamp | `-a` (change access time only), `-m` (change modification time only) |
| `cp` | Copy files or directories | `-r` (recursive), `-i` (interactive), `-v` (verbose) |
| `mv` | Move or rename files/directories | `-i` (interactive), `-v` (verbose) |
| `rm` | Remove files or directories | `-r` (recursive), `-f` (force), `-i` (interactive) |
| `rmdir` | Remove empty directories | `-p` (remove parent dirs if empty) |
| `find` | Search for files | `-name` (by name), `-type` (by type), `-size` (by size), `-mtime` (by modification time) |
| `locate` | Find files by name quickly | `-i` (case-insensitive) |

## File Viewing

| Command | Description | Common Options |
|---------|-------------|---------------|
| `cat` | Display file contents | `-n` (number lines) |
| `less` | View file with pagination | |
| `head` | Display beginning of file | `-n` (specify number of lines) |
| `tail` | Display end of file | `-n` (specify number of lines), `-f` (follow file updates) |
| `grep` | Search for patterns in files | `-i` (case-insensitive), `-r` (recursive), `-v` (invert match), `-n` (show line numbers) |
| `wc` | Count lines, words, characters | `-l` (lines only), `-w` (words only), `-c` (bytes only) |

## Text Processing

| Command | Description | Common Options |
|---------|-------------|---------------|
| `sort` | Sort lines of text | `-n` (numeric sort), `-r` (reverse), `-k` (specify field) |
| `uniq` | Report or filter out repeated lines | `-c` (count occurrences), `-d` (only show duplicates) |
| `cut` | Extract sections from lines | `-d` (delimiter), `-f` (fields) |
| `paste` | Merge lines from files | `-d` (delimiter) |
| `tr` | Translate characters | `-d` (delete characters) |
| `sed` | Stream editor for filtering/transforming text | `-i` (edit in place), `-e` (multiple commands) |
| `awk` | Pattern scanning and text processing | `-F` (field separator) |

## File Permissions and Ownership

| Command | Description | Common Options |
|---------|-------------|---------------|
| `chmod` | Change file permissions | `+x` (add execute), `-w` (remove write), etc. |
| `chown` | Change file owner | `-R` (recursive) |
| `chgrp` | Change group ownership | `-R` (recursive) |
| `umask` | Set default permissions | |
| `sudo` | Execute command as another user | `-u` (specify user) |

## Process Management

| Command | Description | Common Options |
|---------|-------------|---------------|
| `ps` | Report process status | `aux` (all processes), `f` (full format) |
| `top` | Dynamic process viewer | |
| `htop` | Interactive process viewer | |
| `kill` | Terminate processes | `-9` (force kill) |
| `killall` | Kill processes by name | |
| `bg` | Put processes in background | |
| `fg` | Bring processes to foreground | |
| `jobs` | List current jobs | `-l` (show process IDs) |
| `nohup` | Run command immune to hangups | |
| `nice` | Run with modified scheduling priority | `-n` (priority value) |

## System Information

| Command | Description | Common Options |
|---------|-------------|---------------|
| `uname` | Print system information | `-a` (all info) |
| `hostname` | Show or set system hostname | |
| `uptime` | Show how long system has been running | |
| `date` | Display or set date and time | `+FORMAT` (custom format) |
| `cal` | Display calendar | `-y` (full year) |
| `df` | Report disk space usage | `-h` (human-readable) |
| `du` | Estimate file space usage | `-h` (human-readable), `-s` (summary) |
| `free` | Display memory usage | `-h` (human-readable) |

## Networking

| Command | Description | Common Options |
|---------|-------------|---------------|
| `ping` | Test network connectivity | `-c` (count) |
| `ssh` | Secure shell remote login | `-p` (port) |
| `scp` | Secure copy | `-r` (recursive) |
| `rsync` | Remote file synchronization | `-a` (archive), `-v` (verbose), `-z` (compress) |
| `curl` | Transfer data from/to server | `-O` (save to file), `-L` (follow redirects) |
| `wget` | Download files from web | `-r` (recursive), `-c` (continue) |
| `ifconfig`/`ip` | Network interface configuration | |
| `netstat`/`ss` | Network statistics | `-tuln` (TCP/UDP listening ports) |
| `traceroute` | Trace route to host | |
| `dig`/`nslookup` | DNS lookup | |

## Compression

| Command | Description | Common Options |
|---------|-------------|---------------|
| `tar` | Archive files | `-c` (create), `-x` (extract), `-f` (file), `-z` (gzip), `-j` (bzip2) |
| `gzip` | Compress files | `-d` (decompress), `-k` (keep original) |
| `gunzip` | Decompress gzip files | |
| `zip` | Package and compress files | `-r` (recursive) |
| `unzip` | Extract zip files | |

## Shell Features

| Command/Feature | Description |
|---------|-------------|
| `alias` | Create command shortcuts |
| `history` | Command history |
| `!` | History expansion (e.g., `!!` to repeat last command) |
| `>` | Redirect output to file (overwrite) |
| `>>` | Redirect output to file (append) |
| `<` | Redirect input from file |
| `|` | Pipe output of one command as input to another |
| `&` | Run command in background |
| `&&` | Chain commands (run second if first succeeds) |
| `||` | Chain commands (run second if first fails) |
| `;` | Command separator (run commands sequentially) |
| `*` | Wildcard (matches any characters) |
| `?` | Wildcard (matches one character) |
| `[]` | Character class (matches any character in brackets) |
| `{}` | Brace expansion (generates strings) |
| `ctrl+c` | Interrupt (kill) current process |
| `ctrl+z` | Suspend current process |
| `ctrl+d` | End of input |
| `ctrl+l` | Clear screen |
| `ctrl+r` | Reverse search history |
| `tab` | Auto-complete commands and filenames |