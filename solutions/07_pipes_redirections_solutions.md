# Exercise 7: Pipes and Redirections - Solutions

## 7.1: Basic Output Redirection

```bash
# Write "Hello, Unix!" to greeting.txt
echo "Hello, Unix!" > greeting.txt

# Display contents
cat greeting.txt

# Write a different message (overwriting the file)
echo "New message" > greeting.txt
# The original content is now gone

# Append another line
echo "This is a second line" >> greeting.txt

# Append current date and time
date >> greeting.txt

# Check the final content
cat greeting.txt
```

## 7.2: Input Redirection

```bash
# Create names.txt
cat > names.txt << 'EOF'
Alice
Bob
Charlie
David
Emma
EOF

# Sort names using input redirection
sort < names.txt

# Count lines, words, and characters
wc < names.txt
# Output will be something like: 5 5 29

# Convert to uppercase using tr
tr 'a-z' 'A-Z' < names.txt
# Output will be uppercase versions of all names
```

## 7.3: Using Pipes

```bash
# List files in home directory and find .txt files
ls ~ | grep ".txt"

# Find bash processes
ps aux | grep "bash"

# View first 5 lines of a file
cat names.txt | head -5
# Or more efficiently:
head -5 names.txt

# Pipeline with three commands
# List files, filter for .txt files, sort them
ls | grep ".txt" | sort
```

## 7.4: Handling Standard Error

```bash
# Generate an error
ls /nonexistentdirectory
# You'll see an error message

# Redirect error to a file
ls /nonexistentdirectory 2> errors.log

# Redirect normal output and error to separate files
ls /existingdirectory /nonexistentdirectory > output.log 2> errors.log

# Redirect both to the same file
ls /existingdirectory /nonexistentdirectory > all.log 2>&1
# Or in newer shells:
ls /existingdirectory /nonexistentdirectory &> all.log
```

## 7.5: Combining Redirections and Pipes

```bash
# Create a large file
for i in {1..100}; do echo "Line $i: $(date) - Random number: $RANDOM" >> large_file.txt; done

# Complex command with pipe and redirection
grep "Random" large_file.txt | sort -n -k5 | head -10 > top_randoms.txt

# Using tee to display and save output
ls -la | tee file_listing.txt
```

## 7.6: Command Chaining

```bash
# Run commands in sequence with ;
echo "First command" ; echo "Second command" ; echo "Third command"

# Run second command only if first succeeds
cd /existing_directory && echo "Directory exists"

# Run second command only if first fails
cd /nonexistent_directory || echo "Directory does not exist"

# Complex chain with both && and ||
cd /existing_directory && echo "Success" || echo "Failure"
```

## 7.7: Challenge - Log Analysis Pipeline

Given the sample log file `server.log`:

```bash
# 1. Extract and count different types of log entries
grep -o "\] \w*:" server.log | sort | uniq -c
# Output will look like:
# 10 ] INFO:
#  5 ] ERROR:
#  5 ] WARNING:

# 2. Find login events and extract usernames
grep "User .* logged in" server.log | grep -o "User \w*" | cut -d ' ' -f2 > users.txt

# 3. Sort logs by timestamp and find most recent error
sort server.log | grep "ERROR"

# 4. Create summary report
echo "Log Summary Report" > log_summary.txt
echo "===================" >> log_summary.txt
echo "" >> log_summary.txt
echo "Total entries: $(wc -l < server.log)" >> log_summary.txt
echo "INFO messages: $(grep -c "INFO:" server.log)" >> log_summary.txt
echo "ERROR messages: $(grep -c "ERROR:" server.log)" >> log_summary.txt
echo "WARNING messages: $(grep -c "WARNING:" server.log)" >> log_summary.txt
echo "" >> log_summary.txt
echo "Unique users logged in: $(grep -c "logged in" server.log)" >> log_summary.txt
echo "Most recent error: $(grep "ERROR:" server.log | tail -1)" >> log_summary.txt
```

## Additional Notes

- Redirection operators:

  - `>` redirects stdout to a file (overwriting)
  - `>>` redirects stdout to a file (appending)
  - `<` redirects stdin from a file
  - `2>` redirects stderr to a file
  - `2>&1` redirects stderr to wherever stdout is going
  - `&>` redirects both stdout and stderr to a file

- Pipe (`|`) connects the stdout of one command to the stdin of another
- Command separators and conditional execution:

  - `;` runs commands sequentially
  - `&&` runs the second command only if the first succeeds
  - `||` runs the second command only if the first fails

- `tee` is useful when you want to both see the output and save it to a file

- These techniques form the foundation of text processing pipelines and shell scripting in Unix
