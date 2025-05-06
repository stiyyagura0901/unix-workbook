# Exercise 6: Shell Scripting Basics - Solutions

## 6.1: Your First Script

```bash
# Create the script
cat > hello.sh << 'EOF'
#!/bin/bash

echo "Hello, world!"
echo "Today is $(date)"
echo "Your current working directory is: $(pwd)"
EOF

# Make it executable
chmod +x hello.sh

# Run it
./hello.sh

# Modified script with username
cat > hello.sh << 'EOF'
#!/bin/bash

echo "Hello, world!"
echo "Today is $(date)"
echo "Your current working directory is: $(pwd)"
echo "You are logged in as $(whoami)"
EOF

# Run it again
./hello.sh
```

## 6.2: Working with Variables

```bash
# Create the script
cat > variables.sh << 'EOF'
#!/bin/bash

# Assigning variables
name="Unix Student"
courses=5
today=$(date +%Y-%m-%d)

# Using variables
echo "Hello, $name!"
echo "You are taking $courses courses."
echo "Today's date is $today."

# Environment variables
echo "Your home directory is: $HOME"
echo "Your path is: $PATH"
EOF

# Make it executable
chmod +x variables.sh

# Run it
./variables.sh

# Modified script to calculate days left in the year
cat > variables.sh << 'EOF'
#!/bin/bash

# Assigning variables
name="Unix Student"
courses=5
today=$(date +%Y-%m-%d)

# Calculate days left in the year
year=$(date +%Y)
year_end="${year}-12-31"
today_seconds=$(date -d "$today" +%s)
year_end_seconds=$(date -d "$year_end" +%s)
seconds_left=$((year_end_seconds - today_seconds))
days_left=$((seconds_left / 86400))

# Using variables
echo "Hello, $name!"
echo "You are taking $courses courses."
echo "Today's date is $today."
echo "There are $days_left days left in the year."

# Environment variables
echo "Your home directory is: $HOME"
echo "Your path is: $PATH"
EOF

# Alternative for macOS which doesn't support date -d
cat > variables.sh << 'EOF'
#!/bin/bash

# Assigning variables
name="Unix Student"
courses=5
today=$(date +%Y-%m-%d)

# Calculate days left in the year
current_day_of_year=$(date +%j)
if [ $(date +%Y) -eq $(date -v12-31-23:59:59 +%Y) ]; then
    # Not a leap year
    days_in_year=365
else
    # Leap year
    days_in_year=366
fi
days_left=$((days_in_year - current_day_of_year))

# Using variables
echo "Hello, $name!"
echo "You are taking $courses courses."
echo "Today's date is $today."
echo "There are $days_left days left in the year."

# Environment variables
echo "Your home directory is: $HOME"
echo "Your path is: $PATH"
EOF

# Run the modified script
./variables.sh
```

## 6.3: Control Structures - Decision Making

```bash
# Create the script
cat > decisions.sh << 'EOF'
#!/bin/bash

# Get current hour
hour=$(date +%H)

# Greet based on time of day
if [ $hour -lt 12 ]; then
    echo "Good morning!"
elif [ $hour -lt 18 ]; then
    echo "Good afternoon!"
else
    echo "Good evening!"
fi

# Check if a file exists
echo -n "Enter a filename: "
read filename

if [ -f "$filename" ]; then
    echo "$filename exists and is a regular file."
    echo "It contains $(wc -l < "$filename") lines."
elif [ -d "$filename" ]; then
    echo "$filename exists and is a directory."
    echo "It contains $(ls -1 "$filename" | wc -l) items."
else
    echo "$filename does not exist."
fi
EOF

# Make it executable
chmod +x decisions.sh

# Run it
./decisions.sh

# Modified script to check file permissions
cat > decisions.sh << 'EOF'
#!/bin/bash

# Get current hour
hour=$(date +%H)

# Greet based on time of day
if [ $hour -lt 12 ]; then
    echo "Good morning!"
elif [ $hour -lt 18 ]; then
    echo "Good afternoon!"
else
    echo "Good evening!"
fi

# Check if a file exists
echo -n "Enter a filename: "
read filename

if [ -f "$filename" ]; then
    echo "$filename exists and is a regular file."
    echo "It contains $(wc -l < "$filename") lines."

    # Check permissions
    if [ -r "$filename" ]; then
        echo "The file is readable."
    else
        echo "The file is NOT readable."
    fi

    if [ -w "$filename" ]; then
        echo "The file is writable."
    else
        echo "The file is NOT writable."
    fi

    if [ -x "$filename" ]; then
        echo "The file is executable."
    else
        echo "The file is NOT executable."
    fi

elif [ -d "$filename" ]; then
    echo "$filename exists and is a directory."
    echo "It contains $(ls -1 "$filename" | wc -l) items."

    # Check directory permissions
    if [ -r "$filename" ]; then
        echo "The directory is readable."
    else
        echo "The directory is NOT readable."
    fi

    if [ -w "$filename" ]; then
        echo "The directory is writable."
    else
        echo "The directory is NOT writable."
    fi

    if [ -x "$filename" ]; then
        echo "The directory is executable (you can cd into it)."
    else
        echo "The directory is NOT executable (you cannot cd into it)."
    fi

else
    echo "$filename does not exist."
fi
EOF

# Run the modified script
./decisions.sh
```

## 6.4: Control Structures - Loops

```bash
# Create the script
cat > loops.sh << 'EOF'
#!/bin/bash

# For loop example
echo "Counting from 1 to 5:"
for i in {1..5}; do
    echo "Number: $i"
done

# While loop example
echo -e "\nCountdown:"
count=5
while [ $count -gt 0 ]; do
    echo "$count..."
    count=$((count - 1))
    sleep 1
done
echo "Blast off!"

# Looping through files
echo -e "\nText files in current directory:"
for file in *.txt; do
    if [ -f "$file" ]; then
        echo "- $file ($(wc -l < "$file") lines)"
    fi
done
EOF

# Make it executable
chmod +x loops.sh

# Run it
./loops.sh

# Modified script to ask for names
cat > loops.sh << 'EOF'
#!/bin/bash

# For loop example
echo "Counting from 1 to 5:"
for i in {1..5}; do
    echo "Number: $i"
done

# While loop example
echo -e "\nCountdown:"
count=5
while [ $count -gt 0 ]; do
    echo "$count..."
    count=$((count - 1))
    sleep 1
done
echo "Blast off!"

# Looping through files
echo -e "\nText files in current directory:"
for file in *.txt; do
    if [ -f "$file" ]; then
        echo "- $file ($(wc -l < "$file") lines)"
    fi
done

# Interactive name input loop
echo -e "\nName greeter:"
while true; do
    echo -n "Enter a name (or 'quit' to exit): "
    read name

    if [ "$name" = "quit" ]; then
        echo "Goodbye!"
        break
    fi

    echo "Hello, $name! Nice to meet you."
done
EOF

# Run the modified script
./loops.sh
```

## 6.5: Functions and Arguments

```bash
# Create the script
cat > functions.sh << 'EOF'
#!/bin/bash

# A simple function
greet() {
    echo "Hello, $1!"
}

# Function with return value
is_even() {
    if [ $(($1 % 2)) -eq 0 ]; then
        return 0  # True in bash
    else
        return 1  # False in bash
    fi
}

# Call the greeting function
greet "Student"

# Test the is_even function
echo -n "Enter a number: "
read num

if is_even $num; then
    echo "$num is even."
else
    echo "$num is odd."
fi

# Show argument handling
echo -e "\nScript arguments:"
echo "Number of arguments: $#"
echo "All arguments: $@"
echo "First argument: $1"
echo "Second argument: $2"
EOF

# Make it executable
chmod +x functions.sh

# Run the script with arguments
./functions.sh arg1 arg2 arg3

# Modified script with file_info function
cat > functions.sh << 'EOF'
#!/bin/bash

# A simple function
greet() {
    echo "Hello, $1!"
}

# Function with return value
is_even() {
    if [ $(($1 % 2)) -eq 0 ]; then
        return 0  # True in bash
    else
        return 1  # False in bash
    fi
}

# New file_info function
file_info() {
    if [ ! -e "$1" ]; then
        echo "Error: File '$1' does not exist."
        return 1
    fi

    echo "File information for: $1"
    echo "------------------------"

    # File type
    if [ -f "$1" ]; then
        echo "Type: Regular file"
    elif [ -d "$1" ]; then
        echo "Type: Directory"
    elif [ -L "$1" ]; then
        echo "Type: Symbolic link"
    else
        echo "Type: Special file"
    fi

    # File size
    echo "Size: $(du -h "$1" | cut -f1) ($(stat -f %z "$1") bytes)"

    # Permissions
    echo "Permissions: $(stat -f %Sp "$1") ($(stat -f %Op "$1" | cut -c4-6))"

    # Owner and group
    echo "Owner: $(stat -f %Su "$1")"
    echo "Group: $(stat -f %Sg "$1")"

    # Timestamps
    echo "Last modified: $(stat -f %Sm "$1")"
    echo "Last accessed: $(stat -f %Sa "$1")"

    return 0
}

# Call the greeting function
greet "Student"

# Test the is_even function
echo -n "Enter a number: "
read num

if is_even $num; then
    echo "$num is even."
else
    echo "$num is odd."
fi

# Test file_info function
echo -e "\nFile information:"
echo -n "Enter a filename: "
read filename
file_info "$filename"

# Show argument handling
echo -e "\nScript arguments:"
echo "Number of arguments: $#"
echo "All arguments: $@"
echo "First argument: $1"
echo "Second argument: $2"
EOF

# Run the modified script with arguments
./functions.sh arg1 arg2 arg3
```

## 6.6: Challenge - File Organization Script

```bash
# Create the organization script
cat > organize.sh << 'EOF'
#!/bin/bash

# File Organization Script

# Display help information
show_help() {
    echo "Usage: $0 [OPTIONS] [DIRECTORY]"
    echo "Organize files into subdirectories based on their file types."
    echo ""
    echo "Options:"
    echo "  -h, --help    Show this help message and exit"
    echo ""
    echo "If no directory is provided, the current directory will be used."
    exit 0
}

# Process command line arguments
TARGET_DIR="."

if [ $# -gt 0 ]; then
    if [ "$1" = "-h" ] || [ "$1" = "--help" ]; then
        show_help
    else
        TARGET_DIR="$1"
    fi
fi

# Verify target directory exists
if [ ! -d "$TARGET_DIR" ]; then
    echo "Error: Directory '$TARGET_DIR' does not exist."
    exit 1
fi

echo "Organizing files in: $TARGET_DIR"

# Create subdirectories if they don't exist
mkdir -p "$TARGET_DIR/images"
mkdir -p "$TARGET_DIR/documents"
mkdir -p "$TARGET_DIR/scripts"
mkdir -p "$TARGET_DIR/archives"
mkdir -p "$TARGET_DIR/others"

# Move files to appropriate directories
# Initialize counters
images_count=0
documents_count=0
scripts_count=0
archives_count=0
others_count=0

# Function to move a file and increment the appropriate counter
move_file() {
    local file="$1"
    local dest="$2"
    local counter_var="$3"

    # Don't move the script itself!
    if [ "$(basename "$file")" = "$(basename "$0")" ]; then
        return
    fi

    # Move the file
    mv "$file" "$dest/"

    # Increment the counter (indirectly using eval)
    eval "$counter_var=$((${!counter_var} + 1))"
}

# Process each file in the target directory
for file in "$TARGET_DIR"/*; do
    # Skip directories and non-existing files
    if [ ! -f "$file" ]; then
        continue
    fi

    # Extract lowercase file extension
    filename=$(basename "$file")
    extension="${filename##*.}"
    extension_lower=$(echo "$extension" | tr '[:upper:]' '[:lower:]')

    # Categorize by file extension
    case "$extension_lower" in
        # Images
        jpg|jpeg|png|gif|bmp|svg|tiff|webp)
            move_file "$file" "$TARGET_DIR/images" "images_count"
            ;;

        # Documents
        pdf|doc|docx|txt|md|csv|xls|xlsx|ppt|pptx|odt|ods|odp)
            move_file "$file" "$TARGET_DIR/documents" "documents_count"
            ;;

        # Scripts
        sh|bash|py|pl|rb|js|php)
            move_file "$file" "$TARGET_DIR/scripts" "scripts_count"
            ;;

        # Archives
        zip|tar|gz|bz2|xz|7z|rar)
            move_file "$file" "$TARGET_DIR/archives" "archives_count"
            ;;

        # Others
        *)
            move_file "$file" "$TARGET_DIR/others" "others_count"
            ;;
    esac
done

# Print summary
echo ""
echo "Organization Summary:"
echo "---------------------"
echo "Images:    $images_count"
echo "Documents: $documents_count"
echo "Scripts:   $scripts_count"
echo "Archives:  $archives_count"
echo "Others:    $others_count"
echo "---------------------"
echo "Total:     $((images_count + documents_count + scripts_count + archives_count + others_count))"

exit 0
EOF

# Make it executable
chmod +x organize.sh

# Run the script (in the current directory)
./organize.sh

# Run with a different directory
./organize.sh ~/test_directory

# Get help information
./organize.sh --help
```

## Additional Notes

- Shell scripting allows automation of repetitive tasks
- Common shell script file extensions: `.sh`, `.bash`
- The shebang line (`#!/bin/bash`) tells the system which interpreter to use
- Make scripts executable with `chmod +x script.sh`
- Control structures include:
  - `if/elif/else` for decision making
  - `for` loops for iteration through lists
  - `while` loops for condition-based iteration
  - `case` statements for multiple conditions
- Variables don't need to be declared before use
- Parameter expansion: `$variable`, `${variable}`
- Command substitution: `$(command)` or \`command\`
- Exit codes: 0 means success, non-zero means failure
- Functions help organize code and make it reusable
