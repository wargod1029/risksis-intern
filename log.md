
***

### Task List
- annotation
- robotic arm
- BD project 
  - GNSS
  - sensor fusion between GNSS and lidar
- research on camera for tram 
- research on AI deploy on FPGA
***
### 22/07/2026
#### Research on AI deploy on FPGA (5 hour)
- found this repo using vivado [FPGA AI](https://github.com/nhma20/FPGA_AI)
- read this [eficienct_edge_AI_on_FPGA](./FPGA/Efficient_Edge-AI_Application_Deployment_for_FPGAs.pdf)
- finished infomration.md
  - standard workflow
  - AMD workflow
  - velidate the AI module comparison
  - looking at the information from AMD about:
    - Vitis AI
    - Vitis HLS
  - looking at the information about AI model quantization, compilation and runtime
  - looking at the difference between AI application and standard FPGA application on AMD FPGA



#### Research on camera for tram (3 hour)
- meeting with vadzo imaging
  - minimum amount of purchasement
  - FOV: around 120
  - price:
  - customise lens size:
  - distance coverage: ~25m
  - from vadzo imaging
    - 1.5 month testing time
    - gigabite 
    - FOV: around 12
    - fixed focus
    


### 21/07/2026


#### Robotics arm (2 hours)
- read doc
  - [Design of tendon driven mechanism using geometrica](/home/risksis/Downloads/Design_of_Tendon-Driven_Mechanism_Using_Geometrica.pdf)

#### Research on camera for tram (5 hours)
- find information
  - SDK
  - FPS
  - lens mount
  - network interface
  - stream protocol

#### Reserach on AI deploy on FPGA (1 hour)
- use AMD Vitis
  - FPGA with SoC
  - Vitis toolchain can compress int32 float model to int8 with extremly low accuracy drop
### 20/07/2026
leave


### 17/07/2026
leave

### 16/07/2026
leave

### 15/07/2026
leave

### 14/07/2026
leave

#### research on camera for tram (1 hour)
  - research on lens for camera

#### BD project (7.5 hours)
  - setup environment, packages, and dependencies on Windows and WSL
  - prepare for going out to check coordinates after eagleye


### 13/07/2026  
#### BD project (8 hour)
  - setup eagleye for sensor fusion
  - setup IMU bag in the linux machine
  - get GNSS compass
  - build nmea ros bridge
  - build rtklib ros bridge
  - setup whole environment on window laptop again (not finish)


### 10/07/2026
#### BD project (1 hour)
  - reprogram the python script of GNSS for using RTKNAVI-QT on Linux
  - research on EKF and sensor fusion
    - EKF
    - other algorithm to apply on 
    - sensor fusion
    - lidar odometry
    - pipeline on lidar odometry
    - lidar scan on ouster SDK
    - 

#### research on camera for tram (7 hours)
  - research on
    - AR0234 
      - Global Shutter 1080P GigE Camera
      - Color Global Shutter MIPI Camera
      - Global Shutter 1080P WIFI Camera
    - MV-CH100-60UC
    - Daheng Imaging 
      - ME2P-1230-23U3M
      - MARS-3140-3GM/C-P
    - Teledyne FLIR Blackfly S USB3 (BFS-U3-88S6M-C)
    - Basler ace 2 R 
      - Basic (a2A4096-30umBAS)
      - Pro (a2A4096-30umBAS)
    - lens for basler ace 2
  - prepare .md and powerpoint for proposal

### 9/07/2026
#### BD project (7.5 hours)
  - research on
    - ZED-F9P-15B GNSS receiver user manual
    - GPS
    - RTK
    - RTKLIB
    - RTKNAVI
  - connect between computer and GNSS receiver
  - ensure the antenna and GNSS receiver is working
  - sucessfully receive data from GNSS in my laptop(window) 
  - calculate the step distance and total distance of moving
  - build filter to filter out distance lower than 0.1m
#### research on camera for tram (0.5 hours)
  - Omron STC-MBS231POE
  - sport cam
    - pocket ?
    - insta ?
### 8/07/2026
#### BD project (8 hour)
  - limit test the lidar connection
  - fixed the disconnected problem by prevent the wire from bending
    - soldering new set of ethernet wire
  - read the open source code from Andy
  - transfer the ROS driver for the GNSS to ROS2 humble
  - setup the ROS driver for sensor fusion 
  - try to configure ZED-F9P-15B
  - use u-center to config (only for window machine)
  - try to get recevie something from the GNSS (fail)
    - will be fixed (added to todo)
    - probably the ZED-F9P-15B didn't config correctly
      - the RAWX can enable 
      - but the SFRBX cannot enable (keep in gray)


### 7/07/2026
#### preparation about robotic arm (1 hour)
- update on powerpoint
  -discover more choices on gearbox and frameless

#### BD project (7hour)
  -  use the new ROS2 driver to test the IMU and lidar raw data
  - try to calculate the polar coordinate by the data
  - added auto config on the script
    but after adding the auto config, the lidar will disconnected when it work on around 8 minute
      - not sure it happen because of the script or originally already have
  - test on how long until disconnnected. 
  - full drive the lidar with the IMU and lidar turn on at the same time and get data.
    - packet loss remain 0% before it disconnected
  - read doc for GNSS
    - how to use
    - how to sync
    - wrote script for GNSS


### 6/07/2026

#### Preparation about robotic arm (1 hours)
- update on powerpoint
- new frameless motor found


#### BD project (7 hours)
  - change the ROS driver to ROS 2
    - and setup ROS2 driver in the computer

  - get the IMU value from the lidar
    - x y z angular velocity and acceleration
  - wrote two script for 
    - restart driver
    - print IMU data
- solder backup wire for power supply
- solder additional 12-5V DC DC converter

### 3/07/2026


[ouster sdk](https://github.com/ouster-lidar/ouster-sdk)

### 2/07/2026
#### Preparation about robotic arm (2.5 hours)
- research on more motor to use
- discuss with mech about 
  - use titanium and CF to make the arm
  - use linear actualator in the forearm
  - use shovel to pick up the luggage instead of finger
- discuss with jovial about
  - use TOF to measure distance between arm and other object
  - use camera to scan the luggage to get the position of the luggage
  - use camera to scan the code on the luggage to get the weight of the luggage (?)
  - use TFT monitor to check the status of the robot
- research on camera to use
  - ESP32 camera
- reaserch on TFT to use

#### BD project (5.5 hours)
- setup the python viz to check the lidar data
- DC power source
  - solder 
    - wire
    - XT60 connector
    - 1 to 2 XT60 connector
- PCB
  - ethernet connection debuging
   - Br -> Bl/
   - Br/ -> G/
   - G -? Br/
   - Bl/ -> Br
   - O/ -? Bl
   - O -> G/
   - G/ -> O
   - Bl -> O/ ,,
  - wire clamping


### 30/06/2026
#### BD project (3 hours)
- lidar testing
  - installing docker for ROS 1 noetic
  - install ouster sdk in python
  - install ouster sdk in C++
  setting up ROS1 noetic
#### Preparation about robtoic arm (5 hours)
- research on motor to use
  - BLDC
  - frameless BLDC
  - BLDC ESC
  - AC motor with driver
- discuss with jovial about motor and power electronic
- discuss with mech about overall design and material choice
- prepare the powerpoint for proposal with jovial


### 29/06/2026
#### BD project (7 hour)
- lidar testing
  - setting up ROS
- bluepill coding 
  - sync 1 Hz PPS to generate 10Hz
- solder and wire from lidar to PCB

#### AnyLabekling (1 hour)


### 26/06/2026
#### preparation about robotic arm (4 hour)   
- [reference manual](./robotic_arm/sigma7_communication_references_project.pdf)
- ![](./robotic_arm/image/EtherCAT_RUN_indicator.png)
- ![](./robotic_arm/image/EtherCAT_ERR_indicator.png)
- ![](./robotic_arm/image/EtherCAT_LINK_indicator.png)
- ![servo pack choices](./robotic_arm/image/servopack_choice.png)
- 4.3: wiring the power supply to the SERVOPACK
  - **required external regenerative resistor between B1/+ and B2**
  - power supply: 200VAC to 240 VAC 50Hz/ 60 Hz
  - ![wiring](./robotic_arm/image/servo_wireconnection.png)
  - use three phase for continuous, steady flow of energy
  - ![wiring diagram 1](./robotic_arm/image/wiring_diagram1.png)
  - ![wiring diagram 2](./robotic_arm/image/wiring_diagram2.png)
  - ![wiring diagram more than one servo](./robotic_arm/image/wiring_diagram3.png)

  - 4.3.5 wiring regenerative resistors
  - 4.3.6 wiring reactors for harmonic suppression
  - 4.4.3 wiring absolute encoder
  - 4.4.4 wiring holding brake
    - ![wiring diagram for holding brake](./robotic_arm/image/wiring_holding_brake.png)
  - use Photocoupler Circuit for isolate signal from high frequency power
  - use line driver for projection 
  - safety input circuit 
    - ![wiring diagram for safety input circuit](./robotic_arm/image/safety_input_circuit.png)

#### annotation on corrosion
- image count: 6010/6458 (12:00)
- image count: 6458/6458 (17:30)

#### Any labeling
half hour
### 25/06/2026
#### annotation on corrosion
- image count: 4520/6458 (11:00)
- image count: 5000/6458 (15:00)
- image count: 5579/6458 (17:00)
#### preparation about robotic arm (3 hour)
- Control of AC motor
- AC motor driver from yaskawa
- introduction to ROS (robotic operating systme)
  - [ROS](./robotic_arm/ROS.pdf)

### 24/06/2026
#### preparation about robotic arm (5hour)
- choice of motor
  - AC motor
  - AC servo motor
  - BLDC motor
- power input
  - 48V DC (will cause too much current when driving motor 60-100A)
  - 220VAC 
    - easier to obtain from wall socket
    - fewer motor choice
    - have to choose some weaker motor
  - 400VAC
    - difficlt to obtain
    - better motor choice
    - can go faster with lower current


- 200VAC S7G gear motors 500W-7.5kW
  - https://www.yaskawa.com/products/motion/sigma-7-servo-products/gear-motors/s7g-gear-motors

servo control
- servo pack
  - Model Code Subtype: SGD7S-xxxxxxx00
    - control by 5V/24V square-wave pulse, each pulse equal to 0.001 degrees of rotation, faster the pulse, faster the spin
  - Model Code Subtype: SGD7S-xxxxxxxA0
    - control via Ethernet 

Force and torque
- waist motor: 330Nm
- elbow motor: 266Nm
  - Gravity Torque: 193.7 Nm.
  - Segment Mass Resistance: 36.8 Nm.
  - Payload Weight Resistance: 156.9 Nm.
  - Moment of Inertia: 9.25 kg·m².
  - Dynamic Acceleration Torque: 72.6 Nm.
  - Total Joint 2 Torque: 266.3 Nm.
- base motor: 791Nm
  - Gravity Torque: 461.1 Nm.
  - Segment 1 Weight Resistance: 36.8 Nm.
  - Segment 2 Weight Resistance: 110.4 Nm.
  - Payload Weight Resistance: 313.9 Nm.
  - Total System Inertia: 42.0 kg·m².
  - Dynamic Acceleration Torque: 329.7 Nm.
  - Total Joint 1 Torque: 790.8 Nm.

- safety factor 1.5-2
 
**want to know about the dimension limit 
the power/voltage limit 
of the robotic arm
and the budget allowed
how long the arm be
how far the target be
do it really need to throw or just transportation**

#### annotation on corrosion
- image count: 2938/6458 (14:00)
- image count: 3500/6458 (16:00)
- image count: 4020/6458 (17:50)

### 23/06/2026


#### preparation about robotic arm
- want to lift and throw ~32kg luggage
- 6 Dof (degree of freedom)
- Base Servo (Left/Right)
- Shoulder Servo (Up/Down)
- Elbow Servo (Up/Down)
- Wrist Pitch (Up/Down) — Added to original
- Wrist Yaw (Left/Right) — Added to original
- Wrist Roll (Twist) — Added to original
- Gripper Motor (Open/Close)

motor driver for BLDC: https://shop.odriverobotics.com/products/odrive-pro
- ODRIVE PRO
- dual absolute encoder
- 3000W Continous power
- 1 motor per driver
- 14-58V
- 70A continuous (100A peak with heatsink and fan)
- Isolated CAN, UART, Step/Dir and PWM control inputs

#### annotation on corrosion
- image count: 1221/6458 (16:00)
- image count: 1600/6458 (17:00)
- image count: 2010/6458 (17:45)



### 22/06/2026
#### annotation on abandon
- image count: 10000/13151 (10:30)
- image count: 11038/13151 (13:00)
- image count: 12053/13151 (15:00)
- image count: 13151/13151 (17:41)
#### revise the image missed previously 



### 18/06/2026
#### annotation on abandon
- image count: 6545/13151 (15:00)
- image count: 7700/13151 (16:30)
- image count: 8200/13151 (17:00)
- image count: 9056/13151 (17:55)

### 17/06/2026
#### annotation on abandon
- image count: 1521/13151 (14:30)
- image count: 3806/13151 (17:17)
- image count: 4127/13151 (17:50)
### 16/06/2026
sick leave

### 15/06/2026
- Completed first PCB schematic design in KiCad  
- Completed first PCB layout in KiCad  
- Reviewed documentation for new LiDAR (Robosense M1 Plus)  
- Discussed integration strategy for new LiDAR  
- Coordinated with mechanical team on mounting holes and PCB dimensions  

**STM32F103C8T6 (Blue Pill) Development**
- Implemented synchronization system: 8 Hz signal aligned to 1 Hz reference  
- Configured PC13 as 1 Hz synchronization output  
- Configured PA8 for input capture  
- Configured PA9 as 8 Hz signal output  

***

### 12/06/2026
- Reviewed documentation for Robosense M1 Plus LiDAR  
- Drafted initial PCB schematic in KiCad  

***

### 11/06/2026
- Sick leave  

***

### 10/06/2026
- Sick leave  

***

### 09/06/2026
- Reviewed system component documentation:
  - LiDAR  
  - Camera  
  - GNSS  
  - MCU  
- Set up development environment:
  - VS Code  
  - STM32CubeIDE  
  - STM32CubeMX  
  - KiCad 9.0  

***
