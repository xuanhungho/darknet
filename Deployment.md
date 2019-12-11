# Deployment

This guide is meant to deploy DLS onto system that does not have the source code and assume that you have already done the installation stated in [Installation and Preparation Guide](./Installation.md) as well as stated in [Docker](./Docker.md) We recommend deploying DLS as docker container to simplify the dependency installation.

## Prerequisties

The following dependencies are needed in both the host machine and the target machine in order to create docker image of DLS.

- Docker
- Docker compose

1. Build docker image

   ```bash
   cd $DLS_HOME
   docker-compose build

   ```

2. Export the docker images from repository

   ```bash
   docker save dlsui:v1.0.0 > dlsuiv1.0.0.tar
   docker save dlscore:v1.0.0 > dlscorev1.0.0.tar
   ```

3. Copy/Send the docker images to the target machine and load

   ```bash
   docker load < dlsuiv1.0.0.tar
   docker load < dlscorev1.0.0.tar
   ```

4. Copy over docker-compose-deploy.yml to the target machine

5. Start the containers on the target machine

   ```bash
   docker-compose -f docker-compose-deploy.yml up -d
   ```

6. Open a web browser and go to [https://localhost:8080](https://localhost:8080) on the target machine. The default username and password is

   username: user1@user1.com
   password: user1
