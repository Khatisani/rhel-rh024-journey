# 08: Organizing Local Users and Groups

An operational guide to local account administration, file-based identity records, group membership, security practices, and UID management in Red Hat Enterprise Linux.

---

## 1. Where Identity Information Lives

Local user and group management on Linux relies on plain-text files stored in the `/etc` configuration directory.

### `/etc/passwd`
Stores public account metadata for all local users on the system. It is world-readable so applications can map numerical IDs to human-readable names.

- Inspect all local user accounts:
  ```bash
  cat /etc/passwd
  ```

Each line records colon-separated attributes: username, password placeholder (`x`), numerical User ID (UID), primary Group ID (GID), user comment/GECOS, home directory path, and default login shell.

### `/etc/shadow`
Stores encrypted account credentials and password aging policies.

    - Highly restricted: readable only by `root` (permissions 000 or 0400).
    - Keeps password hashes protected from standard users to prevent offline brute-force attacks.

### `/etc/group`
Defines local groups, their Group IDs (GID), and secondary group member lists.

## 2. Elevating Privileges: sudo vs. su -

### The Difference
    - `su -`: Switches directly to the root user account and requires the root user's password. It lacks accountability because multiple people share the same credential, obscuring who ran which command.

    - `sudo` ("superuser do"): Runs individual commands with elevated administrative privileges using the caller's own password.

### Why sudo is the Enterprise Standard
Logs every execution to the system journal (`/var/log/secure`), providing an exact audit trail of who executed what command and when.

In RHEL, users belonging to the `wheel` group are granted full `sudo` rights by default in /`etc/sudoers`.

To open an interactive root shell cleanly through sudo:

```Bash
sudo -i
```

## 3. Managing Local Users

### Creating and Securing Accounts
``` Bash
# Create a new local user account
sudo useradd alex

# Set or update the login password
sudo passwd alex

# Lock an account to block login access immediately
sudo passwd -l alex

# Unlock a locked account
sudo passwd -u alex
```

### Modifying User Attributes
Use `usermod` to adjust properties without recreating accounts:

``` Bash
# Change a user's default login shell
sudo usermod -s /bin/zsh alex

# Append a user to a supplementary group (always pair -a with -G to avoid overwriting existing groups)
sudo usermod -aG wheel alex
```

### Inspecting Identity & Group Associations

```
Bash
# Display UID, GID, and all associated group memberships
id alex

# List only the group names a user belongs to
groups alex
```

## 4. Managing Local Groups
Groups bundle permissions together so access rights can be granted to multiple users simultaneously.

``` Bash
# Create a new group
sudo groupadd developers

# Rename an existing group
sudo groupmod -n engineering developers

# Remove a group definition
sudo groupdel engineering
```

## 5. Deleting Users, Data Retention, and the Orphan UID Risk
### Account Deletion Options
    - `userdel alex`: Deletes the user account entry from /etc/passwd and /etc/shadow, but leaves /home/alex and mail spools untouched.

    - `userdel -r alex`: Recursively deletes the user along with their home directory and mailbox.

### The Security Risk of Deleting Accounts
Every file on a Linux filesystem tracks ownership by numerical UID, not by the textual username.

When you delete a user:
1. Files previously owned by that account still hold the original numerical UID (e.g., UID `1002`).

2. These become orphaned files.

3. If a new user is created later and assigned that recycled UID `1002`, they automatically inherit full read, write, and execute permissions over those lingering files.

4. On shared or networked storage, this creates severe privilege escalation and data leak hazards.

### Safe Administration Practice
Instead of deleting user accounts:

Lock the account using `passwd -l <username>` to disable interactive logins while preserving audit histories and file ownership boundaries.

If an account must be purged, sweep the filesystem to audit or reassign all files tagged with that UID before deletion:

```Bash
# Locate all files owned by a specific numerical UID
sudo find / -uid 1002
```

## 6. Standalone vs. Centralized Identity Management (IdM)
    - Local Accounts: Configured directly on a single host inside `/etc/passwd`. Viable for standalone virtual machines, appliances, or emergency maintenance, but impossible to maintain across dozens or hundreds of servers.

    - Red Hat Identity Management (IdM): Included with RHEL subscriptions. Centralizes authentication, authorization, Kerberos single sign-on, host access rules (HBAC), and sudo policies. Day-to-day enterprise user accounts exist in IdM, meaning administrators manage identity in one central system rather than updating local files host by host.