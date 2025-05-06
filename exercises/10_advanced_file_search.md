 # Exercise 10: Advanced File Search

## Concepts Covered
- Finding files by name, type, size, and date
- Searching file contents
- Combining search criteria
- Taking actions on found files
- Using specialized search tools

## Commands to Learn
- `find` - Search for files in a directory hierarchy
- `locate` - Find files by name quickly
- `grep` with recursive options
- `xargs` - Build and execute command lines from input
- `find` with `-exec` option

## Exercises

### 10.1: Finding Files by Name and Type

1. Use the `find` command to locate all `.txt` files in your home directory
2. Find all `.jpg` and `.png` files (use the `-o` operator to combine conditions)
3. Find only directories whose names contain the word "data"
4. Find all symbolic links in a specific directory
5. Use `locate` to quickly find a specific file by name (note: you may need to run `sudo updatedb` first)

### 10.2: Finding Files by Size

1. Find all files larger than 10MB in your home directory
2. Find all files smaller than 1KB in a specific directory
3. Find empty files
4. Find the 5 largest files in a directory tree (combine `find` with other commands)
5. Find files between 1MB and 10MB in size

### 10.3: Finding Files by Time and Date

1. Find files modified in the last 24 hours
2. Find files accessed more than 30 days ago
3. Find files created in a specific date range
4. Find files modified more recently than a reference file
5. Find and delete files older than 90 days in a specific directory (use caution!)

### 10.4: Searching File Contents

1. Use `grep` recursively to find all files containing a specific word
2. Search for a pattern only in files of a specific type (e.g., only in `.py` files)
3. Use `grep` with context options to show lines before and after matches
4. Search for files containing multiple different patterns
5. Use regular expressions with `grep` to find complex patterns

### 10.5: Combining Find with Other Commands

1. Find all `.log` files and display their sizes in human-readable format
2. Find all Python files (`.py`) and count the lines of code in each
3. Find files modified in the last week and create a backup of each
4. Find specific types of files and move them to a different directory
5. Find large files that haven't been accessed in months

### 10.6: Using xargs and -exec

1. Find all `.txt` files and pass their names to a `cat` command
2. Find image files and create thumbnails for each (you can simulate this)
3. Compare the use of `find` with `-exec` versus piping to `xargs`
4. Use `find` with `-exec` to search and replace text in multiple files
5. Use `xargs` with the `-P` option to process files in parallel

### 10.7: Challenge - File System Cleanup Script

Create a script called `cleanup.sh` that:

1. Takes a directory as an argument
2. Finds and reports on:
   - Duplicate files (based on content, not just name)
   - Empty directories
   - Files that haven't been accessed in over a year
   - Unusually large files
   - Temporary files (e.g., those ending in `.tmp` or `~`)
3. For each category, offers to perform appropriate actions (delete, archive, etc.)
4. Generates a summary report of space that could be freed
5. Includes a "dry run" option that shows what would be done without actually doing it

## Reflection Questions
- How does the `find` command compare to graphical file search tools?
- What are the performance differences between `find` and `locate`?
- When would you use `-exec` versus piping to `xargs`?
- How could advanced file search techniques help in system administration or data management tasks?