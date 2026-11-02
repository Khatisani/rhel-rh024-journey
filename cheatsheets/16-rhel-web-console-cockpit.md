# 16: Managing RHEL via the Web Console (Cockpit)

A guide to the RHEL Web Console (powered by the upstream project **Cockpit**), covering socket activation, network port requirements, interface capabilities, and extending dashboard functionality via modular add-ons.

---

## 1. Overview: What is the RHEL Web Console?

The **RHEL Web Console** (Cockpit) provides a browser-based management graphical interface for Red Hat Enterprise Linux systems:

- **Simplified Administration:** Perform complex system administration tasks (storage management, networking, system accounts, updates) visually without memorizing syntax or configuration file locations.
- **Real-Time Telemetry:** Live graphs and meters for CPU, memory, storage I/O, and network throughput.
- **Embedded Terminal:** Offers direct, fully functional terminal access inside the browser session.
- **Resource Efficient:** Does not run a heavy web server stack. Instead, it relies on systemd socket activation and consumes zero memory when not actively in use.

---

## 2. Enabling and Starting Cockpit

Cockpit utilizes systemd **socket activation**. The background daemon (`cockpit-ws`) only spawns when an incoming connection hits the listening port, automatically terminating when idle to conserve system resources.

### Enable the Socket
```bash
# Enable on boot and immediately start the listening socket
sudo systemctl enable --now cockpit.socket

# Verify the socket is listening
systemctl status cockpit.socket
```

## 3. Firewall Access & Connecting via Browser
By default, the Web Console listens on network port 9090.

### Open Port 9090 in firewalld
```Bash
# Allow the cockpit service through firewalld
sudo firewall-cmd --add-service=cockpit --permanent

# Reload the active firewall configuration
sudo firewall-cmd --reload
``` 

### Accessing the Web Dashboard
Open any modern web browser on your network and navigate to:

```Bash
https://<server-ip-or-hostname>:9090
```

(Accept the default self-signed SSL certificate warning, then log in using your standard RHEL system credentials or administrative `root` user).

## 4. Key Capabilities in the Web Console
Once authenticated, the console organizes operational data into specialized views:

- System Health & Specs: Overview of machine specifications, BIOS/firmware details, hardware health, and uptime.

- Resource Usage: Real-time and historical graphs of CPU, RAM, disk, and network consumption.

- Logs & Diagnostics: Direct integration with systemd-journald to search, filter by severity, and inspect system log entries.

- Storage Management: Create partition schemes, manage LVM volume groups, configure RAID, and mount NFS shares visually.

- Network Management: Toggle interfaces, assign static IP addresses, inspect routes, and configure bonding or VLANs.

- Accounts: Create and manage local users, set passwords, lock accounts, and manage SSH public keys.

- Software Updates: Audit pending security errata, install package updates, and configure automatic patching.

- Terminal: Built-in web terminal to run standard CLI commands directly from the dashboard.

## 5. Extending Functionality with Cockpit Modules
Cockpit is modular. Extra management screens (such as Podman containers, virtual machines, and file sharing) can be installed from DNF repositories:

```Bash
# Search for available Cockpit extensions and plugins
dnf search cockpit-*

# Example: Install support for managing Podman containers and KVM virtual machines
sudo dnf install -y cockpit-podman cockpit-machines

# Install all available Cockpit enhancements
sudo dnf install -y cockpit-*
``` 

After installing plugins, refresh the browser page to display the newly integrated menus in the console navigation sidebar.