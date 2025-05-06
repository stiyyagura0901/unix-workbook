# Exercise 5: Process Management - Solutions

## 5.1: Examining Running Processes

```bash
# List your own processes
ps

# List all processes on the system
ps aux

# Filter to show only your processes
ps aux | grep $USER

# View processes in real-time
top
# (press q to exit)

# If available, try htop
htop
# (press q to exit)
```

## 5.2: Working with Foreground and Background Jobs

```bash
# Run sleep for 60 seconds
sleep 60

# Press Ctrl+Z to suspend it
# Output: [1]+ Stopped    sleep 60

# Check job status
jobs
# Output should show the suspended sleep job

# Continue in background
bg
# Output: [1]+ sleep 60 &

# Run another sleep in background
sleep 120 &
# Output: [2] 12345 (where 12345 is the PID)

# List background jobs
jobs
# Should show both sleep jobs

# Bring first job to foreground
fg %1
# Or without the job number, which brings the most recent job
# fg

# Press Ctrl+C to terminate it

# Check jobs again
jobs
# Should show only the second sleep job
```

## 5.3: Process Control

```bash
# Start a background job
sleep 300 &
# Output: [1] 12345 (where 12345 is the PID)

# Find its PID
jobs -l
# Or use ps
ps | grep sleep

# Kill the process
kill 12345
# (replace 12345 with the actual PID)

# Start several sleep processes
sleep 100 & sleep 200 & sleep 300 &

# Kill all of them at once
pkill sleep
# Or
killall sleep
```

## 5.4: System Monitoring

```bash
# Run top to answer questions
top

# In top, press:
# - P to sort by CPU
# - M to sort by memory
# - k to kill a process (then enter PID)
# - d to change update interval
# - q to quit
```

Questions answers:

- The process using most CPU will be at the top when sorted by CPU (%CPU column)
- The process using most memory will be at the top when sorted by memory (%MEM column)
- The number of processes running is shown in the header ("%d running" in the tasks line)
- Load average is shown in the header (load average: x.xx, x.xx, x.xx)

## 5.5: Challenge - Process Monitoring Script

```bash
# Create monitoring script
cat > monitor.sh << 'EOF'
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
EOF

# Make executable
chmod +x monitor.sh

# Run in background with output to file
./monitor.sh > system_status.txt &

# Run every minute for 5 minutes using a loop
(
  for i in {1..5}; do
    ./monitor.sh >> system_status_loop.txt
    echo "=== Run $i completed ===" >> system_status_loop.txt
    sleep 60
  done
) &

# Keep script running after logout
nohup ./monitor.sh > system_status_persistent.txt &
```

To keep a script running even after logout:

1. Using `nohup` as shown above
2. Using `screen` or `tmux`:

   ```bash
   # Install if needed (package manager dependent)
   # sudo apt install screen  # For Debian/Ubuntu
   # sudo yum install screen  # For CentOS/RHEL

   # Start a screen session
   screen
   # Run your command
   ./monitor.sh > output.txt
   # Detach from screen with Ctrl+A then D
   # Later reattach with:
   screen -r
   ```

## Additional Notes

- Process states: R (running), S (sleeping), D (waiting on I/O), T (stopped), Z (zombie)
- Every process has a unique PID (Process ID)
- Signal types:
  - SIGTERM (15): Default for kill, allows graceful termination
  - SIGKILL (9): Force termination, cannot be caught or ignored
  - SIGINT (2): Interrupt signal (Ctrl+C)
  - SIGHUP (1): Hangup signal, often used to reload configuration
- Job control (bg, fg, jobs) only works within the same shell session
- For persistent background processes, use nohup, screen, tmux, or system services
