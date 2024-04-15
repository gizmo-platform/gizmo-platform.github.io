# Drive a Robot

So far, these tutorials have covered the basics of using Circuit Python to
program your Gizmo. This includes things like Python language concepts,
useful modules, and Gizmo-specific features. In this tutorial, we'll walk
through the "basic_robot.py" example program from the
`circuitpython_gizmo` module. This code uses gamepad inputs to control two
motors. With this, you can make a robot with two wheels that drives around.

## Getting the Code

The `circuitpython_gizmo` module also provides a handful of example
programs to show how to use different Gizmo features. You can download
these examples by grabbing the examples zip file from the
[latest release](https://github.com/gizmo-platform/CircuitPython_Gizmo/releases/latest).
When you open that zip file, you should see several Python files. We're
going to use "basic_robot.py".

This is what that code looks like (without comments, for brevity):

```Python
import board
import time
import pwmio
import digitalio
from adafruit_motor import servo
from adafruit_simplemath import map_range
from circuitpython_gizmo import Gizmo

gizmo = Gizmo()

motor_left = servo.ContinuousServo(pwmio.PWMOut(gizmo.MOTOR_1, frequency=50))
motor_right = servo.ContinuousServo(pwmio.PWMOut(gizmo.MOTOR_2, frequency=50))

builtin_led = digitalio.DigitalInOut(board.GP25)
builtin_led.direction = digitalio.Direction.OUTPUT

while True:
    builtin_led.value = not builtin_led.value

    gizmo.refresh()

    throttle_left = map_range(gizmo.axes.left_y, 0, 255, -1.0, 1.0)
    throttle_right = map_range(gizmo.axes.right_y, 0, 255, -1.0, 1.0)

    motor_left.throttle = throttle_left
    motor_right.throttle = throttle_right

    time.sleep(0.02)
```

This program is only 28 lines of code and it can drive a robot. Now, this
robot doesn't do much, but it is a great starting point to extend further.
As written, this code expects a left and right drive motor to be attached
to the first 2 motor ports. It also toggles the built-in LED to show that
the program is running and hasn't frozen.

For more detail about what each component does, the example code in the
released zip is heavily commented with descriptions for each component.
Hopefully you recognize most of the contents as the building blocks
introduced in earlier tutorials.

The one portion of this code that hasn't been shown yet in any of our
previous programs is the `map_range()` function. These two lines do a
really important task:

```Python
throttle_left = map_range(gizmo.axes.left_y, 0, 255, -1.0, 1.0)
throttle_right = map_range(gizmo.axes.right_y, 0, 255, -1.0, 1.0)
```

The joystick axis values range from 0 to 255, but the motor control
range is from -1.0 to 1.0. The mapping between these two values is just
a linear proportion, but the `map_range()` function saves the effort of
writing out this proportion. It takes an input value, in this case
the value of the `gizmo.axes.left_y` or `gizmo.axes.right_y` properties,
an input range (0 to 255), and an output range (-1.0 to 1.0) and performs
all the math in the background to transpose from one range to the other.
The output of these computations is saved to temporary values to contain
the target left and right motor speeds, but we could just as easily have
condensed the code to store the values directly in the throttle parameters
of the motor objects.

To drive a robot around using this code, save it to the Gizmo and
make sure you have a [practice mode](/field/practice.md) running. You
should see the 3 status LEDs on the Gizmo indicate green on top for a
stable wireless connection, then a blinking white light in the middle
to indicate the practice field is connected, and the bottom status LED
will change colors based on the battery voltage. Once the middle LED
starts blinking white, try pushing on the joysticks and the motors
should respond. If one motor spins in the wrong direction, you can
either fix this problem in software by inverting the range for that
motor, or you can just swap the wires around to physically reverse the
polarity of the motor.

## Recap

In this tutorial we loaded the basic_robot.py demo code and drove a robot
around using the `gizmo` practice field. From here you can use the
basic_robot.py program as a starting point to make your own additions.
Try adding another motor to control a manipulator, consider adding
servos for movement, or try reading limit switches to navigate by
bumping up against walls fully autonomously.

Remember you can always learn more about Circuit Python on
[the Circuit Python website](https://docs.circuitpython.org/) and
[Adafruit Circuit Python tutorials](https://learn.adafruit.com/welcome-to-circuitpython),
or if you get stuck, you can [get help](/appendix/help.md).
