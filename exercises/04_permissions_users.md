 # Exercise 4: Permissions and Users

## Concepts Covered
- File and directory permissions
- Permission notation (symbolic and numeric)
- Changing owners and groups
- Understanding users and groups

## Commands to Learn
- `chmod` - Change file/directory permissions
- `chown` - Change file/directory owner
- `chgrp` - Change group ownership
- `sudo` - Execute commands as another user (usually as superuser)
- `id` - Display user and group information
- `groups` - Show group memberships
- `ls -l` - List files with permissions

## Exercises

### 4.1: Understanding Permissions

1. Create a new directory called `permission_practice`
2. Inside this directory, create a file called `script.sh` with the following content:
   ```bash
   #!/bin/bash
   echo "This script is running as user: $(whoami)"
   echo "Current date and time: $(date)"
   ```
3. Use `ls -l` to examine the permissions of the file
4. Try to execute the script with `./script.sh` - what happens?
5. Make the script executable using chmod
6. Try running the script again
7. Create a file called `private_notes.txt` with some sample text

### 4.2: Changing Permissions

1. Using symbolic notation, change the permissions of `private_notes.txt` so that:
   - You (owner) can read and write to it
   - Your group can only read it
   - Others cannot access it at all
2. Verify the permissions with `ls -l`
3. Now change the permissions using numeric (octal) notation to achieve the same result
4. Create a new directory called `shared_docs`
5. Set the permissions so anyone can list its contents, but only you can add or remove files

### 4.3: Understanding User and Group Information

1. Use the `id` command to see your user and group IDs
2. Use the `groups` command to see which groups you belong to
3. Check who owns the files in your home directory with `ls -l ~`

### 4.4: Special Permissions - Understanding setuid, setgid, and sticky bit

1. Create a directory called `team_folder`
2. Set both the group ID bit and appropriate permissions on this directory
3. Explain what would happen when new files are created in this directory
4. Research and explain what the "sticky bit" does when set on a directory
5. Find examples of directories on your system that have the sticky bit set

### 4.5: Challenge - Setting Up Secure Collaboration Directory

1. Create the following structure:
   ```
   project/
   ├── public_docs/
   │   └── readme.txt
   ├── team_docs/
   │   └── plans.txt
   └── admin_docs/
       └── credentials.txt
   ```
   
2. Set permissions so that:
   - `public_docs` is readable and executable by everyone
   - `team_docs` is readable and executable by your user and group, but not by others
   - `admin_docs` is only accessible by your user
   - `readme.txt` is readable by everyone
   - `plans.txt` is readable and writable by your user and group
   - `credentials.txt` is only readable and writable by your user

3. Write the commands you would use to verify these permissions are set correctly

## Reflection Questions
- Why is it important to be careful with file permissions?
- When might you want to use `sudo`, and what precautions should you take?
- How do permissions differ between files and directories?
- What security concerns should you consider when setting permissions?
