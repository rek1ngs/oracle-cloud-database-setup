# Oracle 21XE on Oracle Cloud VM

## About the project
This project demonstrates how to deploy Oracle Database 21c Express Edition (XE) inside a Docker container running on an Oracle Cloud Infrastructure (OCI) VM instance.
It shows full configuration -- from SSH setup to database connection in SQL Developer.

## Environment
| Component | Version |
|-----------|---------|
|Oracle Cloud VM | VM.Standard.E3.Flex |
|RAM | 3GB |
| OS | Oracle Linux 9 |
|Docker | 28.5.1 |
|Oracle Database | 21c Express Edition |  
| SQL Devoloper | 24.3.1 (Mac ARM) |

## Deployment steps
```bash
# 1. Connect to Oracle Cloud VM
ssh -i <your_key>.key opc@<your_public_ip>

# 2. Add swap memory for system
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
sudo bash -c 'echo "/swapfile none swap sw 0 0" >> /etc/fstab'
# Checking
free -h

# 3. Update and install Docker
sudo dnf 
sudo dnf remove -y podman-docker podman
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo dnf install -y docker-ce docker-ce-cli containerd.io
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -aG docker opc
exit
#reconnect

# 3. Run Oracle XE 21c container
docker run -d --name oracle21xe \
  -p 1521:1521 \
  -e ORACLE_PASSWORD=MySecurePass123 \
  --shm-size=2g \
  gvenzl/oracle-xe:21-slim

# 4. Check logs
docker logs -f oracle21xe
```

Expected result:  
`DATABASE IS READY TO USE!`

## OCI Security Rules
| Protocol | Port | Source CIDR | Description |
|----------|------|-------------|-------------|
| TCP | 22 | <your_ip>/32 | SSH access|
| TCP | 1521| <your_ip>/32 | SQL Devoloper connection |
