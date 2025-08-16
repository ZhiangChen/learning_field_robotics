# Key Flight Modes in Ardupilot

```
Stabilize
AltHold
Auto
Brake
Guided
Loiter
RTL
SmartRTL
Land
```

- **Stabilize**: This mode provides manual attitude control. You may need to [tune the trim](https://ardupilot.org/copter/docs/autotrim.html) when using this flight mode, e.g., when the UAV is tilting or drifting. 

- **AltHold**: This mode automatically controls altitude using the accelerometer and compass. GPS is not required for this mode. It is significantly more stable than `Stabilize`. However, the UAV may still drift because there is no feedback control for position or yaw. Properly tuning the trim and PID can greatly reduce drifting.

- **Loiter**: This mode uses position control with external positioning systems such as GPS. Even if the trim and PID are not perfectly tuned (though they still need to function), `Loiter` can hold a position very stably. However, if the PID is not well-tuned, controlling the UAV's movement may be less stable.

- **Guided**: This mode uses position control with external positioning systems. Instead of relying on RC, pilots can assign waypoints by selecting "right click -> Fly to Here" in Mission Planner. Before using "Fly to Here," the UAV must be armed and take off. Additionally, the "Fly to Here Alt" option allows specifying the altitude of the assigned waypoint. This mode is particularly useful for testing ideas in simulation or recording a flight path.

- **RTL (Return to Launch)**: This mode commands the UAV to return to its launch point. The UAV will ascend to a predefined altitude (`RTL_ALT`, if it is below the RTL altitude) before traveling back to the launch location. Once it reaches the launch point, it will either hover or land, depending on the configuration. This mode is useful for safely recovering the UAV in case of signal loss or when the mission is complete.If the UAV is above the predefined RTL altitude, it will maintain its current altitude while traveling back to the launch location.

- **SmartRTL**: This mode is similar to `RTL` but with an added feature. Instead of following a direct path back to the launch point, the UAV retraces its flight path in reverse. This is particularly useful in environments with obstacles, as it ensures the UAV avoids hazards encountered during its initial flight. SmartRTL requires sufficient memory to store the flight path and may not be available on all hardware.

- **Land**: This mode commands the UAV to descend and land at its current location. The descent is controlled to ensure a smooth and safe landing. If the UAV is equipped with a rangefinder, it will use the rangefinder data to improve landing accuracy. This mode is particularly useful for ending a mission or in emergency situations where the UAV needs to land immediately. Once landed, the motors will disarm automatically after a short delay to ensure safety.

- **Auto**: This mode allows the UAV to execute a pre-programmed mission autonomously. The mission is defined by a series of waypoints and commands uploaded to the flight controller using a ground control station, such as Mission Planner. 

    In `Auto` mode, if the first command is not a takeoff, you must manually arm and take off the UAV before starting the mission. Because `Auto` mode does not allow arm, it is common to begin in `Loiter` mode to arm and take off, then use the "Start Mission" action. This will automatically switch the flight mode to `Auto`. If the first command is a takeoff, you only need to arm the UAV in a different mode and then initiate the "Start Mission" action.

- **Brake**: This mode is designed to bring the UAV to an immediate stop and hold its position. It uses aggressive braking maneuvers to quickly halt the UAV's movement, making it ideal for emergency situations or when precise control is required. After braking, you can manually operate the UAV, switch to `Guided` mode for further control, or use modes like `RTL` or `Land`. Additionally, you can change the flight mode back to `Auto`, allowing the UAV to proceed directly to the next unfinished waypoint and continue the mission. Note that any commands between the current position and the next waypoint will be discarded.


In `Auto` mode, you can use the `Resume Mission` feature to continue the mission from a specific waypoint. The UAV will fly directly to the selected waypoint and resume the mission from there. Note that this functionality is only available while the UAV is in `Auto` mode. Be cautious when using this functionality, as all waypoints before the specified waypoint will be removed. It is recommended to save the mission to a file beforehand to avoid losing important data.
