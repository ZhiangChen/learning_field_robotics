# Flight controller


A flight controller is the electronic component of a UAV that processes sensory information, interprets user commands, and manages motor control to stabilize and maneuver the vehicle during flight.

### Primary inputs to a flight controller:

**User Commands**:  
These are the instructions given by the pilot via a remote control transmitter. They include commands for roll, pitch, yaw, and throttle.

**Sensors Data**: Flight controllers process data from several onboard sensors:  
- Gyroscope: Measures angular velocity (rate of change of angle) for roll, pitch, and yaw.  
- Accelerometer: Measures linear acceleration in three dimensions.  
- Magnetometer (Compass): Provides information about the drone's heading relative to magnetic north.  
- Barometer: Measures atmospheric pressure to estimate altitude.  
- GPS: Provides data on the drone's position (latitude, longitude, and altitude), speed, and direction of movement.  
- Sonar/Ultrasonic sensors: Often used for altitude estimation, especially close to the ground.  
- Optical Flow Sensors: Use visual data to determine lateral movement, useful for position hold without GPS.  

**External Inputs**:  
- Telemetry: Data from a ground control station or other external systems can be inputted, allowing for commands, waypoint updates, or configuration changes during flight.
- Vision Systems: On more advanced drones, inputs from cameras or computer vision systems can be integrated for obstacle detection, tracking, or other vision-based tasks.

**Peripheral Inputs**:  
- Battery Monitors: Provide data on battery voltage and current draw.  
- Temperature Sensors: Monitor temperature for critical components or the environment.  
- Air Speed Sensors: For fixed-wing UAVs, these can provide data on the relative airspeed.

**Receiver Signals**:  
The radio receiver on the drone outputs signals based on the commands it receives from the remote control transmitter. These signals, which represent the desired state (e.g., throttle position, yaw rate), are inputs to the flight controller.

**External Navigation Aids** (less common):  
In specialized applications, a drone might receive inputs from external systems like infrared beacons or differential GPS for more precise localization.


### Primary outputs from a flight controller:

**Motor Control Signals**:  
The most immediate and crucial output from the flight controller is the commands sent to the motors (or servos in the case of fixed-wing aircraft). For multirotor drones:  
- It controls the speed of each motor to manage roll, pitch, yaw, and altitude.
- It ensures that the UAV remains stable, follows the pilot's commands, and responds to changes in conditions or sensor feedback.

**Electronic Speed Controller (ESC) Commands**:  
The flight controller sends Pulse Width Modulation (PWM) signals (or other signal types depending on the ESC and flight controller) to the ESCs, which in turn control the power and rotational speed of the motors.

**Servo Control Signals**:  
For drones with movable parts or for fixed-wing UAVs:  
- It sends commands to adjust the positions of servos, controlling elements like ailerons, rudders, or gimbals.

**Telemetry Data**:  
The flight controller sends back real-time information to the pilot or a ground station. This data may include: 
- Battery status
- GPS position
- Altitude
- Speed
- Sensor readings
- System status or warnings

**LED Signals**:  
Many drones have LEDs that indicate the system's status, mode, or potential issues. The flight controller manages these LED signals.

**Peripheral Control**:  
If the drone is equipped with additional peripherals, the flight controller may manage or communicate with them. Examples include:  
- Activating a camera shutter or starting video recording
- Adjusting camera gimbal angles
- Deploying payloads or release mechanisms
- Managing retractable landing gear

**Audio Signals**:   
Some drones have buzzers or audio indicators. The flight controller can trigger these to provide audible alerts or warnings.

**Communication Signals**:  
The flight controller might send signals to other onboard systems or modules, either to command them or share data. This includes communication with systems like advanced navigation aids, obstacle avoidance systems, or companion computers.

**Safety Protocols**:  
Outputs related to safety features, such as initiating a return-to-home sequence, triggering failsafe modes, or commanding an emergency landing.

## Flight controller hardware options

1. **Pixhawk Series**:
   - **Pixhawk 1**: One of the original and popular open-source flight controllers.
   - **Pixhawk 2 Cube**: An advanced version with modular design and improved sensors.
   - **Pixhawk 4**: The newer generation, manufactured by Holybro, is designed for advanced applications with more powerful processors and improved sensor redundancy.
   
2. **Naze32**: An older but popular board for FPV racing, often used with Cleanflight or Betaflight.

3. **CC3D**: Originally developed for the OpenPilot project, this board became popular among hobbyists and is now often used with LibrePilot or Betaflight.

4. **SP Racing F3/F4/F7**: These boards, designed by the Cleanflight team, are optimized for performance and feature a range of processors.

5. **Omnibus Series**: Known for having an integrated OSD (On-Screen Display), they are available in F3, F4, and F7 variants.

6. **KISS FC**: Developed by Flyduino, these flight controllers are part of the Keep It Super Simple (KISS) system, known for simplicity and high performance.

7. **BrainFPV RADIX**: Recognized for its graphical OSD capabilities.

8. **Matek Systems Flight Controllers**: They produce a wide variety of boards, often with integrated PDB (Power Distribution Board) and OSD.

9. **DJI Flight Controllers**:
   - **Naza-M Lite/V2**: Designed for hobbyists and custom builds.
   - **A3 and N3**: High-end flight controllers designed for professional and commercial use with advanced features and redundancies.

10. **Holybro Kakute Series**: Boards like the Kakute F7 have become popular for their integrated features and sensor performance.

11. **FlightOne (formerly RaceFlight) Boards**: Such as the Revolt and Millivolt, they are optimized for FPV racing and freestyle.

12. **iFlight SucceX Series**: Known for their stackable design and a wide variety of integrated features.

13. **CL Racing F4/F7**: Designed by Cheng Lin, these boards have become popular for FPV flying due to their design considerations and robustness.

14. **JBF4/JBF7 (Joshua Bardwell F4/F7)**: Designed in collaboration with Joshua Bardwell, a prominent figure in the FPV community, these boards focus on ease of use and reliability.

## Pixhawk
Pixhawk is one of the most popular open-source hardware platforms for unmanned vehicles, and its influence spans across both hobbyist and professional realms. 

Products on the market: https://pixhawk.org/products/

Pixhawk Cube (the Cube):  
Recognized by its distinctive cubic shape, the Cube is designed with a modular approach. The core flight controller (the actual 'Cube') is separated from the carrier board, allowing users to swap out or upgrade individual components. Often comes with built-in vibration damping mounts to minimize the effects of vibrations on the IMU (Inertial Measurement Unit). Some versions also use foam for additional isolation. 

- cubepilot: https://docs.cubepilot.org/user-guides/autopilot/the-cube-module-overview#differences-between-cube-colours  
- cuav: https://store.cuav.net/category/autopilot/ 


**Software**:  
- PX4: The native software for Pixhawk, it's a powerful, open-source software stack that provides full control over the vehicle.
- ArduPilot: A versatile, open-source software that is compatible with Pixhawk hardware. It has different versions like ArduCopter, ArduPlane, and ArduRover for various applications.