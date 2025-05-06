 # Exercise 6: Shell Scripting Basics

## Concepts Covered
- Creating and running shell scripts
- Variables and environment variables
- Control structures (if/else, loops)
- Command substitution
- Functions
- Script arguments

## Commands and Concepts to Learn
- `#!/bin/bash` shebang line
- Script execution (`./<script>` vs `bash <script>`)
- Variables and parameter expansion
- `if`, `for`, `while` statements
- `read` command for user input
- `$?`, `$1`, `$@` special parameters
- Script exit codes

## Exercises

### 6.1: Your First Script

1. Create a new file called `hello.sh` with the following content:
   ```bash
   #!/bin/bash
   
   echo "Hello, world!"
   echo "Today is $(date)"
   echo "Your current working directory is: $(pwd)"
   ```

2. Make the script executable with `chmod +x hello.sh`
3. Run it with `./hello.sh`
4. Modify the script to include your username using command substitution with the `whoami` command

### 6.2: Working with Variables

1. Create a script called `variables.sh` with the following content:
   ```bash
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
   ```

2. Run the script
3. Modify it to calculate and display how many days are left in the current year

### 6.3: Control Structures - Decision Making

1. Create a script called `decisions.sh` with the following content:
   ```bash
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
   ```

2. Run the script and test with different filenames
3. Modify the script to also check file permissions (whether it's readable, writable, or executable)

### 6.4: Control Structures - Loops

1. Create a script called `loops.sh` with the following content:
   ```bash
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
   ```

2. Run the script
3. Modify it to include a loop that asks the user for names and greets each person, until the user enters "quit"

### 6.5: Functions and Arguments

1. Create a script called `functions.sh` with the following content:
   ```bash
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
   ```

2. Run the script with different arguments: `./functions.sh arg1 arg2 arg3`
3. Create a new function called `file_info` that takes a filename as an argument and prints its type, size, and permissions

### 6.6: Challenge - File Organization Script

Create a script called `organize.sh` that:

1. Takes a directory as an argument (defaulting to the current directory if none provided)
2. Creates subdirectories for different file types: `images`, `documents`, `scripts`, `archives`, `others`
3. Moves files into these directories based on their extensions:
   - Images: jpg, jpeg, png, gif, bmp
   - Documents: pdf, doc, docx, txt, md, csv
   - Scripts: sh, bash, py, pl, rb
   - Archives: zip, tar, gz, bz2, xz
   - Others: everything else
4. Prints a summary of how many files were moved to each directory
5. Includes error handling for invalid directories
6. Has a help option (-h or --help) that explains how to use the script

## Reflection Questions
- How do shell scripts help with automation?
- What are the advantages of using functions in your scripts?
- When would you use command substitution versus variable assignment?
- How can you make your scripts more robust with error handling?
