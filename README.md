# Jetson-Nano-Edge-Setup

## Install dependencies
```
sudo apt-get -y update && sudo apt-get -y upgrade
sudo apt-get install python3-pip curl nano build-essential gcc g++ make ufw git
pip install --upgrade pip
pip install numpy matplotlib scikit-learn PyMongo
```

### If facing /var/lib/apt/lists/lock problem
```
sudo rm-ref /var/lib/apt/lists/lock
sudo apt-get -y update && sudo apt-get -y upgrade
```

## Install tensorflow
### Based on NVIDIA official website
```
sudo apt-get update
sudo apt-get install libhdf5-serial-dev hdf5-tools libhdf5-dev zlib1g-dev zip libjpeg8-dev liblapack-dev libblas-dev gfortran
sudo apt-get install python3-pip
sudo pip3 install -U pip testresources setuptools==49.6.0
sudo pip3 install -U numpy==1.19.4 future==0.18.2 mock==3.0.5 h5py==2.10.0 keras_preprocessing==1.1.1 keras_applications==1.0.8 gast==0.2.2 futures protobuf pybind11
sudo pip3 install --pre --extra-index-url https://developer.download.nvidia.com/compute/redist/jp/v46 tensorflow
```

## Install mosquitto
### Installing mosquitto
```
sudo apt-add-repository ppa:mosquitto-dev/mosquitto-ppa
sudo apt-get update
sudo apt-get install mosquitto
sudo apt-get install mosquitto-clients
sudo apt clean
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
wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | sudo apt-key add -
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list
sudo apt-get update
sudo apt-get install -y mongodb-org=4.2.15 mongodb-org-server=4.2.15 mongodb-org-shell=4.2.15 mongodb-org-mongos=4.2.15 mongodb-org-tools=4.2.15
```

### Start mongoDB service
```
sudo systemctl daemon-reload
sudo systemctl start mongod
sudo systemctl status mongod
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

### Remove mongodb
```
sudo apt-get purge mongodb*
```

## Install node-red
### Install node.js v16
```
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
sudo apt-get install -y nodejs
```

### Install node-red
```
sudo npm install -g --unsafe-perm node-red node-red-admin
```

### Allow port use
```
sudo ufw allow 1880
```

### Run node-red
```
node-red
```

### Load flow files
1. Download from the drive, open node-red folder
2. Copy all files to /home/jetson/.nodered
3. Open browser, go to 127.0.0.1:1880
4. On the right side, find import
5. Import the files inside /home/jetson/.nodered

### Run node-red on startup
```
sudo npm install -g pm2
pm2 start /usr/bin/node-red --node-args="--max-old-space-size=256" -- -v
pm2 save
pm2 startup systemd
follow the command
reboot
```

## Enable VNC
```
cd /usr/lib/systemd/user/graphical-session.target.wants
sudo ln -s ../vino-server.service ./.
gsettings set org.gnome.Vino prompt-enabled false
gsettings set org.gnome.Vino require-encryption false
reboot
```

## Check the port
1. mongoDB port
```
netstat -n | grep 27017
```
2. MQTT port
```
netstat -n | grep 1883
```
