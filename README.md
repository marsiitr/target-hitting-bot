# target-hitting-bot
# Target Hitting Robot Using Dynamic Initial Positions

## About It
The robot travels along a single axis—forward or backward—under user control, smoothly transitioning until it reaches the exact stopping point. Once in place, the operator triggers a search command, activating the ultrasonic sensor on the launchpad to sweep the horizontal for the target. When the sensor locks onto the target, it signals the servo motor to fix the launchpad in the precise orientation needed for an accurate shot. Immediately after, two high-speed wheels accelerate to a predetermined RPM, gibing the ball enough energy so it follows a stable trajectory toward the target (almost a straight line, projectile motion was ignored). Upon launch, the launching wheels turn off, the servo releases its hold, and the launchpad slides back along the same path to its original home position. This fully automated sequence—from precise positioning and target detection to projectile launch and system reset—ensures reliable, repeatable performance without any manual recalibration between cycles.



## Main Components Used

- 4 ***SGM37-520*** -- (DC motors for moving the robot).
- 1 ***HC-SR04*** -- (The ultrasonic sensor responsible for locating the target).
- 1 ***Rhino MDD20*** -- (The motor driver responsible for making the car go forward and backwards).
- 1 ***IRFZ44NB*** -- (A MOSFET used as a signal-based switch).
- 1 ***MG995*** -- (The servo motor responsible for the movement of the launchpad).
- 2 ***CAD Designed Structures*** -- (The launchpad and the base of the robot).

## User Instructions 
If the ESP-32 already has the code in it, then the user must do as follows.
1. Connect the ESP-32 with the **bluetooth terminal** so as to control the robot remotely. 
2. The robot will move as per these commands mentioned below;
   - `B --> Move the robot backwards`
   - `F --> Move the robot forwards`
   - `S --> Stop the robot`
   - `Lxxx --> Set the left motor speed as xxx (0-255)`
   - `Rxxx --> Set the right motor speed as xxx (0-255)`
   - `SV --> Enable Servo Sweeping (cause the launchpad to go up and down)`
   - `SP --> Stop the Servo Sweeping (if the user wants to do it manually and not by the ultrasonic sensor)`
3. The user will make these commands as per their desire on the bluetooth terminal.
#### Here, the main possibilites of tweaking the code could be to;

- Integrating a few command sequences into one, easing the use of it (combining ***SV*** and ***L*** into one).
- To change the [names](#commands-used) of the commands used (Change the name of ***S*** to ***Stop*** and so on).
- To adjust the distance detection for the ultrasonic sensor (from ***50cm*** to ***40cm*** or ***60cm***) as per the assumed distance of the target.
- To adjust the ***PWM*** of the motors (if both are working perfectly then the PWM should be the same).

## Bugs

Beinng at this project for a nearly a month, bugs and issues were bound to come. Some of the noteworthy are;

#### Challenges Faced

- **Integrating the MOSFET with an ESP-32** : As an ESP-32 only supplies 3.3 Volts from its terminal, a standard MOSFET will not work (it needs around 5 volts for its gate to fully open). So whilst the MOSFET would work excellently with an Arduino, it will only give an illusion of being functional with an ESP. Changing from a standar MOSFET to a logic level one is cruical.

- **Projectile Dynamics** : Ping Pong Balls have a smooth shape and hollow structure. Launching it at even moderate distances induces significant impact from the air drag and would cause us to miss our target. Fixing that required either factoring in the complex dynamics of the ping pong ball, or use another projectile. We went with the latter.

- **Launchpad Weight** : Designing an optimized launch platform demands multidisciplinary analysis to minimize footprint and maximize usable volume. You must choose servos with sufficient torque margins, strategically position launch wheels for efficient force transfer, and ensure precise weight distribution to maintain balance under dynamic loads. By holistically evaluating spatial constraints, component placement, and mechanical requirements, you’ll achieve a compact, high-performance launch system.
## The plan ahead 

1. **Introduction to 2 Dimensional movement :** Moving in 2-Dimensions allows our robot to hit the target from any place on land. Implementing a four-wheel Mecanum drive grants full omnidirectional mobility—effortlessly strafing laterally or diagonally on flat surfaces. Adapting the control code is straightforward: swap differential-drive logic for standard Mecanum mixing equations. This 2D maneuverability significantly enhances platform robustness and broadens applications in constrained or dynamic environments.

2. **Target Identification :** Changing to a Raspberry Pi platform will do the. A Raspberry Pi 4 or 5 paired with the official Pi Camera can execute TensorFlow Lite object-detection models, such as MobileNet-SSD, at approximately 5–10 frames per second. By integrating this setup with Python and OpenCV, you can detect target cups, extract their pixel coordinates, and convert them into precise robotic motion commands.

3. **Target Movement :**  If our target gets moving Using a high-FPS, global-shutter camera and optical-flow or detection+tracking (e.g., SORT) we get per-frame target positions. Fuse these noisy measurements in a 2D Kalman filter to estimate velocity and predict where the target will be after your processing/control delay. Finally, drive your actuator with a PID loop plus a feedforward “lead” term so you fire at the predicted intercept point.

