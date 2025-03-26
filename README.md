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

### 3.1 Task 1: Prepare Your Ubuntu Server

1. **Update** and **upgrade** packages:
   ```bash
   sudo apt-get update && sudo apt-get upgrade
