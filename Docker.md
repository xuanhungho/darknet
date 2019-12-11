# Docker

This guide is meant to run DLS on docker for development and assume that you have already done the installation at [Installation and Preparation Guide](./Installation.md)

## Prerequisties

The following dependencies are needed in order to run DLS on docker.

- Docker
- Docker compose
- OpenVINO installed at /opt/intel/openvino

1. Install Docker

   ```bash
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

   sudo apt-get update
   sudo apt-get install -y docker-ce
   ```

2. Install docker compose

   ```bash
   sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

   sudo chmod +x /usr/local/bin/docker-compose
   ```

3. Start all the container necessary.

   ```bash
   cd $DLS_HOME
   docker-compose up -d

   ```

4. Open a web browser and go to [https://localhost:8080](https://localhost:8080). The default username and password is

   username: user1@user1.com
   password: user1
