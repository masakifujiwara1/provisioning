# TIPS
## change simulation time
default scenario duration: 15s  
change the 15.0 in the following code section to any value you like.

https://github.com/motional/nuplan-devkit/blob/e9241677997dd86bfc0bcd44817ab04fe631405b/nuplan/planning/script/config/common/scenario_builder/scenario_mapping/nuplan_scenario_mapping.yaml#L15
```
scenario_map:
  # scenario_name: [scenario_duration, extraction_offset, subsample_ratio]
  accelerating_at_crosswalk: [15.0, -3.0]
  accelerating_at_stop_sign: [15.0, -3.0]
```
