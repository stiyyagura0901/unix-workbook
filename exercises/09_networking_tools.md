 # Exercise 9: Networking Tools

## Concepts Covered
- Network connectivity
- Remote access
- File transfer
- Network diagnostics
- Service monitoring

## Commands to Learn
- `ping` - Test network connectivity
- `ssh` - Secure Shell remote access
- `scp`/`rsync` - Secure file transfer
- `curl`/`wget` - Download files and make web requests
- `netstat`/`ss` - Network statistics
- `dig`/`nslookup` - DNS lookup
- `ip`/`ifconfig` - Network interface configuration
- `traceroute` - Trace network path

## Exercises

### 9.1: Basic Network Connectivity

1. Use `ping` to check if you can reach a popular website (e.g., google.com)
2. Observe the round-trip time (RTT) for packets
3. Try pinging a local IP address if you know one on your network
4. Use ping with the count option to send exactly 5 packets
5. What happens when you ping an invalid hostname?

### 9.2: Network Interface Information

1. Use the appropriate command (`ip addr`, `ifconfig`, or similar) to view your network interfaces
2. Identify your primary interface (typically eth0, en0, or wlan0)
3. Note your IP address, netmask, and MAC address
4. Check which network ports are currently in use with `netstat -tuln` or `ss -tuln`
5. Identify which programs are using specific network ports

### 9.3: DNS Lookups

1. Use `dig` or `nslookup` to find the IP address of a popular website
2. Look up the mail exchange (MX) records for a domain
3. Find the authoritative nameservers for a domain
4. Perform a reverse DNS lookup on an IP address
5. Compare the results of different DNS servers (e.g., Google's 8.8.8.8 vs. Cloudflare's 1.1.1.1)

### 9.4: Downloading Files and Making Web Requests

1. Use `curl` to download a web page and display it in the terminal
2. Save the result of a web request to a file
3. Use `wget` to download a file
4. Make a POST request to a test API endpoint
5. Use curl to check HTTP headers only
6. Download a file while showing a progress bar

### 9.5: Tracing Network Paths

1. Use `traceroute` (or `tracepath`) to a popular website
2. Count how many hops it takes to reach the destination
3. Identify where packet loss or timeouts occur
4. Compare the path to different destinations
5. Try running a traceroute to an international website and note differences

### 9.6: Remote Access with SSH (if you have access to an SSH server)

1. Connect to an SSH server using `ssh username@hostname`
2. Run a few commands on the remote system
3. Create a file on the remote system
4. Exit the SSH session
5. Use SSH to run a single command on the remote system without staying connected

### 9.7: Secure File Transfer (if you have access to an SSH server)

1. Use `scp` to copy a local file to a remote system
2. Use `scp` to download a file from the remote system to your local machine
3. Try `rsync` to synchronize a directory with a remote location
4. Use rsync's dry-run option to see what would be transferred without actually doing it
5. Transfer a directory and all its contents

### 9.8: Challenge - Network Monitoring Script

Create a script called `network_monitor.sh` that:

1. Checks connectivity to several important hosts (e.g., your default gateway, DNS servers, and key websites)
2. Reports on current network interface status
3. Lists active network connections
4. Performs a speed test (you can use a simple download or ping test)
5. Alerts if any critical services are unreachable
6. Runs continuously, checking at regular intervals
7. Logs results to a file with timestamps

## Reflection Questions
- How can network tools help diagnose connectivity issues?
- What security concerns should you be aware of when using remote access tools?
- What are the differences between `scp` and `rsync`, and when would you prefer one over the other?
- How could you use these tools to automate routine network maintenance tasks?
