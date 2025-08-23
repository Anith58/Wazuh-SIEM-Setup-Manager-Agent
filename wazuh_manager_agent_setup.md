# Wazuh SIEM Setup - Manager & Agent

## ✨ Overview

This project demonstrates how to set up a Wazuh SIEM environment with both the manager and agents running on virtual machines. It includes detailed steps for installing and configuring the manager and agents.

---

## 📁 Project Structure

- Wazuh Manager: Ubuntu VM
- Agents: Ubuntu + Windows VMs
- Dashboard: Access via web browser (Kibana/Wazuh dashboard)

---

## 🔧 Step 1: Install Wazuh Manager (Ubuntu)

### Update the system

```bash
sudo apt update && sudo apt upgrade -y
```

### Download and install Wazuh Manager

```bash
curl -sO https://packages.wazuh.com/4.12/wazuh-install.sh && sudo bash ./wazuh-install.sh -a
```

### Allow firewall ports...

```bash
sudo ufw allow 1514/tcp
sudo ufw allow 1515/tcp
sudo ufw allow 55000/tcp
sudo ufw allow 5601/tcp
```

### Access Dashboard

```
https://<your-ubuntu-ip>:5601
```

---

## 🧰 Step 2: Install Wazuh Agent on Ubuntu

### Add repo and install agent

```bash
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo apt-key add -
echo "deb https://packages.wazuh.com/4.x/apt/ stable main" | sudo tee /etc/apt/sources.list.d/wazuh.list
sudo apt update
sudo apt install wazuh-agent
```

### Configure agent to connect to manager

```bash
sudo nano /var/ossec/etc/ossec.conf
```

Update:

```xml
<server>
  <address>YOUR_MANAGER_IP</address>
  <protocol>tcp</protocol>
</server>
```

### Start agent

```bash
sudo systemctl daemon-reexec
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

---

## 🚪 Step 3: Install Wazuh Agent on Windows

1. Download agent: [https://packages.wazuh.com/4.x/windows/wazuh-agent-4.8.0.msi](https://packages.wazuh.com/4.x/windows/wazuh-agent-4.8.0.msi)
2. During install:
   - Manager IP
   - Port: 1514 (TCP)
3. Open Wazuh Agent Manager app and click **Start**

---

## 🔐 Step 4: Register Agent with Manager

### On Manager

```bash
sudo /var/ossec/bin/manage_agents
```

- Press `A` to add agent
- Set agent name (e.g., ubuntu-client)
- Copy key

### On Linux Agent

```bash
sudo /var/ossec/bin/agent-auth -m <WAZUH_MANAGER_IP> -p 1515 -A ubuntu-client
```

---

## 📊 Step 5: View Alerts in Wazuh Dashboard

### Use the following filter in the dashboard:

```
data.srcuser = admin
```

> This helps identify specific events in the logs.

---

## 📃 Sample Alert Rule Used

```xml
<data>
  <srcuser>admin</srcuser>
</data>
```

---

## 📊 Tools Used

- Wazuh 4.8
- VirtualBox
- Ubuntu 22.04 LTS
- Windows 10

---

## 📝 License

This project is for educational and personal use only.

---

## 👤 Author

**ANITH**\
Cybersecurity Enthusiast | SOC Analyst in Training

---

