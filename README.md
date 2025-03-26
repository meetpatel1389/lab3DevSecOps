# Lab 3: Grafana Installation and Dashboard Creation for Ubuntu Server Performance

**Course**: CST8919 - DevOps: Security and Compliance  
**Lab Title**: Grafana Installation and Dashboard Creation for Ubuntu Server Performance  

## 1. Objective

This lab will guide you through:
- Installing **Grafana** on an Ubuntu server
- Setting up **Managed Identity** for Azure Monitor
- Connecting Grafana to **Azure Monitor** to collect performance metrics
- Creating a **Grafana dashboard** to visualize CPU, memory, and network usage

---

## 2. Prerequisites

1. **Ubuntu Server 18.04+** (running on Azure)
2. **Azure account** with permission to:
   - Enable managed identities on VMs
   - Assign roles (Reader, Monitoring Reader)
3. Basic knowledge of **Linux command line** and the **Azure Portal**

---

## 3. Lab Tasks

### Task 1: Prepare Your Ubuntu Server

1. **Update** and **upgrade** packages:
   ```bash
   sudo apt-get update && sudo apt-get upgrade

   # Task 2: Install Grafana

## Install required dependencies:
```bash
sudo apt-get install -y apt-transport-https software-properties-common wget
```

## Add Grafana’s GPG key and repository:
```bash
sudo mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null

echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
```

## Install Grafana:
```bash
sudo apt-get update
sudo apt-get install grafana
```

## Start and enable Grafana service:
```bash
sudo systemctl daemon-reload
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

## Check status:
```bash
sudo systemctl status grafana-server
```

## Access Grafana:
Open a browser and go to: `http://<your-server-ip>:3000`

Default login is: **admin / admin**

You will be prompted to set a new password.
