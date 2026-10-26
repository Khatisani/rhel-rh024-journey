# 11: Managing Networking

A guide to network configuration in Red Hat Enterprise Linux using NetworkManager, connection profiles, `nmcli`, routing tables, and DNS settings.

---

## 1. NetworkManager Architecture: Devices vs. Connection Profiles

NetworkManager is the default network configuration daemon in RHEL. It separates network hardware from configuration settings:

- **Devices:** Physical or virtual network interfaces recognized by the Linux kernel (e.g., `enp7s0`, `lo`).
- **Connection Profiles:** Saved software configurations containing IP addresses, gateways, subnets, and DNS settings.
- **Rule:** Multiple profiles can be defined for a single interface, but only **one active profile** can be bound to a device at any given time.

### Interface Naming Conventions (Predictable Network Interface Names)
RHEL uses hardware-location-based device names rather than legacy `eth0` naming:
- Example: **`enp7s0`**
  - `en`: Ethernet
  - `p7`: Bus / Port 7
  - `s0`: Slot 0

---

## 2. Inspecting Network Configuration

### Inspect Profiles with `nmcli`
The `nmcli` (NetworkManager Command-Line Interface) utility controls NetworkManager settings.

```bash
# Display all configured connection profiles
nmcli connection show
# (or the common short alias)
nmcli con show

# Inspect details of a specific prof
```

### Inspect Active Interfaces & Addresses with `ip`
The modern `ip` utility (from the `iproute2` package) inspects operational kernel network parameters:

```Bash
# Display IP addresses assigned to all interfaces
ip address show

# Shorthand alias
ip a s

# Display address details for a single interface
ip a s enp7s0
```

## 3. Creating & Managing Connection Profiles

### Creating a Connection Profile

```Bash
# Basic profile addition syntax
nmcli connection add con-name "static-profile" ifname enp7s0 type ethernet ipv4.method manual ipv4.addresses 192.168.1.50/24 ipv4.gateway 192.168.1.1 ipv4.dns 8.8.8.8
```

### Deleting a Connection Profile

``` Bash
# Remove an existing connection profile
nmcli con del "profile_name"
```

## 4. Verifying Network Routes & DNS Services

### Verifying Routes & Gateway
Use `ip` route show (or short `ip r s`) to verify that the default gateway is properly populated:

``` Bash
# Display kernel routing table and default gateway
ip route show

# Shorthand alias
ip r s
```

### Inspecting DNS Configuration
DNS resolvers assigned to the host are displayed in `/etc/resolv.conf`:

```Bash
# View active nameservers configured for the host
cat /etc/resolv.conf
```

## 5. Text User Interface Alternative: `nmtui`
or interactive configuration without memorizing nmcli arguments, RHEL provides nmtui (NetworkManager Text User Interface):

```Bash
# Launch the interactive curses-based network manager
nmtui
```

- User-friendly: Visual, menu-driven navigation directly inside the terminal.

- Safe & Quick: Avoids syntax errors when setting static IPs, adjusting DNS entries, or toggling connections up and down.