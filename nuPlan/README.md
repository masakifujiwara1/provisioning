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

## using virtual python environment
### miniconda install (linux 64-bit)
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

### conda env create
```
conda env create -f environment.yml
```

## without virtual python environment
### alternative python version
```
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.9 1
```
check python version
```
python --version
```
### install requirements
```
pip3 install -e .
pip3 install -r requirements_torch.txt
pip3 install -r requirements.txt
pip3 install "timm>=0.4.12,<0.6.0"
pip3 install "Pillow<10.0.0"
```
### running scripts
```
cd nuplan-devkit
python3 nuplan/planning/script/run_training.py \
    experiment_name=raster_experiment \
    py_func=train \
    +training=training_raster_model \
    scenario_builder=nuplan_mini \
    scenario_filter.limit_total_scenarios=500 \
    lightning.trainer.params.max_epochs=10 \
    data_loader.params.batch_size=8 \
    data_loader.params.num_workers=8
```
after
```
python3 nuplan/planning/script/run_nuboard.py
```
