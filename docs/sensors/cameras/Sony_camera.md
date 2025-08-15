# Sony Camera

This tutorial focuses on using the Sony a6000 camera.

Sony cameras can be triggered through a multiport connection. Multiport connectors resemble microUSB connectors in shape but utilize a proprietary protocol and additional hidden pins. There are two primary methods to trigger a camera using a multiport connection:

1. **Relay**: This method involves using binary voltage to control the camera's triggering and focus.
2. **PWM (Pulse Width Modulation)**: This method employs different PWM frequencies to control various settings and modes of the camera, enabling more advanced and versatile control.

This tutorial primarily focuses on PWM triggering. For information on relay triggering, refer to the [Pixhawk Camera Trigger Setup](https://ardupilot.org/copter/docs/common-pixhawk-camera-trigger-setup.html#common-pixhawk-camera-trigger-setup). While the emphasis is on PWM triggering, this tutorial also provides foundational knowledge of camera configuration in ArduPilot. Completing this tutorial will enhance your understanding of both PWM and relay triggering methods.

To use PWM triggering, we need a Seagull REC module: https://www.seagulluav.com/product/seagull-rec/

This module has four channels. Each channel has a neutral PWM state, during which the camera remains inactive. This configuration is beneficial for default camera setups. When any channel's PWM state changes, the corresponding function is executed. However, the Seagull website and manual do not clarify the priority of channels or functions. So the behavior in cases of conflict is uncertain. It is advisable to carefully avoid potential conflicts.

## AUX ports
First, check the ports on your flight controller and identify four AUX ports to use. Next, locate their mapping in "Mission Planner -> Setup -> Mandatory Hardware -> Servo Output." Typically, the first eight servo outputs are assigned to the main motors, while the subsequent ports correspond to AUX1, AUX2, and so on. For example, when using the Holybro Pixhawk 6X, AUX1-4 can be assigned to Seagull REC channels 1-4. The corresponding servo ports for these AUX ports are servo9-12.

## Servo parameter configuration
When using servo9-12, you need to configure the following `SERVOx_` parameters:
```
SERVOx_FUNCTION
SERVOx_MAX
SERVOx_MIN
SERVOx_REVERSED
SERVOx_TRIM
```
Typically, `SERVOx_FUNCTION` should be set to either `camera trigger` or `RCINxScaled`. 

- **RCINxScaled**: This setting allows the AUX port to be controlled exclusively by the corresponding RC channel.
- **Camera Trigger**: This setting enables the AUX port to be controlled via the Mission Planner GUI. Additionally, by assigning `camera trigger` to an RC channel, you can trigger the camera using the RC. 

To assign `camera trigger` to an RC channel, navigate to "Config -> Extended Tuning -> RCx Opt" in Mission Planner. Here, set RCx Opt to `camera trigger`. Ensure that the mapping between the RC channel and the physical switches on your controller is properly configured on your RC.

`SERVOx_MAX` specifies the maximum scaled PWM value that the AUX port can output. Refer to the Seagull module manual to determine the appropriate value. Similarly, `SERVOx_MIN` defines the minimum scaled PWM value.

`SERVOx_REVERSED` determines whether the high and low PWM signals are inverted.

`SERVOx_TRIM` should be set to the neutral PWM value specified in the manual.

## CAM parameter configuration
You should configure `CAM1_` parameters. The important ones are as follows:
```
CAM1_DURATION
CAM1_INTRVAL_MIN
CAM1_SERVO_OFF
CAM1_SERVO_ON
CAM1_TYPE
```
`CAM1_DURATION` specifies the duration, in seconds, that the camera shutter remains open. This parameter requires testing and tuning. If set too short, the camera may not trigger; if set too long, the delay between triggers will increase. 

`CAM1_INTRVAL_MIN` should be set to 0.

`CAM1_SERVO_OFF` represents the neutral PWM value for triggering, while `CAM1_SERVO_ON` specifies the PWM value for the desired triggering mode.

`CAM1_TYPE` should be set to `servo` to enable PWM-based triggering. If set to `relay`, the triggering will operate in relay mode. Note that parameters like `CAM1_RELAY_ON` are not applicable when the type is set to `servo`.

Other parameters such as `CAM1_HFOV` and `CAM1_VFOV` can be specified as needed. 