# Jetson-Nano-Edge-Setup

## Install dependencies
```
1. sudo apt-get -y update && sudo apt-get -y upgrade
2. sudo apt-get install python3-pip
3. pip install --upgrade pip
4. pip install numpy matplotlib nano build-essential scikit-learn
```

### If facing /var/lib/apt/lists/lock problem
```
1. sudo rm-ref /var/lib/apt/lists/lock
2. sudo apt-get -y update && sudo apt-get -y upgrade
```
