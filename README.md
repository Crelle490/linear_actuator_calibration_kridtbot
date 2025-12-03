# linear_actuator_calibration_kridtbot
[![Ask DeepWiki](https://devin.ai/assets/askdeepwiki.png)](https://deepwiki.com/Crelle490/linear_actuator_calibration_kridtbot/tree/main)

This ROS 2 package provides a node for the automatic calibration of linear actuators on the Kridtbot mobile robot. The calibration routine determines the physical limits of the actuators by monitoring joint effort (torque) and establishes a new zero reference position for each actuator

## Prerequisites

*   ROS 2
*   `ros2_control` framework configured on the robot.
*   A `joint_state_broadcaster` publishing the states of the target joints.
*   Configured `linear_velocity_controller` and `linear_position_control` controllers under a `controller_manager`.

## Installation

1.  Clone this repository into the `src` directory of your ROS 2 workspace:
    ```bash
    cd your_ros2_ws/src
    git clone https://github.com/Crelle490/linear_actuator_calibration_kridtbot.git
    ```

2.  Navigate to your workspace root and install dependencies:
    ```bash
    cd ..
    rosdep install --from-paths src --ignore-src -r -y
    ```

3.  Build the package:
    ```bash
    colcon build --packages-select linear_actuator_calibration
    ```

## Usage

1.  Source your ROS 2 workspace:
    ```bash
    source install/setup.bash
    ```

2.  Launch the calibration sequence:
    ```bash
    ros2 launch linear_actuator_calibration calibration.launch.py
    ```
    This will start the `calibration_sequence` node, which executes the entire calibration process and shuts down upon completion.

## Calibration Process

The `calibration_sequence` node performs the following steps to calibrate the four linear actuators (`left_front_linear`, `left_rear_linear`, `right_front_linear`, `right_rear_linear`):

1.  **Initiation**: The node starts by sending negative velocity commands to the `linear_velocity_controller/commands` topic, causing the actuators to retract towards their physical limits.

2.  **End-Stop Detection**: While the actuators are moving, the node subscribes to the `/joint_states` topic and monitors the `effort` field for each actuator.

3.  **Zeroing**: When an actuator's effort exceeds a threshold of `0.20`, the node concludes it has reached its physical end-stop. It records the current joint `position` as the calibrated relative zero position for that actuator.

4.  **Controller Switch**: After all four actuators have been successfully zeroed, the node calls the `/controller_manager/switch_controller` service. It deactivates the `linear_velocity_controller` and activates the `linear_position_control` controller.

5.  **Finalization**: Upon a successful controller switch, the node publishes a final target position to the `linear_position_control/commands` topic to move the actuators to a known starting state relative to their newly found zero points. The node then destroys itself, completing the calibration.

## Package Structure

```
.
├── LICENSE.md                  # Apache 2.0 License
├── README.md                   # This documentation file
├── launch/
│   └── calibration.launch.py   # ROS 2 launch file to start the calibration node
├── linear_actuator_calibration/
│   └── calibration_sequence.py # Python source for the calibration ROS 2 node
├── package.xml                 # ROS 2 package manifest
├── resource/                     # ament resource index marker
├── setup.cfg                   # Python package install configuration
├── setup.py                    # Python setup script with console entry points
└── test/                       # Linters and copyright tests
```

## License

This project is licensed under the Apache 2.0 License. See the [LICENSE.md](LICENSE.md) file for details.
