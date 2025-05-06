# Exercise 9: Networking Tools - Solutions

## 9.1: Basic Network Connectivity

```bash
# Check connectivity to a website
ping google.com
# Output will show round-trip times and packet statistics

# Ping with a specific count of 5 packets
ping -c 5 google.com

# Ping a local IP (example)
ping 192.168.1.1

# Ping an invalid hostname
ping nonexistentwebsite.invalidtld
# This will typically fail with an error about unknown host
```

## 9.2: Network Interface Information

```bash
# View network interfaces
# On most Linux systems
ip addr
# Or on some systems including macOS
ifconfig

# Check network ports in use
# On Linux
netstat -tuln
# Or
ss -tuln
# On macOS
netstat -an | grep LISTEN

# Find which program is using a specific port (example for port 80)
# Linux
lsof -i :80
# macOS
lsof -i tcp:80
```

## 9.3: DNS Lookups

```bash
# Find IP address of a website
dig google.com
# Or
nslookup google.com

# Look up mail exchange records
dig google.com MX

# Find authoritative nameservers
dig google.com NS

# Perform reverse DNS lookup
dig -x 8.8.8.8
# Or
nslookup 8.8.8.8

# Compare results from different DNS servers
dig @8.8.8.8 google.com
dig @1.1.1.1 google.com
```

## 9.4: Downloading Files and Making Web Requests

```bash
# Download and display a web page
curl https://example.com

# Save result to a file
curl -o output.html https://example.com

# Use wget to download a file
wget https://example.com/file.zip

# Make a POST request
curl -X POST -d "param1=value1&param2=value2" https://example.com/api

# Check HTTP headers only
curl -I https://example.com

# Download with progress bar
wget -q --show-progress https://example.com/largefile.zip
# Or with curl
curl -# -O https://example.com/largefile.zip
```

## 9.5: Tracing Network Paths

```bash
# Trace route to a website
traceroute google.com
# On some systems, you may need to use
tracepath google.com

# Count hops to destination
# You can count the number of lines in the traceroute output, each representing a hop

# Compare paths to different destinations
traceroute google.com
traceroute facebook.com

# International website trace (example)
traceroute bbc.co.uk
```

## 9.6: Remote Access with SSH

```bash
# Connect to an SSH server
ssh username@hostname

# Run commands on the remote system
# While connected via SSH:
ls -la
pwd
whoami

# Create a file on the remote system
echo "Hello from remote" > remote_file.txt

# Exit SSH session
exit
# Or press Ctrl+D

# Run a single command on remote system without staying connected
ssh username@hostname "ls -la"
```

## 9.7: Secure File Transfer

```bash
# Copy a local file to remote system
scp localfile.txt username@hostname:/path/to/destination/

# Download a file from remote system
scp username@hostname:/path/to/remotefile.txt localfile.txt

# Use rsync to synchronize a directory
rsync -avz local_directory/ username@hostname:/path/to/remote_directory/

# Rsync dry-run (show what would be transferred)
rsync -avzn local_directory/ username@hostname:/path/to/remote_directory/

# Transfer a directory and all contents
scp -r local_directory/ username@hostname:/path/to/destination/
```

## 9.8: Challenge - Network Monitoring Script

```bash
#!/bin/bash
# network_monitor.sh - Simple network monitoring script

# Log file
LOG_FILE="network_monitor.log"

# Hosts to check (customize these)
GATEWAY=$(ip route | grep default | awk '{print $3}')
DNS_SERVER1="8.8.8.8"  # Google DNS
DNS_SERVER2="1.1.1.1"  # Cloudflare DNS
WEBSITES=("google.com" "github.com" "example.com")

# Function to check if a host is reachable
check_host() {
    host=$1
    if ping -c 1 -W 2 $host > /dev/null 2>&1; then
        echo "✓ $host is reachable"
        return 0
    else
        echo "✗ $host is unreachable!"
        return 1
    fi
}

# Function to log with timestamp
log_with_time() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" | tee -a $LOG_FILE
}

# Main monitoring function
run_monitor() {
    log_with_time "=== Network Monitor Check Started ==="

    # Check network interfaces
    log_with_time "Network Interface Status:"
    ip addr | grep -E "^[0-9]+:|inet " | tee -a $LOG_FILE

    # Check connectivity to important hosts
    log_with_time "Connectivity Tests:"

    # Check gateway
    log_with_time "Default Gateway ($GATEWAY):"
    if ! check_host $GATEWAY | tee -a $LOG_FILE; then
        log_with_time "ALERT: Cannot reach default gateway!" | tee -a $LOG_FILE
    fi

    # Check DNS servers
    log_with_time "DNS Servers:"
    if ! check_host $DNS_SERVER1 | tee -a $LOG_FILE; then
        log_with_time "ALERT: Cannot reach primary DNS ($DNS_SERVER1)!" | tee -a $LOG_FILE
    fi

    if ! check_host $DNS_SERVER2 | tee -a $LOG_FILE; then
        log_with_time "ALERT: Cannot reach secondary DNS ($DNS_SERVER2)!" | tee -a $LOG_FILE
    fi

    # Check websites
    log_with_time "Websites:"
    for site in "${WEBSITES[@]}"; do
        if ! check_host $site | tee -a $LOG_FILE; then
            log_with_time "ALERT: Cannot reach $site!" | tee -a $LOG_FILE
        fi
    done

    # List active network connections
    log_with_time "Active Network Connections:"
    netstat -tuln | head -20 | tee -a $LOG_FILE

    # Simple speed test (download a small file)
    log_with_time "Speed Test:"
    start_time=$(date +%s.%N)
    wget -q -O /dev/null https://example.com/
    end_time=$(date +%s.%N)
    download_time=$(echo "$end_time - $start_time" | bc)
    log_with_time "Download time for example.com: $download_time seconds" | tee -a $LOG_FILE

    log_with_time "=== Network Monitor Check Completed ===\n" | tee -a $LOG_FILE
}

# Run continuously with interval
if [ "$1" == "-d" ] || [ "$1" == "--daemon" ]; then
    interval=${2:-300}  # Default to 5 minutes if not specified
    log_with_time "Starting network monitor in daemon mode (interval: ${interval}s)"
    while true; do
        run_monitor
        sleep $interval
    done
else
    # Run once
    run_monitor
fi
```

Make it executable and run:

```bash
chmod +x network_monitor.sh
./network_monitor.sh
# Or run in daemon mode
./network_monitor.sh -d 60  # Check every 60 seconds
```

For macOS, replace `ip addr` with `ifconfig` and adjust other commands as needed.

## Additional Notes

### Network Connectivity Tools

- `ping` checks if a host is reachable and measures round-trip time
- Common ping options: `-c` (count), `-i` (interval), `-W` (timeout)
- Ping uses ICMP protocol, which may be blocked on some networks

### Network Interface Configuration

- `ifconfig` (older) or `ip` (newer) command shows interface details
- Each network interface has an IP address, netmask, and MAC address
- Common interfaces: eth0/en0 (Ethernet), wlan0/en1 (WiFi)

### DNS Tools

- DNS (Domain Name System) translates domain names to IP addresses
- Common record types: A (IPv4), AAAA (IPv6), MX (mail), NS (nameserver), CNAME (alias)
- Popular public DNS servers: 8.8.8.8 (Google), 1.1.1.1 (Cloudflare)

### Web Request Tools

- `curl` is more feature-rich with many options for complex HTTP requests
- `wget` is simpler and better for downloading files and recursively downloading websites
- Common curl options: `-X` (HTTP method), `-H` (headers), `-d` (data)

### Remote Access Security

- Always use key-based authentication instead of passwords when possible
- Generate SSH keys with `ssh-keygen`
- Copy public key to server with `ssh-copy-id username@hostname`
- Configure SSH securely in `/etc/ssh/sshd_config` (server) or `~/.ssh/config` (client)

### Transfer Tool Differences

- `scp` is simple but `rsync` is more powerful for synchronization
- `rsync` only transfers changed parts of files, making it faster for large files
- `rsync` options: `-a` (archive), `-v` (verbose), `-z` (compress), `-n` (dry-run)

```

```
