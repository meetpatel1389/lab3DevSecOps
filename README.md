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
# Task 3: Connect Grafana to Azure Monitor

## Goal:
Use Managed Identity so you don’t have to store Azure credentials in Grafana.

### Enable System-Assigned Managed Identity on your VM:
- Go to Azure Portal → Your VM → **Identity** (under Settings)
- Toggle **System assigned** to **On**
- Click **Save**

### Assign Roles to the VM’s identity:
- In **Azure Monitor** → **Access control (IAM)**, add the **Monitoring Reader** role for your VM’s identity.
- In your **Azure Subscription** → **Access control (IAM)**, add the **Reader** role for your VM’s identity.

### Edit Grafana config to enable Managed Identity:
```bash
sudo nano /etc/grafana/grafana.ini
```

Under the `[azure]` section, ensure you have:
```ini
[azure]
managed_identity_enabled = true
```

Save and exit nano (press `CTRL + O`, then `Enter`, and `CTRL + X`).

### Restart Grafana:
```bash
sudo systemctl restart grafana-server
```

### Configure Azure Monitor in Grafana:
- In the Grafana UI, go to **Configuration → Data Sources → Add Data Source**
- Select **Azure Monitor** from the list.
- Choose **Authenticate using Managed Identity** (instead of providing Tenant ID, Client ID, and Client Secret).
- Click **Save & Test** to verify the connection.

---

# Task 4: Create a Dashboard in Grafana

## Create a new Dashboard:
- Click the **+** icon (on the left) → **Dashboard** → **New Dashboard** → **Add new panel**

## Select Data Source:
- From the dropdown, select **Azure Monitor**

## Pick a Metric:
- Choose metrics such as **CPU Usage, Memory Usage, Network In/Out**, etc.

## Customize:
- Adjust thresholds, colors, labels, and visualization type (e.g., graph, gauge, etc.)

## Save the panel and dashboard.
Repeat these steps to add more panels as needed to get a complete performance view of your Ubuntu server.

---

# Troubleshooting & Tips

### Grafana prompts for Tenant ID/Client ID/Secret:
- Verify that `managed_identity_enabled = true` is **not commented out** in `/etc/grafana/grafana.ini`.
- Ensure your VM has a **System Assigned identity** and that roles are assigned correctly.
- Restart Grafana after any configuration changes.

### Permissions Issues:
- Double-check that your VM’s identity has the **Reader** role on the **Subscription** and **Monitoring Reader** on **Azure Monitor**.

### Port 3000 Blocked:
- Update your **NSG (Network Security Group)** or firewall rules to allow inbound traffic on **port 3000**.

