# linear_actuator_calibration_kridtbot

This repository provides tools and modules for calibrating the linear actuators used in the Kridtbot mobile robot. The package includes calibration routines, configuration templates, and test scripts that support accurate mapping of actuator commands to physical displacement.

## Repository Structure

├── launch/ # Launch files (ROS integration or experiment launching)

├── linear_actuator_calibration/ # Main Python package with calibration code

├── resource/ # Resource files (configuration templates, calibration data)
├── test/ # Test scripts and validation tools
├── setup.py # Python installation script
├── setup.cfg # Package metadata configuration
├── package.xml # ROS package manifest (if used in a ROS workspace)
├── LICENSE.md # License information (Apache 2.0)
└── README.md # Project documentation


## Features

- Calibration routines for linear actuators on the Kridtbot platform
- Resource and configuration templates to assist calibration workflows
- Validation and test scripts to verify actuator behavior
- ROS-compatible structure (via `package.xml` and `launch/`)
- Installable as a Python package using `setup.py`

## Installation

Clone the repository:

```bash
git clone https://github.com/Crelle490/linear_actuator_calibration_kridtbot.git
cd linear_actuator_calibration_kridtbot

