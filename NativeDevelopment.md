# Native Development

This guide is meant to run DLS natively for development and assume that you have already done the installation at [Installation and Preparation Guide](./Installation.md)

## Prerequisties

Extra dependencies are needed in order to run DLS.

- Nodejs (Verified with 10.15.3)
- Redis (Verified with 5.0)
- MongoDB (verified with 4.0)
- Mosquitto

**Note**: Redis, MongoDB and Mosquitto are databases or message broker. The guide below only covers basic installation of the components needed in DLS. Extra steps not covered in this guide is needed to secure the databases or message broker.

1. Configure Nodejs PPA

   ```bash
   cd /tmp
   wget https://deb.nodesource.com/setup_10.x -o nodesource_setup.sh

   sudo bash nodesource_setup.sh

   ```

2. Add MongoDB repository

   ```bash
    sudo -E apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
   echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
   ```

3. Install all the dependencies required.

   ```bash
   sudo apt-get update
   sudo -E apt install -y git nodejs redis-server mongodb-org python3-pip python-dev openssh-server graphviz python3-tk protobuf-compiler python3-pil python3-lxml python3-tk socat python3-numpy mosquitto
   ```

4. Install Nodejs third party library for UI and frontend server

```bash

   cd $DLS_HOME/DeepLearningSuite

   npm install
```

5. Run the UI and frontend server

```bash

cd $DLS_HOME/DeepLearningSuite

npm run build
node .
```

6. On a separate terminal, start the training job dispatcher

   ```bash

   cd $DLS_HOME/NNFramework/tf
   source env.sh
   python3 jobDispatcher.py
   ```

7. On another terminal, start the remote agent dispatcher

   ```bash
   cd $DLS_HOME/NNFramework/tf
   source env.sh
   python3 remoteAgent.py

   ```

8. Open a web browser and go to [https://localhost:8080](https://localhost:8080). The default username and password is

   username: user1@user1.com
   password: user1
