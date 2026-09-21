# 06: Basic File Management

A comprehensive operational reference for inspecting, creating, navigating, copying, renaming, and safely removing files and directories on Red Hat Enterprise Linux.

---

## 1. Directory Listing & Inspection (`ls`)

The `ls` utility queries the filesystem to display the contents of a target directory. Without flags, it prints only file and directory names.

### Key Flags & Options
- **`-l` (Long Listing):** Formats output in a multi-column view showing file types, permissions, hard link counts, owner user, owner group, byte size, and last modification timestamp.
- **`-h` (Human-Readable):** Pairs with `-l` to convert raw byte values into scaled units (`K`, `M`, `G`).
- **`-a` (All):** Displays hidden entries—files or directories whose names begin with a leading dot (`.`), such as shell configuration files (`.bashrc`) and SSH keys.

### Practical Bash Examples
```bash
# Basic listing of the current directory
ls

# Detailed listing showing permissions, owner, group, and raw byte sizes
ls -l /var/log

# Detailed human-readable listing, including hidden dotfiles
ls -lah ~

# Sort files by modification time, newest first, with detailed units
ls -lh -t /etc
```

## 2. Directory Creation (`mkdir`)
The `mkdir` (make directory) command creates one or more new directories inside the filesystem hierarchy.

### Key Flags & Options
- **-p (Parents):** Creates parent directories as needed without returning an error if intermediate paths do not exist. If the target directory already exists, it succeeds silently without throwing an error.

- **-v (Verbose):** Prints an explicit diagnostic message for every created directory, useful in setup scripts to verify operations.

### Practical Bash Examples

```bash
# Create a single directory in the current working directory
mkdir projects

# Create a deeply nested directory path in a single command
# Without -p, this fails if 'backups' or 'databases' do not yet exist
mkdir -p /backups/databases/sql_dumps

# Create nested paths verbosely to inspect each step in stdout
mkdir -pv /opt/apps/backend/logs
```

## 3. Directory Navigation (`cd`) & File Creation (`touch`)

#### Directory Navigation (`cd`)
The `cd` (change directory) shell built-in moves the current working context across the filesystem hierarchy.

### Practical Bash Examples

```bash
# Navigate using an absolute path (starting from root '/')
cd /etc/systemd/system

# Navigate using a relative path (relative to your current working directory)
cd ../../var/log

# Return directly to your user's home directory (/home/username or /root)
cd ~
# Or simply:
cd

# Jump back to the immediately preceding directory
cd -
```


#### File Creation (`touch`)
The `touch` command creates an empty regular file if the target does not exist. If the file already exists, it updates its access and modification timestamps without altering content.

### Practical Bash Examples

```bash
# Create a new, empty configuration file
touch app_config.yaml

# Create multiple empty placeholder files simultaneously
touch index.html style.css app.js

# Update the modification timestamp of an existing file to current system time
touch /var/log/custom_audit.log
```

## 4. Copying Files & Directories (`cp`)
The `cp` utility duplicates files or entire directory trees from a source location to a destination.

### Key Flags & Options
- **-r or -R (Recursive):**  Copies directories and all contained subdirectories and files.

- **-p (Preserve):** Retains original file mode permissions, ownership, and timestamps.

- **-i (Interactive):** Prompts for user confirmation before overwriting an existing destination file.

### Practical Bash Examples
```bash
# Duplicate a file within the same directory as a backup
cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bak

# Copy a file to another directory while retaining its original name
cp database.sqlite /var/data/

# Copy an entire directory tree recursively
cp -r /home/developer/code /backup/code_snapshot

# Copy an entire directory while preserving file permissions, timestamps, and ownership
cp -rp /var/www/html /opt/web_backup
```

## 5. Moving & Renaming Items (`mv`)
In Linux, moving a file to a new directory and renaming a file are the exact same operation handled by the `mv` command. The entry is simply updated to point to the new location or name.

### Key Flags & Options
- **-i (Interactive):** Prompts before overwriting an existing destination file.
- **-v (Verbose):** Prints what was moved or renamed.

 ### Practical Bash Examples

```bash 
# Rename a file within the same directory
mv legacy_settings.conf production_settings.conf

# Move a file to a different directory without renaming it
mv report.pdf /home/user/documents/

# Move and rename a file in a single operation
mv error.log /var/log/archived/error_2026_09.log

# Move a directory and its entire contents
mv /tmp/staging_data /opt/production_data/
```

## 6. Removing Files & Directories (`rm`)
The `rm` utility unlinks and removes files or directories from the filesystem. Deletions on Linux are permanent, files are not sent to a desktop trash bin.

### Key Flags & Options
- **-r (Recursive):** Traverses and deletes directories and their full contents.

- **-i (Interactive):** Prompts for confirmation before each file removal—the safest way to delete files manually.

- **-f (Force):** Ignores non-existent files and never prompts, overriding write-protected confirmations if permissions allow.

### Practical Bash Examples

```bash
# Safely delete a single file with an interactive confirmation prompt
rm -i unwanted_output.txt

# Remove multiple specific files
rm server.log debug.log trace.log

# Recursively delete a directory with interactive prompts for every file
rm -ri /tmp/testing_suite/

# Forcefully remove a directory and its contents without any confirmation prompt
# Use with extreme caution, especially when executing as root
rm -rf /tmp/scratch_cache/
```