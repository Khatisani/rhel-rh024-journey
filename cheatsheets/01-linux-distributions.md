# 01: Linux Distributions

## 1. What is a Linux Distribution?
A Linux distribution (distro) is a complete, deployable operating system constructed around the Linux Kernel. 

While the kernel manages hardware and system resources, each distribution differentiates itself through:
- **Tools:** Default shells, init systems (`systemd`), and core utilities.
- **Applications:** Specific admin tools and runtime environments.
- **User Experience:** CLI defaults, desktop environments (like GNOME), and operational workflows.
- **Analogy:** "Two cars built from the same parts, but tuned differently."


## 2. The Red Hat Pipeline
Red Hat uses an open source development pipeline that transitions software from community innovation to enterprise grade stability. Code is built openly, consumed, and contributed back to strengthen the wider Linux ecosystem.

```text
[Fedora Linux]      --> Upstream Innovation
      │                 Community innovation & cutting-edge features first
      │                 Rapid release cycle, community drivem. 
      │
      ▼
[CentOS Stream]     --> Midstream Integration
      │                 Publicly visible development branch for the next minor release of RHEL.
      │
      ▼
   [RHEL]           --> Downstream Production 
                        Hardened, certified, and fully supported production release
                        Polished, strictly tested, highly reliable, and enterprise certified.
```

## 3. Package Manager: rpm 

All distributions in the Red Hat ecosystem use the RPM (Red Hat Package Manager) format.

- Manages installation, updates, queries, and removal of software packages.

- Ensures binary compatibility and standard dependency management across Fedora, CentOS Stream, and RHEL.

## 4. Why RHEL Stands Out: Predictability and Standards 
### File Hierarchy Standard (FHS)

RHEL strictly adheres to the Filesystem Hierarchy Standard (FHS). This standardizes where files, binaries, and configurations live (e.g., /etc for system configurations, /var for variable data), making troubleshooting and automation predictable across systems.

### Predictable Release 
- Major Releases: Released every 3 years.

- In place Upgrades: Uses Leapp (the Red Hat utility for automated in-place upgrades) to move between major versions (e.g., RHEL 8 to RHEL 9) safely and smoothly.

### Lifestyle Support 

RHEL guarantees long-term stability across structured support phases:

```text 
Years 0 ────── 5 ─────────────── 10 ────────────── 12+
┌───────────────┬───────────────┬────────────────────┐
│ Full Support  │  Maintenance  │ Extended Lifecycle │
│ (Features +   │   (Critical   │   Support (ELS)    │
│  Fixes + Sec) │   Sec Only)   │     (Add-on)       │
└───────────────┴───────────────┴────────────────────┘
```

- Full Support (Years 1–5): Full functional updates, software enhancements and critical bug fixes

- Maintenance Support (Years 6–10): Focuses exclusively on critical/important security fixes and urgent bug patches. No new features are introduced to preserve API/ABI stability.

- Extended Update Support: Optional commercial add on providing selected critical patches beyond the initial 10-year period.

