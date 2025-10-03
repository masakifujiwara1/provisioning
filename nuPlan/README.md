# Install nuPlan
## devkit install
```
cd && git clone https://github.com/motional/nuplan-devkit.git && cd nuplan-devkit
```

## python install
```
sudo apt update
sudo apt install python3-pip
sudo apt install software-properties-common
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt-get install python3.9
sudo apt-get install python3.9-dev
```

## miniconda install (linux 64-bit)
```
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm ~/miniconda3/miniconda.sh
```
after install
```
source ~/miniconda3/bin/activate
conda init --all
```

## conda env create
```
conda env create -f environment.yml
```
