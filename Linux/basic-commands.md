# Linux Basic Commands

Quick reference for the most commonly used Linux commands.

---

## Navigation

```bash
pwd                  # Print current working directory
ls -la               # List all files with details
cd /path/to/dir      # Change directory
cd ..                # Go up one level
cd ~                 # Go to home directory
```

## File & Directory Operations

```bash
touch file.txt                  # Create an empty file
mkdir my-folder                 # Create a directory
mkdir -p a/b/c                  # Create nested directories
cp source.txt dest.txt          # Copy a file
cp -r source/ dest/             # Copy a directory recursively
mv old.txt new.txt              # Move or rename a file
rm file.txt                     # Delete a file
rm -rf my-folder/               # Delete a directory and its contents (use with caution)
```

## Viewing Files

```bash
cat file.txt         # Print file contents
less file.txt        # View file with pagination (q to quit)
head -n 20 file.txt  # Show first 20 lines
tail -n 20 file.txt  # Show last 20 lines
tail -f app.log      # Follow a log file in real time
```

## Search

```bash
grep "pattern" file.txt          # Search for a pattern in a file
grep -r "pattern" ./dir/         # Search recursively in a directory
grep -i "pattern" file.txt       # Case-insensitive search
find /path -name "*.log"         # Find files by name
find /path -type f -mtime -1     # Files modified in the last day
```

## Permissions

```bash
chmod 755 script.sh   # rwxr-xr-x
chmod +x script.sh    # Add execute permission
chown user:group file # Change file owner and group
ls -l file.txt        # View file permissions
```

## Process Management

```bash
ps aux               # List all running processes
top                  # Interactive process viewer
htop                 # Improved interactive process viewer
kill <PID>           # Terminate a process by PID
kill -9 <PID>        # Force-kill a process
```

## Disk & Memory

```bash
df -h                # Disk usage (human-readable)
du -sh /path/        # Directory size
free -h              # Memory usage
```

## Networking (Quick)

```bash
ip a                 # Show network interfaces and IP addresses
ping google.com      # Test connectivity
curl -I https://example.com   # Send HTTP HEAD request
wget https://example.com/file # Download a file
```

## Archives & Compression

```bash
tar -czf archive.tar.gz /path/to/dir   # Create a gzip-compressed archive
tar -xzf archive.tar.gz                # Extract a gzip archive
zip -r archive.zip /path/to/dir        # Create a zip archive
unzip archive.zip                      # Extract a zip archive
```

## Useful Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl + C` | Kill current process |
| `Ctrl + Z` | Suspend current process |
| `Ctrl + L` | Clear the terminal |
| `Ctrl + R` | Reverse-search command history |
| `!!` | Repeat last command |
| `!$` | Last argument of previous command |
