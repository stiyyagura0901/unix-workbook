 # Exercise 7: Pipes and Redirections

## Concepts Covered
- Redirecting output and input
- Piping commands together
- Standard input, output, and error
- Command chaining
- Text processing pipelines

## Commands and Concepts to Learn
- `>` - Redirect output to a file (overwrite)
- `>>` - Redirect output to a file (append)
- `<` - Redirect input from a file
- `|` - Pipe output of one command as input to another
- `2>` - Redirect error output
- `&>` - Redirect both standard output and error
- `tee` - Read from stdin and write to stdout and files
- Command chaining with `;`, `&&`, and `||`

## Exercises

### 7.1: Basic Output Redirection

1. Use the `echo` command to write "Hello, Unix!" to a file called `greeting.txt`
2. Use the `cat` command to display the contents of the file
3. Use `echo` again to write a different message to the same file. What happens to the original content?
4. Now use the append operator (`>>`) to add another line to the file without erasing the current content
5. Use the `date` command to append the current date and time to the file

### 7.2: Input Redirection

1. Create a file called `names.txt` with the following content:
   ```
   Alice
   Bob
   Charlie
   David
   Emma
   ```

2. Use the `sort` command with input redirection to sort the names
3. Use the `wc` command with input redirection to count the lines, words, and characters in the file
4. Create a command that reads from `names.txt` and converts all text to uppercase (hint: use the `tr` command with input redirection)

### 7.3: Using Pipes

1. List all files in your home directory and pipe the output to `grep` to find only files with ".txt" in their names
2. Run `ps aux` and pipe to `grep` to find processes containing a specific word (e.g., "bash")
3. Use the `cat` command on a text file and pipe to `head` to see only the first 5 lines
4. Create a pipeline using at least three commands: list files, grep for a pattern, and sort the results

### 7.4: Handling Standard Error

1. Run a command that will generate an error, like `ls /nonexistentdirectory`
2. Run the same command but redirect the error message to a file called `errors.log`
3. Run a command that produces both normal output and error output, and redirect them to separate files
4. Run a command that produces both normal output and error output, and redirect both to the same file

### 7.5: Combining Redirections and Pipes

1. Create a large text file by running:
   ```bash
   for i in {1..100}; do echo "Line $i: $(date) - Random number: $RANDOM" >> large_file.txt; done
   ```

2. Write a single command that:
   - Searches for lines containing the word "Random" in the file
   - Sorts them numerically based on the random number
   - Takes only the first 10 results
   - Saves the output to a file called `top_randoms.txt`

3. Use the `tee` command to display the output of a command on the screen AND save it to a file at the same time

### 7.6: Command Chaining

1. Use the semicolon (`;`) to run three commands in sequence regardless of whether they succeed or fail
2. Use `&&` to run a second command only if the first one succeeds
3. Use `||` to run a second command only if the first one fails
4. Create a complex chain that uses both `&&` and `||` to implement an if-else logic

### 7.7: Challenge - Log Analysis Pipeline

You're given a sample log file (`server.log`) with the following format:
```
[2023-03-15 08:23:45] INFO: User alice logged in from 192.168.1.5
[2023-03-15 08:25:12] ERROR: Database connection failed
[2023-03-15 08:25:30] INFO: User bob logged in from 10.0.0.2
[2023-03-15 08:26:45] WARNING: High CPU usage detected (85%)
[2023-03-15 08:30:15] INFO: Backup process started
[2023-03-15 08:35:23] ERROR: Disk space low on /dev/sda1
```

Your task is to create a series of command pipelines that:

1. Extract and count the different types of log entries (INFO, ERROR, WARNING)
2. Find all login events and extract just the usernames, saving them to `users.txt`
3. Sort the log by timestamp and find the most recent error
4. Create a summary report showing counts of each type of event, saved to `log_summary.txt`

## Reflection Questions
- How do pipes help with the Unix philosophy of "doing one thing well"?
- When would you use output redirection versus pipes?
- What are the advantages of using command chaining?
- How could these techniques help you automate a complex task in your daily work?
