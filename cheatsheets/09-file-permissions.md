# 09: Linux File System Permissions

A detailed operational reference for Linux permission architecture, numeric (octal) calculations, ownership management (`chown`), and permission configuration (`chmod`) on Red Hat Enterprise Linux.

---

## 1. Permission Types: Files vs. Directories

The three standard permission bits behave differently depending on whether they are applied to a regular file or a directory:

```bash
r (Read)    = 4
w (Write)   = 2
x (Execute) = 1
```

### Regular Files
- Read (`r`): View the text or binary contents of a file (e.g., using `cat`, `less`, `head`).

- Write (`w`): Modify, edit, append to, or overwrite the contents of a file (e.g., using `vim`, `nano`, redirect operators `>`).

- Execute (`x`): Run a binary file or script as a process.

To run a script (like a Bash or Python script), you typically need both read and execute (`rx`) so the shell/interpreter can read the file lines and execute them.

### Directories
- `Execute (x): Change into the directory (`cd /path/to/dir`) or access files inside it if you already know their names.

- Read + Execute (`rx`): List the contents of a directory (`ls`) and view file names.

- Write + Execute (`wx`): Create new files/subdirectories inside the directory, or delete/rename existing files within it.

## 2. Who Gets Permissions? (User, Group, Other)
Permissions are defined across three distinct scopes:

```bash
- rwx r-x r--
  ─── ─── ───
   │   │   └─ Other (World)
   │   └───── Owning Group
   └───────── Owning User
```

- Owning User (`u`): The individual user account that owns the file or directory.

- Owning Group (`g`): The group that owns the file or directory. Any user who is a member of this group inherits these permissions.

- Other / World (`o`): Every account on the system that is neither the owning user nor a member of the owning group. ("Other" is not an actual group!)

## 3. Managing Ownership (`chown`)
The `chown` (change owner) command modifies the owning user and/or owning group. Modifying ownership typically requires administrative privileges (`sudo`).

### Change Owning User
```bash
# Change the owning user of 'foo' to 'ricardo'
sudo chown ricardo foo
```

### Change Owning Group
```bash
# Change the owning group of 'foo' to 'developers'
sudo chown :developers foo

# Alternative command specifically for changing group
sudo chgrp developers foo
```

### Change Both User and Group Simultaneously
```bash
# Set 'ricardo' as user and 'developers' as group
sudo chown ricardo:developers foo

# Recursively change ownership for an entire directory tree
sudo chown -R ricardo:developers /var/www/html
```

## 4. Calculating Numeric (Octal) Permissions
Permissions are mathematically combined by adding their octal values:

```plaintext
r = 4
w = 2
x = 1
- = 0
```

- 7 = 4 + 2 + 1 = Read, Write, and Execute (`rwx`)

- 6 = 4 + 2 + 0 = Read and Write (`rw-`)

- 5 = 4 + 0 + 1 = Read and Execute (`r-x`)

- 4 = 4 + 0 + 0 = Read Only (`r--`)

- 0 = 0 + 0 + 0 = No Permissions (`---`)

## 5. Setting Permissions with chmod
The `chmod` (change mode) command modifies permission bits using either numeric (octal) or symbolic notation.

### Numeric Mode Examples
```bash
# Grants 6 (rw-) to owner, 4 (r--) to group, 0 (---) to other
chmod 640 foo

# Recursively grant 7 (rwx) to owner, 5 (r-x) to group, and 5 (r-x) to other
chmod -R 755 /bar

# Standard secure permissions for private SSH keys
chmod 600 ~/.ssh/id_rsa

# Standard secure permissions for web root directories
chmod 755 /var/www/html
```

### Symbolic Mode Examples
Symbolic notation uses references (`u` for user, `g` for group, `o` for other, `a` for all) with operators (+ add, - remove, = set explicitly):

```bash
# Make a script executable for everyone
chmod +x deploy.sh

# Remove write access from group and others
chmod go-w sensitive_data.txt

# Add read and write access for group only
chmod g+rw project_notes.md

# Set exact permissions: user gets rw, group gets r, others get nothing
chmod u=rw,g=r,o= config.json
```

## 6. Real-World Permission Patterns
- `chmod 700 /home/user/.ssh`: Only the owner can enter, read, and write to the .ssh directory; completely locked down from everyone else.

- `chmod 644 /etc/resolv.conf`: Owner (root) can read and modify; everyone else can only view system DNS configurations.

- `chmod 775 /shared/project`: Owner and team group members can create and delete files inside the shared directory; others can view and navigate.