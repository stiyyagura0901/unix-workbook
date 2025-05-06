# Exercise 10: Advanced File Search - Solutions

## 10.1: Finding Files by Name and Type

```bash
# Find all .txt files in home directory
find ~ -name "*.txt"

# Find all .jpg and .png files
find ~ -name "*.jpg" -o -name "*.png"

# Find directories whose names contain "data"
find ~ -type d -name "*data*"

# Find all symbolic links in a specific directory
find /usr/local -type l

# Use locate to quickly find a file by name
# First update the database (might need sudo)
sudo updatedb
# Then search
locate filename.txt
```

## 10.2: Finding Files by Size

```bash
# Find files larger than 10MB
find ~ -type f -size +10M

# Find files smaller than 1KB
find ~ -type f -size -1k

# Find empty files
find ~ -type f -empty

# Find the 5 largest files in a directory tree
find ~ -type f -exec ls -lh {} \; | sort -k5 -rh | head -5
# Or a more efficient version:
find ~ -type f -printf "%s %p\n" | sort -rn | head -5 | awk '{print $2}' | xargs ls -lh

# Find files between 1MB and 10MB
find ~ -type f -size +1M -size -10M
```

## 10.3: Finding Files by Time and Date

```bash
# Find files modified in the last 24 hours
find ~ -type f -mtime -1

# Find files accessed more than 30 days ago
find ~ -type f -atime +30

# Find files created in a specific date range (using stat's modification time)
# Files modified after Jan 1, 2023
find ~ -type f -newermt "2023-01-01"
# Files modified before Dec 31, 2023
find ~ -type f -not -newermt "2023-12-31"
# Combining both for a date range
find ~ -type f -newermt "2023-01-01" -not -newermt "2023-12-31"

# Find files modified more recently than a reference file
touch -t 202301010000 /tmp/reference_file
find ~ -type f -newer /tmp/reference_file

# Find and delete files older than 90 days (use with caution!)
find ~/old_logs -type f -mtime +90 -delete
# Or safer, preview first:
find ~/old_logs -type f -mtime +90 -print
```

## 10.4: Searching File Contents

```bash
# Find all files containing a specific word
grep -r "searchterm" ~

# Search for a pattern only in specific file types
find ~ -name "*.py" -exec grep "import" {} \;
# Or more efficiently:
grep -r --include="*.py" "import" ~

# Use grep with context
grep -r -n -A2 -B2 "error" ~/logs
# -n shows line numbers
# -A2 shows 2 lines after match
# -B2 shows 2 lines before match

# Search for files containing multiple patterns
grep -l "first_pattern" ~/documents/*.txt | xargs grep -l "second_pattern"

# Use regular expressions with grep
grep -r -E "error[0-9]{3}" ~/logs
```

## 10.5: Combining Find with Other Commands

```bash
# Find all .log files and display their sizes
find ~/logs -name "*.log" -exec ls -lh {} \;

# Count lines of code in Python files
find ~/projects -name "*.py" -exec wc -l {} \;

# Create backups of files modified in the last week
find ~/documents -type f -mtime -7 -exec cp {} {}.bak \;

# Move specific types of files to a different directory
find ~/downloads -name "*.pdf" -exec mv {} ~/documents/pdfs/ \;

# Find large files that haven't been accessed in months
find ~ -type f -size +100M -atime +90 -ls
```

## 10.6: Using xargs and -exec

```bash
# Find all .txt files and pass to cat
find ~ -name "*.txt" | xargs cat

# Create thumbnails for image files (simulated)
find ~/pictures -name "*.jpg" | xargs -I{} echo "convert {} -resize 100x100 {}.thumb"

# Compare find with -exec versus piping to xargs
# Using -exec
find ~/documents -name "*.txt" -exec grep "important" {} \;
# Using xargs
find ~/documents -name "*.txt" | xargs grep "important"

# Search and replace in multiple files
find ~/projects -name "*.html" -exec sed -i 's/old/new/g' {} \;

# Process files in parallel with xargs
find ~/large_directory -name "*.log" | xargs -P 4 -I{} gzip {}
```

## 10.7: Challenge - File System Cleanup Script

```bash
#!/bin/bash
# cleanup.sh - File System Cleanup Script

# Display help
show_help() {
    echo "Usage: $0 [OPTIONS] DIRECTORY"
    echo ""
    echo "Scan a directory for potential cleanup candidates and offer to remove them."
    echo ""
    echo "Options:"
    echo "  -d, --dry-run    Show what would be done without actually doing it"
    echo "  -h, --help       Display this help message and exit"
    echo ""
    exit 0
}

# Process arguments
DRY_RUN=0
TARGET_DIR=""

while [[ $# -gt 0 ]]; do
    case $1 in
        -d|--dry-run)
            DRY_RUN=1
            shift
            ;;
        -h|--help)
            show_help
            ;;
        *)
            if [[ -z "$TARGET_DIR" ]]; then
                TARGET_DIR="$1"
            else
                echo "Error: Multiple directory arguments provided."
                echo "Please specify only one directory to clean up."
                exit 1
            fi
            shift
            ;;
    esac
done

# Check if a directory was provided
if [[ -z "$TARGET_DIR" ]]; then
    echo "Error: No directory specified."
    echo "Please provide a directory to clean up."
    exit 1
fi

# Check if the directory exists
if [[ ! -d "$TARGET_DIR" ]]; then
    echo "Error: '$TARGET_DIR' is not a valid directory."
    exit 1
fi

# Use absolute path
TARGET_DIR=$(realpath "$TARGET_DIR")
echo "Scanning directory: $TARGET_DIR"
if [[ $DRY_RUN -eq 1 ]]; then
    echo "DRY RUN MODE: No actual changes will be made."
fi
echo ""

# Function to prompt for action
prompt_action() {
    local question="$1"
    local default="${2:-n}"

    if [[ $DRY_RUN -eq 1 ]]; then
        # In dry run mode, always assume 'n'
        return 1
    fi

    while true; do
        if [[ "$default" = "y" ]]; then
            read -p "$question [Y/n] " answer
            answer=${answer:-y}
        else
            read -p "$question [y/N] " answer
            answer=${answer:-n}
        fi

        case $answer in
            [Yy]*)
                return 0
                ;;
            [Nn]*)
                return 1
                ;;
            *)
                echo "Please answer y or n."
                ;;
        esac
    done
}

# Function to find and handle duplicate files
find_duplicates() {
    echo "=== Searching for Duplicate Files ==="

    # Create a temporary file for storing file checksums
    CHECKSUM_FILE=$(mktemp)

    # Find all regular files, calculate their MD5 sums, and find duplicates
    echo "Calculating checksums for all files (this may take a while)..."
    find "$TARGET_DIR" -type f -exec md5sum {} \; | sort > "$CHECKSUM_FILE"

    # Find duplicate checksums
    echo "Identifying duplicates..."
    DUPLICATES=$(awk '{print $1}' "$CHECKSUM_FILE" | uniq -d)

    # Process each duplicate checksum
    TOTAL_DUPLICATES=0
    TOTAL_SIZE_SAVED=0

    if [[ -z "$DUPLICATES" ]]; then
        echo "No duplicate files found."
    else
        while read -r checksum; do
            # Get all files with this checksum
            files=$(grep "$checksum" "$CHECKSUM_FILE" | cut -d ' ' -f 3-)

            # Count files and get file size
            file_count=$(echo "$files" | wc -l)
            file_size=$(stat -f "%z" $(echo "$files" | head -1))
            size_saved=$(( (file_count - 1) * file_size ))

            TOTAL_DUPLICATES=$((TOTAL_DUPLICATES + file_count - 1))
            TOTAL_SIZE_SAVED=$((TOTAL_SIZE_SAVED + size_saved))

            echo "Found $file_count duplicate files with checksum $checksum:"
            echo "$files" | nl
            echo "Potential space savings: $(numfmt --to=iec $size_saved)"
            echo ""

            if prompt_action "Delete all but the first file?"; then
                # Keep the first file, delete the rest
                echo "$files" | tail -n+2 | while read -r file; do
                    if [[ $DRY_RUN -eq 0 ]]; then
                        rm "$file"
                        echo "Deleted: $file"
                    else
                        echo "Would delete: $file"
                    fi
                done
            fi
            echo ""
        done <<< "$DUPLICATES"

        echo "Summary: Found $TOTAL_DUPLICATES duplicate files"
        echo "Potential space savings: $(numfmt --to=iec $TOTAL_SIZE_SAVED)"
    fi

    # Clean up
    rm "$CHECKSUM_FILE"
    echo ""
}

# Function to find and handle empty directories
find_empty_dirs() {
    echo "=== Searching for Empty Directories ==="

    empty_dirs=$(find "$TARGET_DIR" -type d -empty)

    if [[ -z "$empty_dirs" ]]; then
        echo "No empty directories found."
    else
        count=$(echo "$empty_dirs" | wc -l)
        echo "Found $count empty directories:"
        echo "$empty_dirs" | nl

        if prompt_action "Delete all empty directories?"; then
            echo "$empty_dirs" | while read -r dir; do
                if [[ $DRY_RUN -eq 0 ]]; then
                    rmdir "$dir"
                    echo "Deleted: $dir"
                else
                    echo "Would delete: $dir"
                fi
            done
        fi
    fi
    echo ""
}

# Function to find old files
find_old_files() {
    echo "=== Searching for Old Files (not accessed in over a year) ==="

    old_files=$(find "$TARGET_DIR" -type f -atime +365)

    if [[ -z "$old_files" ]]; then
        echo "No old files found."
    else
        count=$(echo "$old_files" | wc -l)
        total_size=$(echo "$old_files" | xargs stat -f "%z" | awk '{sum+=$1} END {print sum}')

        echo "Found $count files not accessed in over a year."
        echo "Total size: $(numfmt --to=iec $total_size)"
        echo "Sample of files:"
        echo "$old_files" | head -10 | nl

        if [[ $(echo "$old_files" | wc -l) -gt 10 ]]; then
            echo "... (more files not shown)"
        fi

        echo ""
        if prompt_action "Archive these files (create a tar archive)?"; then
            archive_name="old_files_$(date +%Y%m%d).tar.gz"
            if [[ $DRY_RUN -eq 0 ]]; then
                echo "$old_files" | tar -czf "$TARGET_DIR/$archive_name" -T -
                echo "Created archive: $TARGET_DIR/$archive_name"

                if prompt_action "Delete the original files?"; then
                    echo "$old_files" | while read -r file; do
                        rm "$file"
                        echo "Deleted: $file"
                    done
                fi
            else
                echo "Would create archive: $TARGET_DIR/$archive_name"
                if prompt_action "Delete the original files?"; then
                    echo "$old_files" | while read -r file; do
                        echo "Would delete: $file"
                    done
                fi
            fi
        fi
    fi
    echo ""
}

# Function to find unusually large files
find_large_files() {
    echo "=== Searching for Unusually Large Files (>100MB) ==="

    large_files=$(find "$TARGET_DIR" -type f -size +100M)

    if [[ -z "$large_files" ]]; then
        echo "No unusually large files found."
    else
        count=$(echo "$large_files" | wc -l)
        echo "Found $count files larger than 100MB:"
        echo "$large_files" | while read -r file; do
            size=$(stat -f "%z" "$file")
            size_human=$(numfmt --to=iec $size)
            echo "- $file ($size_human)"
        done

        echo ""
        if prompt_action "Review these files individually?"; then
            echo "$large_files" | while read -r file; do
                size=$(stat -f "%z" "$file")
                size_human=$(numfmt --to=iec $size)
                echo ""
                echo "File: $file"
                echo "Size: $size_human"
                echo "Type: $(file -b "$file")"
                echo "Last accessed: $(stat -f "%Sa" "$file")"
                echo "Last modified: $(stat -f "%Sm" "$file")"

                echo "Options:"
                echo "1. Keep file"
                echo "2. Delete file"
                echo "3. Move to archive folder"

                if [[ $DRY_RUN -eq 0 ]]; then
                    read -p "Select option [1-3]: " choice
                    case $choice in
                        2)
                            rm "$file"
                            echo "Deleted: $file"
                            ;;
                        3)
                            mkdir -p "$TARGET_DIR/large_files_archive"
                            mv "$file" "$TARGET_DIR/large_files_archive/"
                            echo "Moved to: $TARGET_DIR/large_files_archive/$(basename "$file")"
                            ;;
                        *)
                            echo "Keeping file."
                            ;;
                    esac
                else
                    echo "In dry run mode, all files would be kept by default."
                fi
            done
        fi
    fi
    echo ""
}

# Function to find temporary files
find_temp_files() {
    echo "=== Searching for Temporary Files ==="

    temp_files=$(find "$TARGET_DIR" -type f \( -name "*.tmp" -o -name "*~" -o -name "*.bak" -o -name "*.swp" -o -name ".DS_Store" \))

    if [[ -z "$temp_files" ]]; then
        echo "No temporary files found."
    else
        count=$(echo "$temp_files" | wc -l)
        total_size=$(echo "$temp_files" | xargs stat -f "%z" | awk '{sum+=$1} END {print sum}')

        echo "Found $count temporary files."
        echo "Total size: $(numfmt --to=iec $total_size)"
        echo "Files:"
        echo "$temp_files" | nl

        if prompt_action "Delete all temporary files?"; then
            echo "$temp_files" | while read -r file; do
                if [[ $DRY_RUN -eq 0 ]]; then
                    rm "$file"
                    echo "Deleted: $file"
                else
                    echo "Would delete: $file"
                fi
            done
        fi
    fi
    echo ""
}

# Execute the cleanup functions
find_duplicates
find_empty_dirs
find_old_files
find_large_files
find_temp_files

# Print summary
echo "=== Cleanup Summary ==="
echo "Directory scanned: $TARGET_DIR"
if [[ $DRY_RUN -eq 1 ]]; then
    echo "This was a dry run. No actual changes were made."
    echo "Run without -d or --dry-run option to perform actual cleanup."
fi
echo "Scan completed at: $(date)"
```

Make it executable and run:

```bash
chmod +x cleanup.sh
./cleanup.sh --dry-run /path/to/directory
# After reviewing, run without dry-run to actually clean up
./cleanup.sh /path/to/directory
```

## Additional Notes

### find Command Basics

- `-name` searches by filename (case-sensitive)
- `-iname` searches by filename (case-insensitive)
- `-type` specifies file type (f=file, d=directory, l=symlink, etc.)
- `-size` searches by file size (use `+` for greater than, `-` for less than)
- `-mtime` searches by modification time (days)
- `-atime` searches by access time (days)
- `-ctime` searches by change time (days)
- `-newer` searches for files newer than a reference file
- `-exec` executes a command on each found file
- `-delete` deletes found files
- `-print` prints the found files (default action)

### Time Specifications

- `-mtime +7` means "more than 7 days old"
- `-mtime -2` means "less than 2 days old"
- `-mtime 1` means "exactly 1 day old"
- `-mmin` works the same way but with minutes instead of days
- For more precise time specs, use `-newermt "YYYY-MM-DD HH:MM:SS"`

### find vs. locate

- `find` searches the actual filesystem, which is slower but always up-to-date
- `locate` searches a database of filenames, which is faster but may be outdated
- `locate` requires the database to be updated periodically (usually by cron)
- `updatedb` manually updates the locate database

### xargs vs. exec

- `find -exec cmd {} \;` runs the command once for each file
- `find -exec cmd {} \+` runs the command with as many files as possible at once
- `find | xargs cmd` is similar to `find -exec cmd {} \+` but more flexible
- `xargs -I{}` allows using the placeholder anywhere in the command, not just at the end
- `xargs -P N` allows processing files in parallel with N processes

### Advanced find Options

- `-mindepth N` skip subdirectories less than N levels deep
- `-maxdepth N` don't descend more than N levels deep
- `-not` or `!` negates a condition
- `-a` (AND) and `-o` (OR) combine conditions logically
- `-path` searches by path pattern
- `-regex` searches by regular expression
- `-perm` searches by file permissions
