# Oracle 21XE on Oracle Cloud VM

## About the project
This project demonstrates how to deploy Oracle Database 21c Express Edition (XE) inside a Docker container running on an Oracle Cloud Infrastructure (OCI) VM instance.
It shows full configuration -- from SSH setup to database connection in SQL Developer.

## Environment
Component — Version  
Oracle Cloud VM — VM.Standard.E3.Flex  
OS — Oracle Linux 9  
Docker — 28.5.1  
Oracle Database — 21c Express Edition  
SQL Devoloper — 24.3.1 (Mac ARM)  

```bash
# 1. Connect to Oracle Cloud VM
ssh -i <your_key>.key opc@<your_public_ip>

# 2. Add swap memory for sistem
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
sudo bash -c 'echo "/swapfile none swap sw 0 0" >> /etc/fstab'

# Checking
free -h
```
