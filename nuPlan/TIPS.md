# TIPS
## Change simulation time
Default scenario duration: 15s  
Change the 15.0 in the following code section to any value you like.

https://github.com/motional/nuplan-devkit/blob/e9241677997dd86bfc0bcd44817ab04fe631405b/nuplan/planning/script/config/common/scenario_builder/scenario_mapping/nuplan_scenario_mapping.yaml#L15
```
scenario_map:
  # scenario_name: [scenario_duration, extraction_offset, subsample_ratio]
  accelerating_at_crosswalk: [15.0, -3.0]
  accelerating_at_stop_sign: [15.0, -3.0]
```

## Change planner for other agents
ex) closed_loop_reactive_agents: idm  
Replace the Python file specified by hydra  
- https://github.com/motional/nuplan-devkit/blob/e9241677997dd86bfc0bcd44817ab04fe631405b/nuplan/planning/script/experiments/simulation/closed_loop_reactive_agents.yaml#L5  
- https://github.com/motional/nuplan-devkit/blob/e9241677997dd86bfc0bcd44817ab04fe631405b/nuplan/planning/script/config/simulation/observation/idm_agents_observation.yaml#L1

Python file directory  
https://github.com/motional/nuplan-devkit/tree/master/nuplan/planning/simulation/observation
- ego_centric_ml_agents: machine learning agents
- idm_agents: intelligent driver model
- tracks_observation: log replay

### Ex. Add custom agents
1. Add [Hydra config](https://github.com/masakifujiwara1/nuplan-devkit/blob/dev/nuplan/planning/script/config/simulation/observation/simple_control_agents_observation.yaml)  
2. Add [Python file](https://github.com/masakifujiwara1/nuplan-devkit/blob/dev/nuplan/planning/simulation/observation/simple_control_agents.py)

Run simulation
```
python3 nuplan/planning/script/run_simulation.py \
    +simulation=open_loop_boxes \
    planner=simple_planner \
    scenario_builder=nuplan_mini \
    scenario_filter=all_scenarios \
    scenario_filter.scenario_types="[near_multiple_vehicles, on_pickup_dropoff, starting_unprotected_cross_turn, high_magnitude_jerk]" \
    scenario_filter.num_scenarios_per_type=10 \
    observation=simple_control_agents_observation
```

## Ex. Use [Diffusion-Planner](https://github.com/ZhengYinan-AIR/Diffusion-Planner.git)
Inside docker 
```
cd
git clone https://github.com/ZhengYinan-AIR/Diffusion-Planner.git && cd Diffusion-Planner
pip3 install -e .
pip3 install mmengine
```
Change sim_diffusion_planner_runner.sh
```
export CUDA_VISIBLE_DEVICES=0
export HYDRA_FULL_ERROR=1

###################################
# User Configuration Section
###################################
# Set environment variables
export NUPLAN_DEVKIT_ROOT="/home/ubuntu/nuplan-devkit"  # nuplan-devkit absolute path (e.g., "/home/user/nuplan-devkit")
# export NUPLAN_DATA_ROOT="REPLACE_WITH_DATA_DIR"  # nuplan dataset absolute path (e.g. "/data")
# export NUPLAN_MAPS_ROOT="REPLACE_WITH_MAPS_DIR" # nuplan maps absolute path (e.g. "/data/nuplan-v1.1/maps")
# export NUPLAN_EXP_ROOT="REPLACE_WITH_EXP_DIR" # nuplan experiment absolute path (e.g. "/data/nuplan-v1.1/exp")

# Dataset split to use
# Options: 
#   - "test14-random"
#   - "test14-hard"
#   - "val14"
SPLIT="val14"  # e.g., "val14"

# Challenge type
# Options: 
#   - "closed_loop_nonreactive_agents"
#   - "closed_loop_reactive_agents"
CHALLENGE="closed_loop_reactive_agents"  # e.g., "closed_loop_nonreactive_agents"
###################################


BRANCH_NAME=diffusion_planner_release
ARGS_FILE=/home/ubuntu/Diffusion-Planner/checkpoints/args.json
CKPT_FILE=/home/ubuntu/Diffusion-Planner/checkpoints/model.pth

if [ "$SPLIT" == "val14" ]; then
    SCENARIO_BUILDER="nuplan"
else
    SCENARIO_BUILDER="nuplan_challenge"
fi
echo "Processing $CKPT_FILE..."
FILENAME=$(basename "$CKPT_FILE")
FILENAME_WITHOUT_EXTENSION="${FILENAME%.*}"

PLANNER=diffusion_planner
SCENARIO_BUILDER="nuplan_mini"

python3 $NUPLAN_DEVKIT_ROOT/nuplan/planning/script/run_simulation.py \
    +simulation=$CHALLENGE \
    planner=$PLANNER \
    planner.diffusion_planner.config.args_file=$ARGS_FILE \
    planner.diffusion_planner.ckpt_path=$CKPT_FILE \
    scenario_builder=$SCENARIO_BUILDER \
    scenario_filter=$SPLIT \
    experiment_uid=$PLANNER/$SPLIT/$BRANCH_NAME/${FILENAME_WITHOUT_EXTENSION}_$(date "+%Y-%m-%d-%H-%M-%S") \
    verbose=true \
    worker=ray_distributed \
    worker.threads_per_node=128 \
    distributed_mode='SINGLE_NODE' \
    number_of_gpus_allocated_per_simulation=1.0 \
    enable_simulation_progress_bar=true \
    hydra.searchpath="[pkg://diffusion_planner.config.scenario_filter, pkg://diffusion_planner.config, pkg://nuplan.planning.script.config.common, pkg://nuplan.planning.script.experiments  ]"
```
