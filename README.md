# Simulation Models for PyBullet
A repository of realistic simulation models.

## Installation
To install, `cd` into the repository directory (the one with `setup.py`) and run:
```bash
pip install .
```
or
```bash
pip install -e .
```
The `-e` flag tells pip to install the package in-place, which lets you make changes to the code without having to reinstall every time. *Do not do this on shared workstations!*

You should then be able to import the package in the Python interpreter.
```python
$ python3
Python 3.8.5 (default, Jul 28 2020, 12:59:40) 
[GCC 9.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import pbsm
pybullet build time: Sep 22 2020 00:56:01
>>> from pbsm import UR5_2F85
>>>
```

## Quick test
A example script is provided with the package and can be executed with:
```bash
python3 test_environment.py
```

To build the URDF for the UR5+2F85, use:
```bash
xacro models/UR5_2F85_BuildURDF.xacro > models/UR5_2F85.urdf
```

Prerequisites:
- [Python > 3.6](https://www.python.org/downloads/)
- [PyBullet](https://github.com/bulletphysics/bullet3)
- [Numpy](https://numpy.org/install/)
- [xacro](http://wiki.ros.org/xacro) (usually installed with [ROS](https://wiki.ros.org/ROS/Installation))

And the result should look like this:

![pyb-sim-models_GIF](https://user-images.githubusercontent.com/10478385/104060368-c7a74480-51c4-11eb-961f-cb820409438c.gif)

# Basic Usage
This package provides XACRO files used to generate URDF files. As a convenience, pre-generated URDFs are also included but you will probably want to generate your own URDF after having modified the parameters in the XACROs. 

The main parameters that you might want to modify are:
```xml
<xacro:ur5_robot prefix="" joint_limited="true" ...
```
- ur5_robot prefix : Needed if you simulate more than one UR5 in the same environment
- ur5_robot joint_limited : Can be useful to help with the motion plannning.
```xml
<xacro:Robotiq_2F85 parent="ee_link" name="gripper" enable_underactuation="true" soft_pads="true" softness="5" > ...
```
- Robotiq_2F85 name : Prefix of the gripper
- Robotiq_2F85 enable_underactuation : Set to ```false``` if you want to force pads to stay parallel at all times.
- Robotiq_2F85 soft_pads : Set to ```true``` if you want to simulate the compliance of the pads (not realistic).
- Robotiq_2F85 softness : Used to modulate the amount of compliance for the pads. Used only if ```soft_pads="true"```.

Finally, a Tool Center Point (TCP) is provided in the UR5+2F85 model as a convenience. This TCP is useful if you want to move the robot such that the end-effector reaches a position in a specific orientation (inverse kinematics problem). The position and orientation of the TCP is defined relative to the base of the 2F85 gripper. The distance between the origin of the base of the gripper and the middle of the pads when the gripper is closed is 130.73mm while the distance to the tip of the pads is 149.48mm. You can modify the position of the TCP depending on how you want to plan the motions of the robot.

```xml
<joint name="tool_center_point" type="fixed">
    <origin rpy="${pi/2} ${pi/2} 0" xyz="0 -0.14948 0"/>
    <parent link="gripper_base"/>
    <child link="tcp"/>
</joint>
<link name="tcp" />
```
# Models

## Universal Robots UR-5
A model of the UR-5 is included in this package.
See [here](https://github.com/utiasSTARS/pyb-sim-models/tree/main/pbsm/models/ur5) for documentation about this model.

## Robotiq 2F-85
A realistic model of the under-actuated Robotiq 2F-85 gripper is distributed in this package.
See [here](https://github.com/utiasSTARS/pyb-sim-models/tree/main/pbsm/models/Robotiq_2F85/README.md) for more information about this model.

## UR-5 + 2F-85
The URDF tree of the whole robot can be visualized [here](https://github.com/utiasSTARS/pyb-sim-models/files/5791296/UR5_2F85.pdf).

## Dynamically Generated Test Objects Models
An modular object that can easily be built from 3D printed parts and small weights is distributed in this package. This object can be useful when the inertial parameters of the object (that cannot be directly measured) must be known for real-world experiments. Models of this object can be dynamically generated by [this script](https://github.com/utiasSTARS/pyb-sim-models/tree/main/pbsm/models/CompositeTestObject/GenerateObjectConfiguration.py) exposing a class as well as a command line interface.
An example of how this model can be used for simulated robotic manipulation is offered [here](https://github.com/utiasSTARS/pyb-sim-models/tree/main/pbsm/models/ur5_compositeobject/generate_and_simulate.py).
See [here](https://github.com/utiasSTARS/pyb-sim-models/tree/main/pbsm/models/CompositeTestObject/README.md) for more information about this model.

## Static Test Objects Models
A few other simple objects are also included in the package:
- [Small cube](https://github.com/utiasSTARS/pyb-sim-models/tree/main/pbsm/models/cube_small)
- [Large Hammer](https://github.com/utiasSTARS/pyb-sim-models/tree/main/pbsm/models/HammerHuge)
- [Small Hammer](https://github.com/utiasSTARS/pyb-sim-models/tree/main/pbsm/models/HammerSmaller)
