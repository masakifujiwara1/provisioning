# Install nuPlan
## docker 
```
git clone -b 11.1.1-devel-ubuntu20.04-nuPlan git@github.com:masakifujiwara1/cudagl_docker.git
cd cudagl_docker
docker compose up gpu -d
./login.sh
```

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
### install requirements
```
pip3 install -e .
pip3 install -r requirements_torch.txt
pip3 install -r requirements.txt
pip3 install "timm>=0.4.12,<0.6.0"
pip3 install "Pillow<10.0.0"
```
### running scripts
change dir
```
cd nuplan-devkit
```
training
```
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
simulation with simple_planner
```
python3 nuplan/planning/script/run_simulation.py \
    +simulation=open_loop_boxes \
    planner=simple_planner \
    scenario_builder=nuplan_mini \
    scenario_filter=all_scenarios \
    scenario_filter.scenario_types="[near_multiple_vehicles, on_pickup_dropoff, starting_unprotected_cross_turn, high_magnitude_jerk]" \
    scenario_filter.num_scenarios_per_type=10
```
simulation with ml_planner
```
python3 nuplan/planning/script/run_simulation.py \
    '+simulation=open_loop_boxes' \
    'model=raster_model' \
    'planner=ml_planner' \
    'planner.ml_planner.model_config=${model}' \
    'planner.ml_planner.checkpoint_path="/home/ubuntu/nuplan/exp/exp/raster_experiment/raster_model/2025.10.06.16.45.38/best_model/epoch=9-step=409.ckpt"' \
    'scenario_builder=nuplan_mini' \
    'scenario_filter=all_scenarios' \
    'scenario_filter.scenario_types=[near_multiple_vehicles, on_pickup_dropoff, starting_unprotected_cross_turn, high_magnitude_jerk]' \
    'scenario_filter.num_scenarios_per_type=10'
```
dashboard
```
python3 nuplan/planning/script/run_nuboard.py
```
<img width="2560" height="1440" alt="image" src="https://github.com/user-attachments/assets/735ff440-af3c-49be-b922-79104fdd6159" />
