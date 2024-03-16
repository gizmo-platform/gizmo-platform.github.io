# Drive a Robot

So far with the Arduino environment we've learned about the basics of
the framework, how to use libraries, and about the `<Gizmo.h>`
library.  Now we can put it all together to use the Gizmo board to
drive a 2 wheeled robot.  This robot will use tank/skid steering, and
will receive its control signals from a [practice
field](/field/practice.md).

## Writing the Code

To drive the robot, we'll make use of a sample program that comes with
the ArduinoGizmo library.  In the Arduino IDE menu bar, select the
BasicRobot example code by opening the following menus:

> File > Examples > Gizmo > BasicRobot

You may need to scroll down in the Examples menu to get to the Gizmo
option, it will be near the bottom.  With the comments removed, the
program that will drive the robot is as shown:


```C
#include <Servo.h>
#include "Gizmo.h"

Servo DriveL, DriveR;
Gizmo gizmo;

void setup() {
  gizmo.begin();

  pinMode(GIZMO_MOTOR_1, OUTPUT);
  pinMode(GIZMO_MOTOR_2, OUTPUT);

  DriveL.attach(GIZMO_MOTOR_1);
  DriveR.attach(GIZMO_MOTOR_2);

  pinMode(LED_BUILTIN, OUTPUT);
}

void loop() {
  digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));

  gizmo.refresh();

  int targetL, targetR;
  targetL = map(gizmo.getAxis(GIZMO_AXIS_LY), 0, 255, 0, 180);
  targetR = map(gizmo.getAxis(GIZMO_AXIS_RY), 0, 255, 0, 180);

  DriveL.write(targetL);
  DriveR.write(targetR);

  delay(20);
}
```

This program is only 31 lines of code and it can drive a robot.  Now,
this robot doesn't do much, but it is a great starting point to extend
further.  As written, this code expects a left and right drive motor
to be attached to the first 2 motor ports.  It also toggles the
built-in LED to show that the program is running and hasn't frozen.

For more detail about what each component does, the example code in
the Arduino IDE is heavily commented with descriptions for each
component.

The one portion of this code that hasn't been shown yet in any of our
previous programs is the `map()` function.  These two lines do a
really important task:

```C
targetL = map(gizmo.getAxis(GIZMO_AXIS_LY), 0, 255, 0, 180);
targetR = map(gizmo.getAxis(GIZMO_AXIS_RY), 0, 255, 0, 180);
```

The joystick axis values range from 0 to 255, but the motor control
range is from 0 to 180.  The mapping between these two values is just
a linear proportion, but the `map()` function saves the effort of
writing out this proportion.  It takes an input value, in this case
the result of the `gizmo.getAxis()` function, an input range (0-255),
and an output range (0-180) and performs all the math in the
background to transpose from one range to the other.  The output of
these computations is saved to temporary values to contain the target
left and right motor speeds, but we could just as easily have
condensed the map function into the `write()` for the motors.

To drive a robot around using this code, upload it to the Gizmo and
make sure you have a [practice mode](/field/practice.md) running.  You
should see the 3 status LEDs on the Gizmo indicate green on top for a
stable wireless connection, then a blinking white light in the middle
to indicate the practice field is connected, and the bottom status LED
will change colors based on the battery voltage.  Once the middle LED
starts blinking white, try pushing on the joysticks and the motors
should respond.  If one motor spins in the wrong direction, you can
either fix this problem in software by inverting the range for that
motor, or you can just swap the wires around to physically reverse the
polarity of the motor.

## Recap

In this tutorial we loaded the BasicRobot demo code and drove a robot
around using the `gizmo` practice field.  From here you can use the
BasicRobot program as a starting point to make your own additions.
Try adding another motor to control a manipulator, consider adding
servos for movement, or try reading limit switches to navigate by
bumping up against walls fully autonomously.

Remember you can always learn more about the Arduino environment on
the [Arduino Website](https://arduino.cc), or if you get stuck, you
can [get help](/appendix/help.md).
