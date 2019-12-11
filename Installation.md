# Installation and Preparation

## Dependencies

- Cython
- Tensorflow
- networkx
- numpy
- pymongo
- protobuf
- redis
- opencv-python
- matplotlib
- contextlib2
- paho-mqtt
- setuptools
- pycocotools
- graphviz
- requests
- sklearn
- test-generator==0.1.1
- defusedxml
- jpeg2dct

For simplicity, set the root directory of DLS as a variable.

```bash
export DLS_HOME=`pwd`

```

The libraries can be installed on Ubuntu 16.04 as follows:

```bash
sudo apt-get install protobuf-compiler python3-pil python3-lxml python3-tk libjpeg-dev
pip install Cython
pip install numpy

# For CPU
pip install -r NNFramework/tf/requirements.txt
# For GPU
pip install -r NNFramework/tf/requirements-gpu.txt

```

**Note**: GPU required CUDA enabled hardware and extra dependencies which will not be covered in the installation guide

## Install Tensorflow Object Detection API

1. Download Object Detection API

   ```bash
   cd $DLS_HOME
   git clone https://github.com/tensorflow/models
   cd models
   git checkout e80b385a20bee6a21af1683424575bea7c8457bc #To be safe, checkout the version that we have verified

   ```

2. Install COCO API

   ```bash
   cd $DLS_HOME
   git clone https://github.com/cocodataset/cocoapi.git
   cd cocoapi/PythonAPI
   make
   cp -r pycocotools $DLS_HOME/models/research/

   ```

3. Compile protobuf

   ```bash
   cd $DLS_HOME/models/research
   protoc object_detection/protos/*.proto --python_out=.
   ```

   **Note**: make sure the protobuf-compiler >= 3.2.0 otherwise try installing it manually as follows:

   ```bash
   cd $DLS_HOME/models/research
   wget -O protobuf.zip https://github.com/google/protobuf/releases/download/v3.0.0/protoc-3.0.0-linux-x86_64.zip
   unzip protobuf.zip
   ```

   Run the compilation process again

   ```bash
   ./bin/protoc object_detection/protos/*.proto --python_out=.

   ```

4. Copy the object detection module to the right path

   ```bash
   cd $DLS_HOME/models/research
   cp -r object_detection ../../NNFramework/tf/
   cp -r slim ../../NNFramework/tf/
   ```

5. Compile file format proto

   ```bash
       cd $DLS_HOME/NNFramework/tf/protos
       protoc label.proto --python_out=.
   ```

## Download Tensorflow Object Detection Pretrained Model

Download object detection pretrained model from [Tensorflow detection model zoo](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md). This is the base model where the solution will fine tune to your dataset. The following is a sample command line script to download the models.

```bash
cd $DLS_HOME/NNFramework/tf/pretrained_model

#for ssd_mobilenet_v1
wget http://download.tensorflow.org/models/object_detection/ssd_mobilenet_v1_coco_2018_01_28.tar.gz
tar -xvf ssd_mobilenet_v1_coco_2018_01_28.tar.gz

# for ssd_mobilenet_v2
wget http://download.tensorflow.org/models/object_detection/ssd_mobilenet_v2_coco_2018_03_29.tar.gz
tar -xvf ssd_mobilenet_v2_coco_2018_03_29.tar.gz

# for ssd_inception_v2
wget http://download.tensorflow.org/models/object_detection/ssd_inception_v2_coco_2018_01_28.tar.gz
tar -xvf ssd_inception_v2_coco_2018_01_28.tar.gz

# for rfcn_resnet101
wget http://download.tensorflow.org/models/object_detection/rfcn_resnet101_coco_2018_01_28.tar.gz
tar -xvf rfcn_resnet101_coco_2018_01_28.tar.gz

# for ssd_resnet50
wget http://download.tensorflow.org/models/object_detection/ssd_resnet50_v1_fpn_shared_box_predictor_640x640_coco14_sync_2018_07_03.tar.gz
tar -xvf ssd_resnet50_v1_fpn_shared_box_predictor_640x640_coco14_sync_2018_07_03.tar.gz

# for faster_rcnn_inception_v2
wget http://download.tensorflow.org/models/object_detection/faster_rcnn_inception_v2_coco_2018_01_28.tar.gz
tar -xvf faster_rcnn_inception_v2_coco_2018_01_28.tar.gz

# for faster_rcnn_inception_resnet_v2
wget http://download.tensorflow.org/models/object_detection/faster_rcnn_inception_resnet_v2_atrous_coco_2018_01_28.tar.gz
tar -xvf faster_rcnn_inception_resnet_v2_atrous_coco_2018_01_28.tar.gz

# for faster_rcnn_nas
wget http://download.tensorflow.org/models/object_detection/faster_rcnn_nas_coco_2018_01_28.tar.gz
tar -xvf faster_rcnn_nas_coco_2018_01_28.tar.gz

# for faster_rcnn_resnet50
wget http://download.tensorflow.org/models/object_detection/faster_rcnn_resnet50_coco_2018_01_28.tar.gz
tar -xvf faster_rcnn_resnet50_coco_2018_01_28.tar.gz

# for faster_rcnn_resnet101
wget http://download.tensorflow.org/models/object_detection/faster_rcnn_resnet101_coco_2018_01_28.tar.gz
tar -xvf faster_rcnn_resnet101_coco_2018_01_28.tar.gz

```

## Install Intel OpenVINO at /opt/intel

1. Download the Intel® Distribution of OpenVINO™ toolkit package file from [Intel® Distribution of OpenVINO™ toolkit for Linux\*](https://software.intel.com/en-us/openvino-toolkit/choose-download?elq_cid=2822695&erpm_id=5437487). Select the Intel® Distribution of OpenVINO™ toolkit for Linux package from the dropdown menu.

2. Change directories to where you downloaded the Intel Distribution of OpenVINO toolkit for Linux\* package file.
   If you downloaded the package file to the current user's Downloads directory:

   ```bash
   cd ~/Downloads
   ```

3. Unpack the .tgz file:

   ```bash
   tar -xvzf l_openvino_toolkit_p_<version>.tgz
   ```

4. Go to the l_openvino_toolkit_p_version directory

   ```bash
   cd l_openvino_toolkit_p_<version>
   ```

5. Run the installation executable.

   ```bash
   sudo ./install_GUI.sh
   ```

6. Follow the steps shown on the GUI.

7. Install OpenVINO dependencies

   ```bash
   cd /opt/intel/openvino/install_dependencies
   sudo -E ./install_openvino_dependencies.sh
   ```

8. Configure OpenVINO Model Optimizer

   ```bash

   cd /opt/inte/openvino/deployment_tools/model_optimizer/install_prerequisites
   sudo ./install_prerequisites.sh

   ```

## Setup TLS remote agent (client) for EIS v1.5

1. Prior to setup TLS remote agent (client) for EIS v1.5 edge device, make sure the EIS v1.5 has been setup and running successfully on edge device by following the EIS v1.5 setup guide.

2. From TLS parent directory, copy the "ClientRemoteAgent" directory to EIS v1.5 parent directory.

3. Prior to start the remote agent (client) in EIS v1.5 edge device, first setup the enviroment.

   ```bash
   cd ClientRemoteAgent/ClientAgent
   source setenv.sh
   ```

4. Update the config.json file based on EIS v1.5 cameras setup.

5. Start the remote agent (client) in EIS v1.5 edge device with the options below, where:
   -dev true (EIS v1.5 running in development mode. If EIS v1.5 running in production mode, use -dev false)
   -host <TLS_IP_address> IP address for TLS server

   python3.6 client_remote_agent.py -dev true -host <TLS_IP_address>
