---
permalink: /exercises/Drones/labyrinth_escape
title: "Labyrinth Escape"

sidebar:
  nav: "docs"

toc: true
toc_label: "TOC Labyrinth Escape"
toc_icon: "cog"

gallery:
  - url: /assets/images/exercises/labyrinth_escape/labyrinth_escape.jpg
    image_path: /assets/images/exercises/labyrinth_escape/labyrinth_escape.jpg
    alt: "Labyrinth Escape."
    title: "Labyrinth Escape."

youtubeId: JR5OH_XHw7U
---

The goal of this exercise is to implement the logic that allows a quadrotor to escape a labyrinth through visual signals placed on the ground.

{% include gallery caption="Gallery." %}

## Installation
Install the [General Infrastructure](https://jderobot.github.io/RoboticsAcademy/installation/#generic-infrastructure) of the JdeRobot Robotics Academy.

As this is a drones exercise, you will need to additionally install the `jderobot-assets`, `dronewrapper` and `rqt_drone_teleop` packages. These can be installed as

```bash
sudo apt-get install ros-melodic-drone-wrapper ros-melodic-rqt-drone-teleop ros-melodic-jderobot-assets
```

There is an additional dependency on MAVROS and PX4 that you can fulfill following the [Drones installation instructions](/RoboticsAcademy/installation/#specific-infrastructure).

## How can I run the exercise?

To launch the exercise, simply use the following command from this directory:

```bash
roslaunch labyrinth_escape.launch
```

## How should I solve the exercise?

To solve the exercise, you must edit the `my_solution.py` file and insert the control logic into it.

### Where to insert the code

Your code has to be entered in the `execute` function between the `Insert your code` here comments.

`my_solution.py`

```python
def execute(event):
  global drone
  img_frontal = drone.get_frontal_image()
  img_ventral = drone.get_ventral_image()
  # Both the above images are cv2 images
  ################# Insert your code here #################################

  set_image_filtered(img_frontal)
  set_image_threshed(img_ventral)

#########################################################################
```

## API

### Sensors and drone state

* `drone.get_position()` - Returns the actual position of the drone as a numpy array [x, y, z], in m.
* `drone.get_velocity()` - Returns the actual velocities of the drone as a numpy array [vx, vy, vz], in m/s
* `drone.get_yaw_rate()` - Returns the actual yaw rate of the drone, in rad/s.
* `drone.get_orientation()` - Returns the actual roll, pitch and yaw of the drone as a numpy array [roll, pitch, yaw], in rad. 
* `drone.get_roll()` - Returns the roll angle of the drone, in rad
* `drone.get_pitch()` - Returns the pitch angle of the drone, in rad.
* `drone.get_yaw()` - Returns the yaw angle of the drone, in rad. 
* `drone.get_landed_state()` -  Returns 1 if the drone is on the ground (landed), 2 if the drone is in the air and 4 if the drone is landing. 0 could be also returned if the drone landed state is unknown. 

### Actuators and drone control

The three following drone control functions are *non-blocking*, i.e. each time you send a new command to the aircraft it immediately discards the previous control command. 

#### 1. Position control

* `drone.set_cmd_pos(x, y, z, yaw)` - Commands the *position* (x,y,z) of the drone, in m and the *yaw angle* (in rad) taking as reference the first takeoff point (map frame)

#### 2. Velocity control

* `drone.set_cmd_vel(vx, vy, vz, yaw_rate)` - Commands the *linear velocity* of the drone in the x, y and z directions (in m/s) and the *yaw rate* (rad/s) in its body fixed frame

#### 3. Mixed control

* `drone.set_cmd_mix(vx, vy, z, yaw_rate)` - Commands the *linear velocity* of the drone in the x, y directions (in m/s), the *height* (z) related to the takeoff point and the *yaw rate* (in rad/s) 

### Drone takeoff and land

Besides using the buttons at the drone teleoperator GUI, taking off and landing can also be controlled from the following commands in your code:

* `drone.takeoff(height)` - Takeoff at the current location, to the given height (in m)
* `drone.land()` - Land at the current location. 

### Drone cameras

* `drone.get_frontal_image()` - Returns the latest image from the frontal camera as a OpenCV cv2_image
* `drone.get_ventral_image()` - Returns the latest image from the ventral camera as a OpenCV cv2_image
* `set_image_filtered(cv2_image)` - Shows a filtered image of the camera images in the GUI
* `set_image_threshed(cv2_image)` - Shows a thresholded image in the GUI

<!-- ## Theory -->

<!-- **Comming soon.** -->

## Hints

Simple hints provided to help you solve the labyrinth_escape exercise. Please note that the **full solution has not been provided.**

### Detecting the visual signals
The first task of the assignment is to detect the visual signals. This can be achieved easily by **filtering the color of the road** from the image.

Secondly, you might process the signal extracting the direction to follow. Notice that there are only four different possibilities.

### Directional control. How should drone yaw be handled? 

If you don't take care of the drone yaw angle or yaw_rate in your code (keeping them always equal to zero), you will fly in what's generally called **Heads Free Mode**. The drone will always face towards its initial orientation, and it will fly sideways or even backwards when commanded towards a target destination. Multi-rotors can easily do that, but what's not the best way of flying a drone.

Another possibility is to use **Nose Forward Mode**, where the drone follows the path similar to a fixed-wing aircraft. Then, to accomplish it, you'll have to implement by yourself some kind of directional control, to rotate the nose of your drone left or right using yaw angle, or yaw_rate. 

In this exercise, you can use the one you prefer.

### Do I need to know when the drone is in the air?

No, you can solve this exercise without taking care of the **land state** of the drone. However, it could be a great enhancement to your blocking position control function if you make it only work when the drone is actually flying, not on the ground.

## Demonstrative video of the solution

{% include youtubePlayer.html id=page.youtubeId %}


----------

## Contributors
- Contributors: [Nikhil Khedekar](https://github.com/nkhedekar), [JoseMaria Cañas](https://github.com/jmplaza), [Diego Martín](https://github.com/diegomrt) and [Pedro Arias](https://github.com/pariaspe). 
- Maintained by [Pedro Arias](https://github.com/pariaspe).