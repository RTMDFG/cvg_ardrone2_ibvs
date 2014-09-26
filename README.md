## Safety Warning and Disclaimer

You are using this software at your own risk. The authors decline any responsibility for personal injuries and/or property damage.

The AR Drone 2.0, supported by this framework, is a TOY. However, its operation might cause SERIOUS INJURIES to people around. So, please consider flying in a properly screened or isolated flight area.

## cvg_ardrone2_ibvs

We present a vision based control strategy for tracking and following objects using an Unmanned Aerial Vehicle. We have developed an image based visual servoing method that uses only a forward looking camera for tracking and following objects from a multi-rotor UAV, without any dependence on GPS systems. Our proposed method tracks a user specified object continuously while maintaining a fixed distance from the object and also simultaneously keeping it in the center of the image plane. The algorithm is validated using a Parrot AR Drone 2.0 in outdoor conditions while tracking and following people, occlusions and also fast moving objects; showing the robustness of the proposed systems against perturbations and illumination changes. Our experiments show that the system is able to track a great variety of objects present in suburban areas, among others: people, windows, AC machines, cars and plants. 

This project is operative and based on [ROS](http://ros.org/ "Robot Operating System"). It was publicly demonstrated in the 11th anniversary of the IEEE International Symposium on Safety, Security, and Rescue Robotics, [SSRR2013](http://www.ssrr-conference.org/2013/Demos.html "Demos website of 11th anniversary of the IEEE International Symposium on Safety, Security, and Rescue Robotics") (Linköping, Sweden).

An explanation of the stack software and connectivity between modules are specified in the following documents and sites:

- J. Pestana, J. L. Sanchez-Lopez, S. Saripalli, P. Campoy. “Computer Vision Based General Object Following for GPS-denied Multirotor Unmanned Vehicles”. In the 2014 American Control Conference (ACC 2014). Portland, Oregon (United States). June 4th - 6th 2014.
- J. Pestana, J. L. Sanchez-Lopez, S. Saripalli, P. Campoy. “Vision based GPS-denied Object tracking and Following for Unmanned Aerial Vehicles”. In the 11th IEEE International Symposium on Safety, Security, and Rescue Robotics (SSRR 2013). Linköping (Sweden). October, 21-26, 2013. [Link](http://ieeexplore.ieee.org/xpl/articleDetails.jsp?tp=&arnumber=6719359&queryText%3DVision+based+GPS-denied+Object+tracking+and+Following+for+Unmanned+Aerial+Vehicles "IBVS_SSRR2013 CVG group paper").
- [http://robotics.asu.edu/ardrone2_ibvs/](http://robotics.asu.edu/ardrone2_ibvs/ "http://robotics.asu.edu")
- [http://www.vision4uav.eu/?q=following](http://www.vision4uav.eu/?q=following "http://www.vision4uav.eu")

## Table of Contents

- [Safety Warning and Disclaimer](#Safety-Warning-and-Disclaimer)
- [cvg_ardrone2_ibvs](#cvg_ardrone2_ibvs)
- [Table of Contents](#Table-of- Contents)
- [Installation](#installation)
	- [Pre-requirements](#Pre-requirements)
	- [Installation Steps](#Installation-Steps)
- [Coordinate Frames](#Coordinate-Frames)
	- [Multirotor coordinate frame](#Multirotor-coordinate-frame)
- [How to Run](#how-to-run)
	- [Launch scripts](#Launch-scripts)
	- [Interaction with UI](#Interaction-with-UI)
- [License](#license)
- [Contributors](#Contributors)
- [Contact Information](#Contact-Information)

## Installation
### Pre-requirements 

This driver has been tested on Linux machines running Ubuntu 12.10 (64 bit). However it should also work on any other mainstream Linux distributions. The driver has been tested on ROS "groovy". The code requires a compiler that is compatible with the C++11 standard. Additional required libraries are: boost and ncurses. To see dependencies upon other ROS package depends on these ROS packages: `ardrone_autonomy`, `opencv2`, `roscpp`, `image_transport`, `sensor_msgs` and `std_srvs`.

### Installation Steps

The installation follows the same steps needed usually to compile a self-contained ROS stack.

* Install ncurses and the boost libraries in your system.

        ```bash
        $ sudo apt-get install libncurses5
        $ sudo apt-get install ncurses-bin
        $ sudo apt-get install ncurses-dev
        $ sudo apt-get install libboost1.49-dev
        ```

* Create a ROS_WORKSPACE to install the stack and the required external ROS packages and stacks. For example, A ROS_WORKSPACE can be configured in the folder. `~/workspace/ros/cvg_ardrone2_ibvs`. The following steps are advised:

        ```bash
        $ # create the ~/workspace/ros/cvg_ardrone2_ibvs folder
        $ cd ~
        $ mkdir workspace
        $ cd workspace
        $ mkdir ros
        $ cd ros
        $ mkdir cvg_ardrone2_ibvs
        $ cd cvg_ardrone2_ibvs
        $ # initialize ROS workspace using ROS
	$ #if you use ROS groovy
        $ rosws init ./ /opt/ros/groovy
	$ #if you use ROS hydro
	$ rosws init ./ /opt/ros/hydro
	$ #if you use ROS indigo
	$ rosws init ./ /opt/ros/indigo
        ```

* Download the required ROS packages using git or rosws:

        ```bash
        $ # create folder where external ROS stacks are downloaded
        $ rosws set ./extStack
        $ # set folder to download the ardrone_autonomy ROS package
        $ rosws set ./extStack/ardrone_autonomy --git https://github.com/AutonomyLab/ardrone_autonomy.git --version=fuerte-devel
        $ # set folder to download the ROS_OpenTLD ROS package
        $ rosws set ./extStack/ros_opentld --git https://github.com/Ronan0912/ros_opentld.git
        $ # set folder to download the cvg_ardrone2_ibvs ROS package
        $ rosws set ./stack --git https://github.com/Vision4UAV/cvg_ardrone2_ibvs.git
        $ # download both packages using git, through the rosws command
        $ rosws update
        ```

* Set up the `IBVS_STACK` and `IBVS_WORKSPACE` environment variables. 

        ```bash
        $ # note: ${IBVS_WORKSPACE}='~/workspace/ros/'
	$ ./stack/installation/installWS.sh
        $ # note: ${IBVS_STACK}='~/workspace/ros/'
	$ cd stack
	$ ./installation/installStack.sh
	$ #You have to close the terminal an re-open it, before you continue
        ```

* Each time the cvg_ardrone2_ibvs is going to be used, do the following (note that the ROS_WORKSPACE and other ROS environment variables should not be loaded in the .bashrc file or other ubuntu terminal startup files):

        ```bash
        $ cd ${IBVS_STACK}
        $ source setup.sh
        ```

* Final steps installation instructions:

        ```bash
        $ cd ${IBVS_STACK}
        $ source setup.sh
        $ rospack profile
        $ rosdep update
        $ # Compile external ROS packages: ardrone_autonomy
        $ cd ${IBVS_WORKSPACE}/extStack/ardrone_autonomy/
        $ # The previous command, should be equivalent to: $ roscd ardrone_autonomy
	$ # Compile internal libraries of ardrone_autonomy
        $ ./build_sdk.sh
        ```

* Compile the stack:

        ```bash
        $ cd ${IBVS_STACK}/launch_dir/
        $ ./rosmake_IBVS_Release.sh
        ```
* If you run into this compilation problem

        ```
        CMake Error at /usr/share/cmake-2.8/Modules/FindOpenCV.cmake:239 (MESSAGE):
          OpenCV required but some headers or libs not found.  Please specify it's
          location with OpenCV_ROOT_DIR env.  variable.
        Call Stack (most recent call first):
          CMakeLists.txt:6 (find_package)
        ```

Then substitute "find_package(OpenCV)" for:

        ```
        ros_opentld/opentld/build/git-opentld/CMakeLists.txt
        ros_opentld/tld_tracker/CMakeLists.txt
        ```

In these files:

        ```
        IF(NOT OpenCV_FOUND)
            set(OpenCV_DIR /opt/ros/groovy/share/opencv2/)
            find_package(OpenCV)
        ENDIF(NOT OpenCV_FOUND)
        ```

It is necessary to calibrate the AR Drone 2 camera, either as explained in the {IBVS_STACK}/ext_resources folder, or to copy the sample calibrations files located in {IBVS_STACK}/ext_resources/ardrone2_cameracalibration/camera_info/ardrone_bottom.yaml to the folder ~/.ros/camera_info .


## Coordinate Frames

### Multirotor coordinate frame

In the documentation located in `${IBVS_STACK}/documentation/Coordinate_Frames/Coordinate_Frames_documentation.tex/pdf`, this reference frame is called F_{drone_LMrT}.

The reference frame that is used to reference the multirotor's telemetry, broadcasted by the multirotor's ROS driver, is:

	[Fm] the mobile reference frame is centered on the drone, with:
	 [xm] horizontal and pointing forward (same direction as the optical axis of the frontal camera
	 [ym] horizontal and pointing rightwards from the center of the drone
	 [zm] vertical and pointing downwards
	
	[F] the fixed reference frame:
	 [x] horizontal and pointing "North" (where the magnetometer reading finds the North)
	 [y] horizontal and pointing "East"
	 [z] vertical and pointing downwards (the z coordinate of the drone is, thus, always negative)
	
	[yaw (mobile to fixed)] increases when the drones rotate in clock-wise direction

The sign convention for the commands, received by the multirotor's ROS driver, is the following:

	[paraphrased] The yaw, pitch, roll angles commands are related to the above mentioned [Fm] reference frame (thus setting their sign convention). The dz/"gaz" command is such that the AR Drone is commanded to go higher (increased altitude) when dz/"gaz" is positive. It can be thought of as the dz/"gaz" command setting a higher thrust on the propellers. 

	[comment] the following phrases are understood from the point of view of a person sitting on the drone looking forward (looking in the same direction as the frontal camera), under no external wind conditions.
	[pitch][+] move backwards
	[pitch][-] move forward
	[roll][+] move rightwards
	[roll][-] move leftwards
	[dyaw][+][speed command] rotate clockwise (as seen from above), or N > E > S > W > N > ...
	[dyaw][-][speed command] rotate counter-clockwise (as seen from above), or N > W > S > E > N > ...
	[dz][+][speed command] increase altitude, move upwards
	[dz][-][speed command] decrease altitude, move downwards

## How to Run

### Launch scripts

In order to run the stack, it was decided to run each node in a separate tab of a terminal window. The initialization of the architecture is done by executing shell scripts that open a new terminal with each node running in its tab. The script that is available is the following (please take a look at them to understand how do they work):

- `${IBVS_STACK}/launch_dir/parrot_IBVSController_launcher_Release.sh`

NOTE: all the launchfiles open a separate terminal with multiple tabs, where each tab usually runs only one tab. If you close the terminal tabs using the close button at the corner of the window which has multiple tabs, then only one of the tabs will be closed correctly (the one that is currently selected):

- The easiest way to do this fast, and cleanly is to: first, press `control+c` on every tab (navigating with `control+repag` and `control+avpag`), second, use the shortcut `ctrl+shift+w` to close first all terminal tabs and, third, `ctrl+shift+q` to close the las terminal tab (which closes the window too) including all tabs. 

- The following script might be used to send a SIG\_TERM to all the terminals (equivalent to pressing `control+c` in them): ${IBVS_STACK}/launch_dir/stop.sh .

The launch scripts have to be called using the following sintax in the shell terminal: 

        ```bash
        $ cd ${IBVS_STACK}/launch_dir
        $ ./parrot_IBVSController_launcher_Release.sh
        ```

### Interaction with UI

Simplified instructions to work with the controller:

1. Window "OpenTLD GUI":
  1. It is important to familiarize with this window. It allows you to start/stop the tracker, toggle learning, change the tracker's target
  2. To start the tracker:
    1. click F5 to update the image shown on the OpenTLD GUI, you have to do this sometimes because no image is shown on ir, or because you want to track something that is not on the currently shown image
    2. Select the target drawing a bounding box around it in the image
    3. press "Enter"
  3. To change the tracker's target:
    1. click "reset" once
    2. redo the "start the tracker" instructions
  4. To toggle learning:
    1. You have to be tracking an object. Then click the toggle tracking button.
    2. This will change from "learning and tracking mode" to "tracking only mode".
    3. To know exactly whether the tracker is currently learning or not; then go to the terminal tab which is running the OpenTLD nodes. This information is printed there.
2. To interact with the rest of the software the "IBVSCntInterf" terminal tab is used:
    1. To check the keybindings the best is to read the code on the source file: ${repository}/parrotBrainInterface/src/sources/IBVS_interface.cpp . There is a switch case instruction which acts upon pressed keys.
    2. The important keys are:

```
      't': take off
      'y': land
      'h': enter hovering mode
      'm': enter move mode
```

    3. In move mode:

```
      arrow_left: move/tilt left
      arrow_right: move/tilt right
      arrow_up: move/tilt forwards
      arrow_down: move/tilt backwards
      'q': move upwards
      'a': move downwards
      'z': yaw left/counterclockwise
      'x': yaw right/clockwise
      's': stop / send "zero" commands in every command. In ROS ardrone_autonomy this means entering the hovering mode	
```

    4. Start the controller:

```
      'o': start the controller
      'i': stop the controller
      'u': reset controller (unimportant command with the IBVS controller)
```

    5. Start the kalman filter (only used for data logging):

```
      'l': start the kalman filter
      'k': stop the kalman filter
      'j': reset kalman filter, reinitialize estiamte to (0,0,0)
```

    6. Note that the commands explained in the move mode act differently when the controller is started. They change the controller references in a logical way (you can check it looking at the HUD window without flying and starting the controller). There are some contradictions yet in this options (like move forward would actually make the drone move further away from the target) and bugs (some events trigger a change of controller references back to default values).
    7. The logs are saved in ${repository}/launch_dir/logs/date/time/${log_files}

I recommend experimenting with the AR Drone and the tracker separately before running the controller. Usually you can recover control of the drone at any moment by stopping the controller ('i') and entering hovering mode ('h').

## License

All distributed software, except the packages listed in the following, are subject to `3-clause BSD` license (see the file: `LICENSE`). 

This software stack uses other third-party open-source libraries (some of them are inluded in the soure of the stack as separate packages):

- Robot Operating System ([ROS](http://ros.org/ "Robot Operating System")), license: `BSD` license, not included in the source of the stack.

- ardrone_autonomy ROS package ([ardrone_autonomy](https://github.com/AutonomyLab/ardrone_autonomy "ardrone_autonomy ROS package")), license: `BSD` license, not included in the source of the stack.

- [OpenTLD](https://github.com/gnebehay/OpenTLD/ "OpenTLD") is published under the terms of the GNU General Public License.

- [ROS_OpenTLD](https://github.com/Ronan0912/ros_opentld "ROS_OpenTLD") is published under the terms of the GNU General Public License.

- Open Source Computer Vision library ([OpenCV](http://opencv.org/ "OpenCV")), license: `BSD` license, not included in the source of the stack.

- pugixml library ([pugixml](http://pugixml.org/ "pugixml")), original license: `MIT` license, included as source in the stack in the `lib_pugixml` package.

- newmat11 library, a matrix library in C++ ([newmat11](http://www.robertnz.net/nm11.htm "newmat11, a matrix library in C++")), original license: ([newmat11 license](http://www.robertnz.net/nm11.htm#use "newmat11 library license")), included as source in the stack in the `lib_newmat` package.

- other software libraries used by this stack also uses licenses similar to `BSD` and `MIT` licenses: [ncurses](https://www.gnu.org/software/ncurses/ "GNU ncurses") and [boost](http://www.boost.org "boost c++ libraries").


## Contributors

- [Msc. Jesus Pestana Puerta](http://www.linkedin.com/in/jespestana/ "Msc. Jesus Pestana Puerta"): PhD. Candidate at [CVG](http://www.vision4uav.com/ "Computer Vision Group, CVG, CAR, UPM-CSIC"), CAR, CSIC-UPM.

- [Msc. Jose Luis Sanchez-Lopez](http://www.vision4uav.com/?q=jlsanchez/personal "Msc. Jose Luis Sanchez-Lopez"): PhD. Candidate at [CVG](http://www.vision4uav.com/ "Computer Vision Group, CVG, CAR, UPM-CSIC"), CAR, CSIC-UPM.

- [Professor Dr. Srikanth Saripalli](http://sese.asu.edu/person/srikanth-saripalli "Professor Dr. Srikanth Saripalli"): head of the [ASTRIL Laboratory](http://robotics.asu.edu/ "ASTRIL // Autonomous System Technologies Research & Integration Laboratory"), SESE, ASU.

- [Professor Dr. Pascual Campoy](http://www.vision4uav.com/?q=pascual "Professor Dr. Pascual Campoy"): head of the [CVG](http://www.vision4uav.com/ "Computer Vision Group, CVG, CAR, UPM-CSIC"), CAR, CSIC-UPM.

## Other collaborators

- [Patrick McGarey](https://www.linkedin.com/pub/patrick-mcgarey/2b/a/189) and [Msc. Mariusz Wzorek](http://www.linkedin.com/pub/mariusz-wzorek/7/a17/906): help and advice during experimental testing.

- [Msc. Ignacio Mellado](https://www.linkedin.com/in/ignaciomellado) and [PhD Iván F. Mondragón B.](http://www.javeriana.edu.co/blogs/imondragon/): extensive shared experience on vision based real-time robotics and multirotors in particular.

## Contact Information

Current CVG (CAR, UPM-CSIC) staff that can give limited support to the usage of this stack are:

- Jesus Pestana Puerta: jesus.pestana@upm.es

- Jose Luis Sanchez-Lopez: jl.sanchez@upm.es

NOTE: we know that this documentation is partial. If the stack gets the interest of the community we will continue adding information depending on the questions made by its users.

