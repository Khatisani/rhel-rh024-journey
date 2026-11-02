# 13: Deploying an Application Runtime & Hosting Services

A guide to provisioning a runtime environment, installing project dependencies, configuring a custom systemd unit file for background execution, and opening application ports through `firewalld` on Red Hat Enterprise Linux.

---

## 1. Provisioning the Application Runtime

To host an application, the host must first install the required programming runtime and build tooling from the default repositories.

```bash
# Install the Node.js runtime and npm package manager
sudo dnf install -y nodejs git
```

---


## 2. Deploying Application Source Code
Clone the remote application repository into your working directory and install its required library dependencies.

```Bash
# Clone the application repository
git clone [https://gitlab.com/rgdacosta/nodejs_rhel10.git](https://gitlab.com/rgdacosta/nodejs_rhel10.git)

# Move into the project directory
cd nodejs_rhel10

# Install declared application dependencies via npm
npm install
```

---


## 3. Configuring a Custom systemd Service Unit
To run the application continuously as a persistent background daemon managed by Linux (rather than manually inside an interactive shell session), create or inspect a systemd service unit file.

- Inspect the Service Unit Definition
```Bash
cat myapp.service
``
A typical application service definition contains:

```Ini, TOML
[Unit]
Description=Node.js Sample Application
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/path/to/nodejs_rhel10
ExecStart=/usr/bin/node index.js
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

- Install and Register the Unit
Copy the custom unit file to the systemd administrative directory where custom system-wide unit files reside:

```Bash
# Copy unit definition to the primary systemd configuration path
sudo cp myapp.service /etc/systemd/system/

# Notify systemd to scan the directory and reload its unit configurations
sudo systemctl daemon-reload

# Enable the service to launch on boot and start it immediately
sudo systemctl enable --now myapp

# Verify that the process is running and active
systemctl status myapp
```

---


## 4. Configuring Network Ingress for the Application
If the Node.js application listens on a non-standard port (such as 8080), firewalld will block inbound external requests until explicitly opened.

```Bash
# Open port 8080 in runtime memory
sudo firewall-cmd --add-port=8080/tcp

# Persist the rule across firewall reloads and system reboots
sudo !! --permanent

# Reload the firewall configuration to synchronize active runtime state
sudo firewall-cmd --reload
```

---

## 5. Verification
Verify the application response either locally or through a browser interface:

```Bash
# Query the locally running service endpoint
curl http://localhost:8080
```

Open a web browser on your network or host machine and navigate to:`http://localhost:8080`