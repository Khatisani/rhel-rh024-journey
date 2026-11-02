# 14: Using Image Mode for RHEL with bootc

A guide to image mode for Red Hat Enterprise Linux, managing immutable container-based operating system blueprints via `bootc`, deploying transactional image updates, and rolling back changes.

---

## 1. What is Image Mode for RHEL?

Traditional Linux deployments manage the operating system as a collection of individual RPM packages that drift over time. **Image mode for RHEL** fundamentally shifts this approach:

- **Single Blueprint:** The operating system, runtime applications, dependencies, and configuration files are packaged into a single container image (OCI container).
- **Immutability & Predictability:** The base operating system filesystem is read-only and identical across all deployed hosts.
- **Transactional Updates:** Updates are not applied package-by-package. Instead, a new container image is built, pulled, and staged.
- **Safe Rollbacks:** Because updates are staged as distinct image states, the system can instantly roll back to the previously working deployment if an issue arises.

The core utility used to manage these operational states is **`bootc`** (Bootable Containers).

---

## 2. Inspecting the System State

To inspect and manage the image-mode host:

### Connect to the Host
```bash
# Connect to the bootc host via SSH
ssh bootchost
```

### Check Image Deployment Status
The primary diagnostic command to inspect staged and active system images:

```Bash
bootc status
``` 

This shows:

- The currently booted container image reference.
- Any staged updates pending reboot.
- Rollback target availability.

### Verify Application Services & Installed Packages
```Bash
# Check if a workload (such as Apache) is running
systemctl status httpd

# Query installed RPM packages from the container image layer
rpm -q httpd
```

---


## 3. Switching and Updating Images (`bootc switch`)
When updates, new features, or updated configurations are published, the host transitions to a new image tag hosted in a registry (e.g., Quay.io).

- Step 1: Switch to the Target Image
The switch command pulls the target container image from the container registry and stages it for the next deployment:

```Bash
sudo bootc switch quay.io/rdacosta/bootc_httpd:2
``` 

- Step 2: Reboot to Apply the Deployment
Because the running operating system is immutable, changes take effect atomically when rebooting into the newly staged image layer:

```Bash
sudo reboot
```

- Step 3: Verify the New Deployment
After the host finishes rebooting:

```Bash
# Reconnect to the host
ssh bootchost

# Confirm the active boot target is now the ':2' image
bootc status
```

---


## 4. Recovering from Failures: Rollbacks
If an update introduces unexpected errors or service failures, image mode allows an immediate atomic rollback to the previous operational state:

```Bash
# Roll back to the preceding operational deployment
sudo bootc rollback

# Reboot into the restored deployment
sudo reboot
```