 # Exercise 2: File Operations

## Concepts Covered
- Creating files
- Viewing file contents
- Copying, moving, and renaming files
- Removing files and directories

## Commands to Learn
- `touch` - Create empty files
- `cat` - Display file contents
- `cp` - Copy files and directories
- `mv` - Move or rename files and directories
- `rm` - Remove files and directories
- `head`/`tail` - View beginning/end of files
- `less` - View file contents with pagination

## Exercises

### 2.1: Creating and Viewing Files
1. Navigate to the `unix_practice` directory you created in Exercise 1
2. Create an empty file called `notes.txt`
3. Create another file called `todo.txt`
4. Use the appropriate command to check that both files exist
5. Use `echo` to add the text "This is my first Unix file" to `notes.txt` (hint: use redirection with `>`)
6. Display the contents of the file on the terminal
7. Add another line "Learning Unix commands is fun!" to the file (hint: use `>>`)
8. View the file again to confirm both lines are present

### 2.2: Copying and Moving Files
1. Create a copy of `notes.txt` called `notes_backup.txt`
2. Verify the contents of both files are the same
3. Create a new directory called `archive`
4. Move `notes_backup.txt` into the `archive` directory
5. Rename `todo.txt` to `tasks.txt`
6. Create a new file `meeting_notes.txt` in your current directory
7. Copy both `tasks.txt` and `meeting_notes.txt` to the `archive` directory in a single command

### 2.3: Viewing Larger Files
1. Use `echo` with a loop to create a file with many lines (here's a command to do this):
   ```bash
   for i in {1..100}; do echo "This is line $i" >> large_file.txt; done
   ```
2. Use the command to view just the first 5 lines of the file
3. Now view just the last 10 lines
4. Use the pager program to browse through the file interactively

### 2.4: Removing Files and Directories
1. Create a file called `temporary.txt`
2. Remove this file
3. Create a directory called `temp_dir` and place a few files inside it
4. Try to remove the directory with the standard remove command - what happens?
5. Use the appropriate option to remove the directory and all of its contents

### 2.5: Challenge
1. Create the following structure in your `unix_practice` directory:
   ```
   project/
   ├── docs/
   │   ├── index.md
   │   └── guide.md
   ├── src/
   │   ├── main.txt
   │   └── utils.txt
   └── README.txt
   ```
2. Add some text to each file
3. Copy the entire docs directory to create a `docs_backup` directory
4. Move the `utils.txt` file to the main directory and rename it to `utilities.txt`
5. Remove the `src` directory and its contents

## Reflection Questions
- What's the difference between `>` and `>>` when redirecting output?
- When would you use `mv` versus `cp`?
- Why is it important to be careful with the `rm` command, especially with certain options?
