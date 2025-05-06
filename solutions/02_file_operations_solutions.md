## 2.1: Creating and Viewing Files

```bash
# Navigate to unix_practice directory
cd unix_practice

# Create empty files
touch notes.txt todo.txt

# Check that files exist
ls

# Add text to notes.txt
echo "This is my first Unix file" > notes.txt

# Display contents
cat notes.txt

# Add another line
echo "Learning Unix commands is fun!" >> notes.txt

# View file again
cat notes.txt
```

## 2.2: Copying and Moving Files

```bash
# Copy notes.txt to notes_backup.txt
cp notes.txt notes_backup.txt

# Verify contents are the same
cat notes.txt notes_backup.txt
# Or use diff to compare
diff notes.txt notes_backup.txt

# Create archive directory
mkdir archive

# Move backup to archive directory
mv notes_backup.txt archive/

# Rename todo.txt to tasks.txt
mv todo.txt tasks.txt

# Create a new file
touch meeting_notes.txt

# Copy both files to archive directory
cp tasks.txt meeting_notes.txt archive/
```

## 2.3: Viewing Larger Files

```bash
# Create a file with many lines
for i in {1..100}; do echo "This is line $i" >> large_file.txt; done

# View first 5 lines
head -5 large_file.txt

# View last 10 lines
tail -10 large_file.txt

# Browse through file interactively
less large_file.txt
# (press q to quit less)
```

## 2.4: Removing Files and Directories

```bash
# Create a temporary file
touch temporary.txt

# Remove the file
rm temporary.txt

# Create directory with files
mkdir temp_dir
touch temp_dir/file1.txt temp_dir/file2.txt

# Try to remove directory (will fail)
rm temp_dir

# Remove directory with contents
rm -r temp_dir
```

## 2.5: Challenge

```bash
# Create directory structure
mkdir -p project/docs project/src
touch project/README.txt project/docs/index.md project/docs/guide.md project/src/main.txt project/src/utils.txt

# Add text to each file
echo "Project README file" > project/README.txt
echo "Documentation index" > project/docs/index.md
echo "User guide document" > project/docs/guide.md
echo "Main source code" > project/src/main.txt
echo "Utility functions" > project/src/utils.txt

# Copy docs directory to create backup
cp -r project/docs project/docs_backup

# Move utils.txt to main directory and rename
mv project/src/utils.txt project/utilities.txt

# Remove src directory and contents
rm -r project/src
```

## Additional Notes

- `>` redirects output to a file, overwriting any existing content
- `>>` appends output to a file, preserving existing content
- `mv` is used to both move files/directories and rename them
- `cp` copies files, while `cp -r` copies directories recursively
- `rm` removes files, while `rm -r` removes directories and their contents
- Always be careful with `rm`, especially with the `-r` and `-f` options, as deleted files cannot be easily recovered
