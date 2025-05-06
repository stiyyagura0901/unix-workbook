 # Exercise 1: Navigation Basics - Solutions

### 1.1: Understanding Your Location
- Command to find current directory: `pwd`

### 1.2: Exploring Directory Contents
- List all files and directories: `ls`
- List with more details: `ls -l`
- List hidden files: `ls -a` or `ls -la` for hidden files with details

### 1.3: Directory Navigation
```bash
# Navigate to home directory
cd ~ 
# or simply
cd

# Create new directory
mkdir unix_practice

# Move into that directory
cd unix_practice

# Create two subdirectories
mkdir documents backups

# Navigate into documents
cd documents

# Navigate back up one level
cd ..

# Navigate to backups using relative path
cd backups

# Return to home using absolute path
cd /home/username  # Replace username with your actual username
# or
cd ~
```

### 1.4: Challenge
```bash
# Create nested directory structure in one command
mkdir -p projects/website/public/images

# Navigate directly to images
cd projects/website/public/images

# Return to projects using relative path
cd ../../..

# List all directories with permissions
ls -la
```

## Additional Notes

- Absolute paths start from the root directory (/) and specify the complete path
- Relative paths start from the current directory
- `cd ..` moves up one directory level (parent directory)
- `cd ~` or just `cd` takes you to your home directory
- `mkdir -p` creates parent directories as needed