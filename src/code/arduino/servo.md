# Spin a Wheel

In the previous tutorial we used some simple functions to turn a light
on and off.  Lets use some more advanced code to now control a motor.
In this program we'll make use of a VEX MC-29 motor controller.  These
motor controllers use a Pulse Width Modulation signal to determine
direction and magnitude of the motor rotation they control.

To control the MC-29, we'll make use of a software library.  A
software library is a collection of code written by someone else which
is packaged up into a format that allows it to be re-used.  Libraries
are a core component of most programs and encapsulate standard
functionality that is abstract enough to be useful across multiple
programs.

For this program, we're going to use the `Servo` library to make the
motor slowly ramp from spinning in one direction to spinning in the
other direction.  Full documentation for the `Servo` library can be
found [here](https://www.arduino.cc/reference/en/libraries/servo/).

Why are we using a library for Servos to control a motor?  Servos are
just a special kind of motor that are aware of how far they've spun.
While this is really useful for a lot of applications where absolute
positioning is required, like the fuel gauge in a car, its less useful
when we want a continuous source or rotation like a DC gear motor.  In
these cases we can use a Continuous Rotation Servo which takes the
same input signals as a servo, but rather than moving to a particular
location in absolute terms, we control the rate of rotation in
relative terms.

To start our program, we'll need to tell the Arduino compiler that we
intend to use the Servo library, and that we want to connect a servo
compatible device to motor port 1 on the Gizmo.  Motor port 1 is the
lowest connector on the right vertical grouping of connectors.  The
signal wire (white) for the MC-29 should be on the left side of the
connector when it is plugged in.

```C
#include <Servo.h>
```

This tells the compiler that we want to include the Servo library and
make its functions available to the rest of the program.  To use the
names for ports rather than the numbers, we'll also need the Gizmo's
library which provides these names.  Include it in the same way:

```C
#include <Gizmo.h>
```

Now that we have the servo library, we can make use of it and define a
motor to use later:

```C
Servo myMotor;
```

Just as in our blinking light program, we'll perform setup tasks in
the `setup()` function:

```C
void setup() {
    myMotor.attach(GIZMO_MOTOR_1);
}
```

This code tells `myMotor` to attach to and take control of the pin
that the motor is connected to.  On the Gizmo we name these pins so
that its easier to understand what they're connected to, and to make
code work across multiple different generations of the Gizmo's
physical hardware.  In this case, we're using the #1 motor port.

Lets make the motor move.  We'll do that with 2 loops, one to ramp
from full reverse slowly to full forward, and one to go from full
forward to slowly full reverse.  Since we want to output a range of
values from one extreme to another, we'll use a `for` loop inside our
larger `loop()` function.

A `for()` loop has the following syntax:

```C
for(initialization, condition, increment)
```

You can read the full documetation of the `for` construct
[here](https://www.arduino.cc/reference/en/language/structure/control-structure/for/),
but the abridged version is as follows: The initialization code
happens exactly once on the first trip through the loop.  The
condition is checked every time and has to evalute as a true
expression to continue running the loop, and the increment happens
after each trip through the loop.  This is pretty abstract, so lets
write a loop and see how it works:


```
for (int i=0 ; i<180 ; i++) {
    myMotor.write(i);
    delay(20);
}
```

This creates an integer variable called `i` and sets its value to 0,
then says that this loop will run as long as the value of `i` is less
than or equal to 180.  Finally, each time through the loop we'll
increase the value of `i` by one.  The `i++` is a more compact way of
writing `i = i + 1`, but it does the same thing.  Then, each time
through the body of the loop (the indented part), we tell the motor to
change speed to the current value of `i` and wait 20 milliseconds.  We
have to wait because the motor has some inertia and needs time to
accelerate.

You might notice that we're incrementing from 0 to 180, and this is
going from full speed in one direction to full speed in another.  The
Servo library uses a range from 0 to 180 to express output values
because most standard servos can sweep through 180 degrees with a
centerpoint at 90 degrees.  Since our servo is a continuous rotation
motor, this means that we have 90 steps of speed in one direction for
values ranging from 0-89, then a stopped motor at a value of 90, then
another set of steps in the other direction ranging from 91-180.

Lets put all of what we've learned together to write the whole
program.  You'll notice that for sending the motor the other way its
the same loop body, but the numbers at the top are reversed so the
loop goes from 180 to 0 now.

```C
#include <Servo.h>
#include <Gizmo.h>

Servo myMotor;

void setup() {
    myMotor.attach(GIZMO_MOTOR_1);
}

void loop() {
    for (int i=0 ; i<180 ; i++) {
        myMotor.write(i);
        delay(20);
    }
    for (int i=180 ; i>0 ; i--) {
        myMotor.write(i);
        delay(20);
    }
}
```

Each time through `loop()` this code will spin the motor from one
speed extreme to another, reversing the direction in the middle.

> [!NOTE]
>
> Motors take a lot of power.  Way more than can be safely pulled out
> of the USB ports on your computer.  To run this program, you'll need
> to have a battery conected to your Gizmo board.  Check the
> instructions around the [Main Power
> Connector](/startup/index.md#main-power-connector) for more
> information on connecting this cable.

## Recap

In this program we learned about libraries and used the Servo library
to control a motor.  We also learned about how to use `for` loops to
increment and decrement a value in a cycle.
