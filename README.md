# snort-nmap-detection-lab
Snort IDS lab setup with Nmap scan detection using Kali Linux and Ubuntu Server
# 🔒 Snort IDS + Nmap Scan Detection Lab

This project demonstrates how to set up a simple IDS lab using **Snort** on Ubuntu Server and simulate attacks using **Nmap** from Kali Linux.

## 🛠️ Lab Setup

- 🐧 **Ubuntu Server**: Snort IDS installed and running
- 🐱‍💻 **Kali Linux**: Used to perform Nmap scans
- 🌐 **Network**: Both VMs on the same bridged network

---

## ⚙️ Snort Setup on Ubuntu Server

### 1. Install Snort:
bash
sudo apt update
sudo apt install snort
During setup, set your network interface and HOME_NET (or manually later in /etc/snort/snort.conf).

2. Set Promiscuous Mode:
bash
sudo ip link set ens33 promisc on
Replace ens33 with your network interface (check using ip a)

3. Set HOME_NET in snort.conf:
bash
sudo nano /etc/snort/snort.conf

snort
ipvar HOME_NET 192.168.1.100      # Use the IP of your Ubuntu Server
Or use subnet:

snort
ipvar HOME_NET 192.168.1.0/24

4. Add Custom Rule (detect Nmap):
sudo nano /etc/snort/rules/local.rules
Paste:
alert tcp any any -> $HOME_NET any (msg:"Nmap Scan Detected"; flags:S; sid:1000002; rev:1;)
alert icmp any any -> $HOME_NET any (msg:"ICMP Packet Detected"; sid:1000001; rev:1;)

snort
include $RULE_PATH/local.rules

5. Test Configuration:
sudo snort -T -c /etc/snort/snort.conf

6. Run Snort:
sudo snort -i ens33 -A console -c /etc/snort/snort.conf

🧪 Attack Simulation from Kali Linux
From Kali, run:
nmap -sS 192.168.1.100     # SYN Stealth Scan
nmap -A 192.168.1.100      # Aggressive Scan
ping 192.168.1.100         # ICMP test

📊 Snort Output Example

If configured correctly, Snort will print:

[**] [1:1000002:1] Nmap Scan Detected [**]
[**] [1:1000001:1] ICMP Packet Detected [**]
