# Snort IDS + Nmap Scan Detection Lab

This project simulates real-world intrusion detection using **Snort IDS** on Ubuntu Server and **Nmap scanning** from Kali Linux.

## 🛠️ Lab Setup
- Ubuntu Server with Snort installed
- Kali Linux used to launch scans
- Both connected via Bridged Networking
- Interface set to promiscuous mode

## 🔍 Nmap Scans Performed
- `nmap -sS` (Stealth SYN Scan)
- `nmap -A` (Aggressive Scan)

## 🧪 Snort Rule Example
```snort
alert tcp any any -> $HOME_NET any (msg:"Nmap Scan Detected"; flags:S; sid:1000002; rev:1;)
