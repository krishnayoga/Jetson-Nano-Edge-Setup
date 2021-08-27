# Jetson-Nano-Edge-Setup

## Install dependencies
```
1. sudo apt-get -y update && sudo apt-get -y upgrade
2. sudo apt-get install python3-pip
3. pip install --upgrade pip
4. pip install numpy matplotlib nano build-essential scikit-learn PyMongo
```

### If facing /var/lib/apt/lists/lock problem
```
1. sudo rm-ref /var/lib/apt/lists/lock
2. sudo apt-get -y update && sudo apt-get -y upgrade
```

## Install tensorflow
### Based on NVIDIA official website
```
1. sudo apt-get update
2. sudo apt-get install libhdf5-serial-dev hdf5-tools libhdf5-dev zlib1g-dev zip libjpeg8-dev liblapack-dev libblas-dev gfortran
3. sudo apt-get install python3-pip
4. sudo pip3 install -U pip testresources setuptools==49.6.0
5. sudo pip3 install -U numpy==1.19.4 future==0.18.2 mock==3.0.5 h5py==2.10.0 keras_preprocessing==1.1.1 keras_applications==1.0.8 gast==0.2.2 futures protobuf pybind11
6. sudo pip3 install --pre --extra-index-url https://developer.download.nvidia.com/compute/redist/jp/v46 tensorflow
```

## Install mosquitto
### Installing mosquitto
```
1. sudo apt-add-repository ppa:mosquitto-dev/mosquitto-ppa
2. sudo apt-get update
3. sudo apt-get install mosquitto
4. sudo apt-get install mosquitto-clients
5. sudo apt clean
```

### Configuring mosquitto bridge connection
1. Download the bridge configuration file from here https://drive.google.com/drive/folders/1KevxM8qMyg8kD67fGy5KnDW1qf52OCia?usp=sharing
2. Move the file to this directory /etc/mosquitto/conf.d/

### Check incoming message
```
mosquitto_sub -t '#'
```

## Install mongoDB
### Install mongoDb v4.2
```
1. wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | sudo apt-key add -
2. echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list
3. sudo apt-get update
4. sudo apt-get install -y mongodb-org=4.2.15 mongodb-org-server=4.2.15 mongodb-org-shell=4.2.15 mongodb-org-mongos=4.2.15 mongodb-org-tools=4.2.15
```

### Start mongoDB service
```
1. sudo systemctl daemon-reload
2. sudo systemctl start mongod
3. sudo systemctl status mongod
```

### Create database on mongoDB
1. Start mongo
```
mongo
```
2. Create database TestMQTT
```
use TestMQTT
```
3. Check list of database
```
show dbs
db
```

### Open database access to all computers
1. Open mongod.conf file
```
sudo nano /etc/mongod.conf
```
2. Edit line bindIP from 127.0.0.1 to 0.0.0.0

### Start mongoDB every startup
1. Download the file from https://drive.google.com/drive/folders/1KevxM8qMyg8kD67fGy5KnDW1qf52OCia?usp=sharing. MongoDB Data folder
2. Download file db_node.sh and esge_server.service
3. Move the db_node.sh to /usr/local/bin/
4. Open terminal, compile the .sh file
```
sudo chmod +x /usr/local/bin/db_node.sh
```
5. Move the edge_server.service file to /etc/systemd/system/
6. Open terminal, run these command
```
sudo systemctl enable edge_server.service
sudo systemctl start edge_server.service
```
