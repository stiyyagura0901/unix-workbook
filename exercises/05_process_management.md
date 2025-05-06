 # Exercise 5: Process Management

## Concepts Covered
- Process monitoring
- Foreground and background processes
- Process control
- Job management
- System monitoring

## Commands to Learn
- `ps` - Report process status
- `top`/`htop` - Interactive process viewers
- `kill` - Terminate processes
- `bg` - Put processes in background
- `fg` - Bring processes to foreground
- `jobs` - List current jobs
- `&` - Run a command in the background
- `nohup` - Run a command immune to hangups

## Exercises

### 5.1: Examining Running Processes

1. Use the `ps` command to list your own processes
2. Use `ps aux` to list all processes on the system
3. Filter the output to show only processes owned by you (hint: combine with `grep`)
4. Use `top` to view processes in real-time (exit with 'q')
5. If available, try `htop` and explore its interface

### 5.2: Working with Foreground and Background Jobs

1. Run the command `sleep 60` in your terminal - this will pause for 60 seconds
2. While it's running, press Ctrl+Z to suspend it
3. Use the `jobs` command to verify the job is suspended
4. Use `bg` to continue running it in the background
5. Run another sleep command in the background directly: `sleep 120 &`
6. Use `jobs` to see both background jobs
7. Use `fg` to bring one of the jobs to the foreground
8. Press Ctrl+C to terminate it
9. Check `jobs` again to see what's still running

### 5.3: Process Control

1. Start a new background job: `sleep 300 &`
2. Find its Process ID (PID) using `jobs -l` or `ps`
3. Use `kill` to terminate this process using its PID
4. Start several sleep processes in the background
5. Kill all of them at once using their process name (hint: `pkill` or `killall`)

### 5.4: System Monitoring

1. Use `top` to answer these questions:
   - What process is using the most CPU?
   - What process is using the most memory?
   - How many processes are running on your system?
   - What is the system load average?

2. Learn a few keyboard commands within `top`:
   - Sort by CPU (press P)
   - Sort by memory (press M)
   - Kill a process (press k, then enter PID)
   - Change update interval (press d, then enter number of seconds)

### 5.5: Challenge - Process Monitoring Script

1. Create a script called `monitor.sh` with the following content:

```bash
#!/bin/bash
# Simple process monitoring script

echo "=== System Information ==="
date
echo "Uptime:"
uptime
echo ""

echo "=== CPU Usage ==="
ps aux | sort -nrk 3,3 | head -5
echo ""

echo "=== Memory Usage ==="
ps aux | sort -nrk 4,4 | head -5
echo ""

echo "=== Disk Usage ==="
df -h
```

2. Make the script executable
3. Run it in the background, redirecting output to a file: `./monitor.sh > system_status.txt &`
4. Set up the script to run every minute for 5 minutes (hint: use a loop or research `watch` command)
5. Find a way to keep the script running even after you log out (research `nohup` or `screen`/`tmux`)

## Reflection Questions
- Why is process management important in a Unix/Linux system?
- What's the difference between suspending and terminating a process?
- When would you want to run a process in the background versus the foreground?
- How could you use what you've learned to troubleshoot a system that's running slowly?
