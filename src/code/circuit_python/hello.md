# Hello World

Let's begin writing some simple Circuit Python code for your Gizmo. A
common beginner program when working with microcontrollers is blinking an
LED. Conveniently, the Gizmo's student processor has an LED built in to
it. This lesson will introduce some of the most common Circuit Python
modules and will walk you through creating a program that blinks the built
in LED. This code won't require any extra hardware plugged into your
Gizmo. You will only need the Gizmo itself and the USB programming cable.

Connect the USB programming cable to the student processor on your Gizmo
and plug it into the computer.

## What is a module?

When writing any kind of software, it is helpful to reuse code that other
people have written. They've already put in the work to make sure it works
correctly! A "library" is collection of helpful code that is shared
between projects. In Python, every library is made up of one or more
"modules." A module is a single file of Python code that defines useful
objects or functions for other Python files to use.

In order to use the tools a module provides, we have to import it into our
script. This is done with an "import statement". These are usually at the
top of a Python script and look like this:

```Python
import module_name
```

You can learn more about the details of Python modules
[here](https://docs.python.org/3/tutorial/modules.html). For now, the
important thing to understand is that we use `import` to pull in useful
code from another file.

## The board Module

The `board` module is the Circuit Python module that gives constants for
the pins and other features your board has. The contents of the `board`
module are specific to the device running the code.

For this tutorial, we'll be using the `board.LED` pin constant. This "pin"
isn't one of the pins that run along the edge of the board. Instead, it is
wired up to control the LED built into the Raspberry Pi Pico.

## The digitalio Module

The `digitalio` module gives us control over the digital input/output pins
available on a microcontroller. API documentation for this module is
available
[here](https://docs.circuitpython.org/en/latest/shared-bindings/digitalio/index.html).

With this module, we can use the `DigitalInOut` class to control pins.

```Python
pin = digitalio.DigitalInOut(board.LED)
pin.direction = digitalio.Direction.OUTPUT

pin.value = True  # Set the pin output high
pin.value = False  # Set the pin output low
```

This code creates a `DigitalInOut` object to control the built in LED pin.
Because we want to control the voltage of this pin with our code, we set
the direction to `OUTPUT`. If we were instead connecting a sensor we
wanted to measure from, we would use the `INPUT` direction. Finally, to
change the output voltage on this pin, we set the `value` property to
either `True` or `False`, which are translated into a high signal (3.3
volts) and low signal (0 volts) respectively.

## The time Module

The `time` module provides a handful of time and timing related functions.
API documentation for this module is available
[here](https://docs.circuitpython.org/en/latest/shared-bindings/time/index.html).

In this tutorial, we'll be using `time.sleep()`. This function pauses the
program for the number of seconds you give the function. For example,
`time.sleep(1.0)` pauses the code for one second.

> [!WARNING]
>
> When the program is sleeping, it is not doing other work like checking
> sensors or responding to gamepad inputs. This might not always be what
> you want, so pay attention to when you add sleeps to your code and how
> long they are.

## Basic Program Structure

Circuit Python programs are scripts where each line of code is executed in
order from the top of the file to the bottom. Technically, an empty file
is a valid Circuit Python program that does nothing. Most Circuit Python
code follows a common pattern where everything the program needs is set up
at the start and then a loop runs repeating the same set of instructions
forever until the device is turned off. Here's what that looks like in
code:

```Python
# Imports at the top to grab the modules the code uses

# Setup code that runs once at the start

while True:
  # Instructions to repeat forever
  pass
```

> [!NOTE]
>
> Lines that start with a # symbol in Python are comments. They are for
> adding notes to our code that the computer won't try to read.

> [!NOTE]
> 
> The `pass` keyword lets us write empty loops or functions without
> breaking Python's rules about indentation. It's common as a placeholder
> when writing code.

## Blinking an LED

No we can put everything from the above sections together into a program
the turns the built in LED on an off.

First, we need to import the modules our code will use.

```Python
import board
import digitalio
import time
```

Then, in the setup part of our program, we'll create the `DigitalInOut`
object for controlling the LED and set it in the output direction.

```Python
led = digitalio.DigitalInOut(board.LED)
led.direction = digitalio.Direction.OUTPUT
```

Finally, our program's loop will turn the LED on, wait some time, turn the
LED off, and wait some more time.

```Python
while True:
  led.value = True
  time.sleep(1.0)
  led.value = False
  time.sleep(1.0)
```

Altogether, our program looks like this.

```Python
import board
import digitalio
import time


led = digitalio.DigitalInOut(board.LED)
led.direction = digitalio.Direction.OUTPUT


while True:
  led.value = True
  time.sleep(1.0)
  led.value = False
  time.sleep(1.0)
```

You can try this code on your Gizmo by connecting the USB cable to the
student processor on your Gizmo and saving the code there in the Mu
editor. You should see the LED on the student processor blinking.

This program works, but can we improve it?  We know that if the light
is on we want to turn it off and if its off we want to turn it on.
Logically, we want the inverse of whatever state the light is in
currently.  Lets refactor our loop to use this knowledge by reading
the value of the light and inverting it.

```Python
while True:
  led.value = not led.value
  time.sleep(1.0)
```

Our 4 line loop is only 2 lines long now, that's a 50% reduction in
code to achieve the same result!  How does it work though?  Setting the
`value` property of our `DigitalInOut` object controls the state of the
pin. This property also remembers what we last set it to. We invert the
old value with the `not` keyword, which turns `False` to `True` and vice
versa.

This works well for our blinking light because we don't care what
state the light is in each second, we just want it to transition to
whatever state its not in now.

Try changing the delay statements or creating patterns with different
combinations of delays and `True`/`False` commands to get a better
understanding of what each change effects.

## Recap

In this program we learned about the `board`, `time`, and `digitalio`
modules. We also learned the common structure of a Circuit Python program
and how to control digital output pins.
