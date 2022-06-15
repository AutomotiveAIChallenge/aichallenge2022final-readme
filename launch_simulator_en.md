# Method to launch simulation
## PSim
Planning Simulator.

Planning simulator can check the control behavior (Planning and Control) by using source codes under the "src" directory.

1. Launch Psim
```
$ source /opt/ros/galactic/setup.bash
$ cd ~/aichallenge2021final
$ source install/setup.bash
$ cd scripts
$ ./psim.sh
```

2. Setting
- Click "2D Pose Estimate" on Rviz, and click and drag on the map to set the initial position.
- Set "2D Goal Pose" in the same way.
- By pushing "Engage" at the bottom left, the ego vehicle will start moving along the trajectory.



## LSim
Logging Simulator (ROSBAG Simulator).

Logging simulator, by planing ROSBAG, checks the results of the Localization, Perception, and Planning modules.

1. Launch Lsim
```
$ source /opt/ros/galactic/setup.bash
$ cd ~/aichallenge2021final
$ source install/setup.bash
$ cd scripts
$ ./lsim.sh
```

2. Playing ROSBAG
```
$ ros2 bag play /path/to/rosbag.db3
```

3. Setting
- Click "2D Pose Estimate" on Rviz, and click and drag the point that seems to be the ego vehicle position.
- If it is out of alignment, repeat "2D Pose Estimate".
- Set "2D Goal Pose" and check the results of the Localization, Perception, and Planning modules.
