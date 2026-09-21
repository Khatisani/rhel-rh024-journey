# 05: The Linux Directories

## 1. The Root Directory (`/`)
In Linux, all files, directories, devices, and mounted drives branch from a single starting point called the **root directory**, represented by a single forward slash (`/`). Unlike Windows, there are no drive letters (like `C:` or `D:`); every storage volume is mounted as a path somewhere under `/`.

### Navigation Fundamentals
- **`pwd`** (Print Working Directory): Displays your current absolute location in the directory tree.
- **`cd`** (Change Directory): Moves between directories (e.g., `cd /var/log`, `cd ..`, `cd ~`).
- **`ls /`**: Lists the top-level directories residing directly under root.

---

## 2. Directory Breakdown
 **`/home`**
Regular User Home Directories
Stores personal files, custom configurations, and user data (e.g., `/home/username`).

**`/root`** 
Superuser Home Directory
The home directory strictly reserved for the `root` administrative user. Standard users cannot access it. 

**`/etc`** 
System Configurations 
Host-specific system configuration files and service settings. Editing files here modifies how the system and services operate.

**`/var`** Variable Data 
Stores dynamic, continuously changing files such as system logs (`/var/log`), spool queues, databases, and mail.

**`/usr`** 
User System Resources 
Contains installed software, utilities, libraries, and read-only data shared across users. 

**`/tmp`** 
Temporary Files 
Scratchpad storage accessible by any user. Files are typically cleared automatically or upon reboot. 

**`/boot`** 
Bootloader & Kernel 
Files necessary to boot the system, including the Linux kernel (`vmlinuz`), initial RAM disk (`initramfs`), and GRUB configs. 

**`/run`** 
Runtime Process State 
Volatile runtime data describing the current active system state (e.g., daemon PIDs, sockets). Mounted as `tmpfs` and recreated fresh every boot. 

**`/mnt`** 
Temporary Mount Point 
Standard location for administrators to temporarily mount external filesystems, USB drives, or network shares. 

---

## 3. Deep Dive: Key Subdirectories

### Inside `/usr` (Installed Software & Shared Assets)
Modern Linux distributions (including RHEL) consolidate core binaries and shared libraries into `/usr`:
- **`/usr/bin`**: Standard executable binary programs available to all users (often symlinked from `/bin`).
- **`/usr/lib64`**: 64-bit shared runtime libraries required by system executables and applications.
- **`/usr/share`**: Architecture-independent shared assets, such as man pages, manuals, documentation, and application data.

### Persistent vs. Volatile Directories
- **Volatile (cleared upon reboot):**
  - **`/run`**: Active runtime state (PIDs, sockets).
  - **`/tmp`**: Short-lived scratch files.
- **Persistent (survives reboot):**
  - **`/etc`**, **`/var`**, **`/usr`**, **`/home`**, **`/boot`**.