# Dockerise-A-PostgreSQL-database-with-AWS-ec2-Instance


# 1. Launch ec2
- Ubuntu Server 22.04 LTS (HVM), SSD Volume Type

# 2. Install Docker

## Updating the apt package index
sudo apt update && sudo apt upgrade -y
## Install the packages that will allow apt to download the repository over https
sudo apt install ca-certificates curl gnupg lsb-release
## Add Docker’s official GPG key
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
## Set up the repository
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

## Update the apt package index
sudo apt-get update

## Install the latest version of the docker tools; docker-cli, docker compose, etc
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

## Check Docker version
docker -v

## Check for the status of your Docker
systemctl status docker --no-pager -l

## Add your Ubuntu user to the Docker Group
sudo usermod -aG docker $USER

## Check if the current user is in the Docker group
id $USER

## Reload your session
newgrp docker

# 3. Pull Postgres Image
docker run --name postgresql -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=post123 -p 5432:5432 -v /data:/var/lib/postgresql/data -d postgres:alpine

# 4. Check for the docker image that’s running
docker ps

# 5. Edit your security group to allow remote users to access your Postgres DB
- Protocol : TCP
- Port : 5432
- Source : 0.0.0.0/0

# 6. Access it from a Postgres terminal
psql -h public_ip_adress -p 5432 -U postgres
