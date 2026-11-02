# 15: Red Hat Insights & Image Builder

A guide to Red Hat Insights, proactive vulnerability management, and standardized deployment workflows using both hosted and on-premise Image Builder tools.

---

## 1. Overview: What is Red Hat Insights?

Red Hat Insights is a predictive analytics and system management service included with active RHEL subscriptions. Rather than reacting to outages or security breaches after they occur, Insights continuously analyzes system configuration, performance metrics, and security footprints against Red Hat's proprietary knowledge base to identify operational risks, compliance gaps, and active vulnerabilities before they impact production environments.

---

## 2. Red Hat Image Builder: Standardized Deployments

Image Builder creates customized, repeatable RHEL system images tailored for target platforms (AWS, Azure, Google Cloud, VMware, or bare-metal ISOs). It guarantees rapid, predictable deployments where every provisioned machine conforms to organization-wide standards.

Two primary interfaces are available:

### 1. Hosted Service (Insights Image Builder)
A fully managed web interface integrated into the Red Hat Hybrid Cloud Console.
- Access via browser: [console.redhat.com](https://console.redhat.com)
- Navigate to: **Insights for RHEL** > Expand **Inventory** > Select **Images**
- Configure blueprints, choose target cloud/virtualization environments, package bundles, and custom user credentials directly in the UI.

### 2. On-Premise CLI / Web Console (`osbuild-composer`)
A local tool run directly on a RHEL host for air-gapped setups or automated local CI/CD pipelines.
- Configured via command line (`composer-cli`) or local RHEL Web Console (Cockpit).

---

## 3. Vulnerability Management with Insights

Tracking published Common Vulnerabilities and Exposures (CVEs) manually across multiple systems is unsustainable. The Insights Vulnerability service bridges this gap:

- **Proactive Risk Prioritization:** Distinguishes between purely theoretical risks and vulnerabilities that actively expose running packages on your registered hosts.
- **Context-Aware Remediation:** Automatically suggests Ansible playbooks to patch or mitigate confirmed CVEs.

### Registering a Host with Insights
To begin reporting system metrics and auditing security status:

```bash
# Register the local system with Red Hat Insights
sudo insights-client --register
```

Once registered, the client initiates an encrypted upload of system metadata to the Hybrid Cloud Console.

### Auditing Vulnerabilities
1. Navigate to console.redhat.com.

2. Go to Insights for RHEL > Security > Vulnerability (or under Inventory > select your host).

3. Review:

- Specific CVE IDs matching your installed packages.

- CVSS base score versus contextual impact.

- Availability of patches (RHSAs) or remediation workarounds.