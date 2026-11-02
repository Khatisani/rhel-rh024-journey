# 12: Managing System Startup Services with systemd

A guide to systemd unit types, controlling background daemons via `systemctl`, service enablement lifecycles, and managing firewall ingress rules with `firewalld`.

---

## 1. What is systemd?

`systemd` is the central system and service manager for Red Hat Enterprise Linux. It coordinates boot sequencing, monitors background daemons, tracks processes, and manages system state transitions.

### Everything is a Unit
Systemd abstracts system resources and services as **units**. The file extension denotes the unit type:

- **`.service`**: Manages background applications and daemons (e.g., `httpd.service`, `sshd.service`).
- **`.socket`**: Manages network sockets or IPC FIFO pipes, enabling on-demand socket activation.
- **`.timer`**: Schedules tasks and delayed executions (the native systemd alternative to cron jobs).
- **`.path`**: Triggers actions or activates services when designated files or directories change.
- **`.target`**: Logical groups of units used to create synchronization states (e.g., `multi-user.target`, `graphical.target`).

---

## 2. Walkthrough: Deploying a Persistent Apache Web Server

### Step 1: Install the Web Server Package
```bash
sudo dnf install -y httpd
```

### Step 2: Create a Sample Web Document
Use `tee` with pipes to write the file while handling privileged file creation cleanly:

```Bash
echo "hello world" | sudo tee /var/www/html/index.html
```

---

## 3. Controlling Services with `systemctl`

The `systemctl` command controls the state and lifecycle of systemd service units.

### Starting and Stopping Services

- Start immediately:

```Bash
sudo systemctl start httpd
``` 

- Stop immediately:

```Bash
sudo systemctl stop httpd
```

- Restart (stop and start freshly):

```Bash
sudo systemctl restart httpd
``` 

- Startup on Boot: `enable` vs. `start`

`start` affects only the active in-memory state right now; it will not survive a reboot.

`enable` creates symlinks in /etc/systemd/system/ so the service launches automatically on boot, but does not start it immediately.

Enable and start in one single step:

``` Bash
sudo systemctl enable --now httpd
```

### Checking Operational Status
Inspect the active execution state, exit codes, PID, and most recent journal logs:

```Bash
systemctl status httpd
```

---


## 4. Opening Ports with `firewalld`

Even if a service is running and bound to port 80, the Linux kernel firewall blocks external incoming traffic by default.

- Adding Ingress Service Rules

```Bash
# Permit HTTP traffic in the running in-memory firewall state (volatile)
sudo firewall-cmd --add-service=http

# Make the rule persistent across firewall reloads and reboots
sudo firewall-cmd --add-service=http --permanent

# Reload firewalld to apply persistent disk rules to the runtime
sudo firewall-cmd --reload
```

- Verifying Firewall Configuration

Inspect all active rules, allowed ports, and permitted services:

```Bash
sudo firewall-cmd --list-all
```

---


## 5. Terminal Productivity: History Expansion Shortcuts

Bash provides fast history expansion operators to modify and rerun recent instructions without retyping:

- 1. Repeat Previous Command with Appended Flags (`!!`)
`!!` expands to the complete text of the last command executed:

```Bash
sudo firewall-cmd --add-service=http
sudo !! --permanent
# Expands to: sudo firewall-cmd --add-service=http --permanent
```

- 2. Search and Replace (`^old^new`)
Quickly substitute a substring in your previous command and execute it immediately:

Switch restart to stop:

``` Bash
sudo systemctl restart httpd
^restart^stop
# Expands to: sudo systemctl stop httpd
```

Switch stop to disable:

``` Bash
^stop^disable
# Expands to: sudo systemctl disable httpd
```