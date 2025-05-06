# Exercise 4: Permissions and Users - Solutions

## 4.1: Understanding Permissions

```bash
# Create a directory
mkdir permission_practice

# Change to that directory
cd permission_practice

# Create script.sh with content
cat > script.sh << 'EOF'
#!/bin/bash
echo "This script is running as user: $(whoami)"
echo "Current date and time: $(date)"
EOF

# Check permissions
ls -l script.sh
# Expected output: -rw-r--r-- or similar (not executable)

# Try to execute (will fail)
./script.sh
# Expected error: Permission denied

# Make executable
chmod +x script.sh

# Try running again (will work)
./script.sh

# Create private_notes.txt
echo "This is confidential information" > private_notes.txt
```

## 4.2: Changing Permissions

```bash
# Using symbolic notation
chmod u+rw,g+r,g-w,o-rwx private_notes.txt

# Verify permissions
ls -l private_notes.txt
# Expected: -rw-r----- or similar

# Using numeric notation (same permissions)
chmod 640 private_notes.txt

# Create shared_docs directory
mkdir shared_docs

# Set permissions: list for all, modify only for owner
chmod 755 shared_docs
# Or symbolic notation:
# chmod u=rwx,g=rx,o=rx shared_docs
```

## 4.3: Understanding User and Group Information

```bash
# View user and group IDs
id

# View groups
groups

# Check home directory file ownership
ls -l ~
```

## 4.4: Special Permissions

```bash
# Create directory
mkdir team_folder

# Set group ID bit and permissions
chmod 2775 team_folder
# Or with symbolic notation:
# chmod g+s,u=rwx,g=rwx,o=rx team_folder

# Verify permissions
ls -ld team_folder
# Expected: drwxrwsr-x or similar with the 's' in the group permissions

# Find directories with sticky bit
ls -ld /tmp
# Expected: drwxrwxrwt or similar with the 't' in the 'others' permissions
```

Explanation for setgid: When the setgid bit is set on a directory, new files created in that directory inherit the group ID of the directory rather than the primary group of the user creating the file. This is useful for shared directories where all files should be accessible to members of a specific group.

Explanation for sticky bit: The sticky bit on a directory prevents users from deleting or renaming files unless they are the owner of the file or the directory, even if they have write permission to the directory. This is commonly used on directories like /tmp where multiple users can create files.

## 4.5: Challenge - Setting Up Secure Collaboration Directory

```bash
# Create directory structure
mkdir -p project/public_docs project/team_docs project/admin_docs

# Create sample files
touch project/public_docs/readme.txt
touch project/team_docs/plans.txt
touch project/admin_docs/credentials.txt

# Set directory permissions
chmod 755 project/public_docs  # rwxr-xr-x - readable/executable by everyone
chmod 750 project/team_docs    # rwxr-x--- - readable/executable by user and group
chmod 700 project/admin_docs   # rwx------ - only accessible by user

# Set file permissions
chmod 644 project/public_docs/readme.txt    # rw-r--r-- - readable by everyone
chmod 660 project/team_docs/plans.txt       # rw-rw---- - readable/writable by user and group
chmod 600 project/admin_docs/credentials.txt # rw------- - only readable/writable by user

# Verify permissions
ls -la project/
ls -la project/public_docs/
ls -la project/team_docs/
ls -la project/admin_docs/
```

## Additional Notes

- File permissions consist of read (r), write (w), and execute (x) permissions for the owner, group, and others
- Numeric notation is a shorthand where r=4, w=2, x=1 and you add them up for each category
- Special permissions include setuid (4), setgid (2), and sticky bit (1) in the fourth digit
- Directory permissions: read allows listing contents, write allows adding/removing files, execute allows entering the directory
- Common permission combinations:
  - 755 (rwxr-xr-x): Standard for directories and executable scripts
  - 644 (rw-r--r--): Standard for regular files
  - 600 (rw-------): Private files
  - 777 (rwxrwxrwx): Full access to everyone (generally avoided for security reasons)
