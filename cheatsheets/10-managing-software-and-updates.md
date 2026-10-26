# 10: Managing Software and Updates

A guide to system registration, software lifecycle management using DNF, security advisory classifications (RHSA, RHBA, RHEA), CVE severity tracking, and reboot diagnostics on Red Hat Enterprise Linux.

---

## 1. System Registration & Content Delivery

To receive package updates, patches, and security errata, a RHEL system must be registered and authenticated.

### Subscription Manager
- **Check identity/registration status:**
  ```bash
  sudo subscription-manager identity
  ```

### Register the host:
```Bash
sudo subscription-manager register
```

### Force re-registration (override existing identity):
```Bash
sudo subscription-manager register --force
```

### Red Hat CDN vs. Red Hat Satellite
Red Hat Content Delivery Network (CDN): Systems pull updates directly from Red Hat servers across the public internet.

Red Hat Satellite: Used when internal servers lack direct internet access. Acts as a local content proxy inside the organization's private network.Satellite downloads packages and updates once from the CDN, then distributes them locally across internal hosts.   

## 2. Package Management with DNF

DNF (Dandified YUM) is the default package management utility on RHEL, handling dependency resolution and installation automatically. 

### Searching and Inspecting Packages
```Bash
# Search repository packages matching a keyword
dnf search "package_name"
```

### Installing Software & Automatic Dependencies
When installing software, DNF calculates and installs required runtime dependencies automatically.

```Bash
# Standard installation (prompts before applying changes)
sudo dnf install "package_name"

# Automated non interactive installation (assumes 'yes' to all prompts)
sudo dnf install -y "package_name"
```

Using the -y flag skips manual package review. Always verify the package dependency tree before running non-interactive installations on production systems.

## 3. Red Hat Errata Advisory Types

Software updates in the Red Hat ecosystem are packaged and delivered as Errata Advisories categorized by purpose:  

RHBA (Red Hat Bug Advisory): Focuses strictly on bug fixes and system stability corrections.

RHEA (Red Hat Enhancement Advisory): Delivers feature additions, minor improvements, and updated hardware or application support.

RHSA (Red Hat Security Advisory): Contains security patches addressing specific Common Vulnerabilities and Exposures (CVEs).

## 4. Understanding Vulnerabilities: CVEs & CVSS

CVE (Common Vulnerabilities and Exposures): A standardized, vendor-neutral system used to uniquely identify public cybersecurity vulnerabilities.

CVSS (Common Vulnerability Scoring System): A numerical framework (0.0 to 10.0) quantifying the relative severity and impact of a vulnerability:  
-  Medium / Moderate (e.g., 5.5): Requires specific conditions or limited privileges to exploit.  
-  Critical (e.g., 9.8): Severe risk; typically enables unauthenticated remote code execution or complete system takeover. 

## 5. Security-Focused Patching and Updates

DNF allows targeted patching so administrators can apply urgent security remediations without updating the entire operating system stack.

### Auditing Errata

```Bash
# List all available errata advisories for installed packages
dnf updateinfo list

# Display detailed metadata for a specific security advisory (e.g., RHSA-2025:10854)
dnf info --advisory=RHSA-2025:10854
# Or query the advisory directly:
dnf info RHSA-2025:10854
```

### Applying Targeted Security Updates

```Bash
# Standard full-system update across all packages
sudo dnf update

# Apply only security-related updates (RHSA), skipping standard feature upgrades
sudo dnf update --security

# Limit updates strictly to High and Critical priority security fixes
sudo dnf update --sec-severity=Important
sudo dnf update --sec-severity=Critical
```

## 6. Post-Update Diagnostics & Reboot Management

Certain updates (such as glibc, systemd, or Linux kernel patches) require service restarts or a full machine reboot to take effect in active memory.

### Checking Reboot Requirements
Use the `needs-restarting` plugin (from `dnf-plugins-core`) to check if processes are holding deleted libraries in memory:

``` Bash
# Check if a full system reboot is required
dnf needs-restarting -r
```

- If `dnf needs-restarting -r` exits with `0`, no reboot is required.

- If it flags that the system must restart (e.g., a newer kernel was installed), reboot the host:
```Bash
sudo reboot
```

