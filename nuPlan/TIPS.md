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
https://github.com/motional/nuplan-devkit/blob/e9241677997dd86bfc0bcd44817ab04fe631405b/nuplan/planning/script/experiments/simulation/closed_loop_reactive_agents.yaml#L5
https://github.com/motional/nuplan-devkit/blob/e9241677997dd86bfc0bcd44817ab04fe631405b/nuplan/planning/script/config/simulation/observation/idm_agents_observation.yaml#L1

Python file directory
https://github.com/motional/nuplan-devkit/tree/master/nuplan/planning/simulation/observation
- ego_centric_ml_agents: machine learning agents
- idm_agents: intelligent driver model
- tracks_observation: log replay
