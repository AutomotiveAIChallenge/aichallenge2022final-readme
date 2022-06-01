# Autoware Documentation Guide
Official Documents is [here](https://autowarefoundation.github.io/autoware-documentation/main/).

## Installtion
The way to install and setup Autoware is written in [installation_jp.md](./installation_jp.md) and [installation_en.md](./installation_en.md).

Note that AI challenge participations should not read the installation of the official documents and should read the above file.


## Degine Documentation
Autoware is composed to some modules (mainly Sensing, Localization, Perception, Planning, and Control).
[Node diagram of Autoware Universe](https://tier4.github.io/autoware-documentation/latest/design/node-diagram/) is a node graph which shows the relations of each module.
The source code of each module is shown in below table (Repository is [here](https://github.com/tier4/autoware.universe/tree/cc39553aec1ce42206448249fac4bfdb9a68f96c)).

| Module name  | Source code path | Explanation | 
| ------------ | ---------------- | ----------- | 
| Sensing      | $WORKSPACE/src/autoware/universe/sensing | This module preprocesses sensor data from LiDAR. | 
| Localization | $WORKSPACE/src/autoware/universe/localization | This module estimates the current pose of the ego vehicle. | 
| Perception   | $WORKSPACE/src/autoware/universe/perception | This module detects surrounding objects by using the sensor data from the Sensing module. | 
| Planning     | $WORKSPACE/src/autoware/universe/planning | This module plans the trajectory from current pose to the goal position. | 
| Control      | $WORKSPACE/src/autoware/universe/control | This module calculates speeds and angular to follow along with the trajectory. | 

## Precautions when modifying the source code

- Don't change the code of the limited speed written in lanelet2_map.osm file.
