# Exercise 1: Navigation Basics

## Concepts Covered

- File system structure
- Current working directory
- Navigating directories
- Listing files and directories

## Commands to Learn

- `pwd` - Print Working Directory
- `ls` - List directory contents
- `cd` - Change Directory
- `mkdir` - Make Directory

## Exercises

### 1.1: Understanding Your Location

1. Open your terminal
2. What command would you use to find out which directory you're currently in?
3. Run this command and note the result

### 1.2: Exploring Directory Contents

1. Use the appropriate command to list all files and directories in your current location
2. Now try listing them with more details (hint: use options/flags with the command)
3. How would you list hidden files as well?

### 1.3: Directory Navigation

1. Navigate to your home directory (hint: `cd` with no arguments or `cd ~`)
2. Create a new directory called `unix_practice`
3. Move into that directory
4. Create two subdirectories: `documents` and `backups`
5. Navigate into `documents`
6. Navigate back up one level to `unix_practice`
7. Navigate to the `backups` directory using a relative path
8. Return to your home directory using an absolute path

### 1.4: Challenge

1. Start in your home directory
2. In a single command line, create the following directory structure:
   ```
   projects/website/public/images
   ```
3. Navigate directly to the images directory using a single command
4. Return to the projects directory using a relative path
5. List all directories you've created with their permissions

## Reflection Questions

- What's the difference between absolute and relative paths?
- When would you use `cd ..` versus `cd /some/specific/path`?
- How can you quickly return to your home directory from anywhere?
