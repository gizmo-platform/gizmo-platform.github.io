# Hello World

Lets start off with a really simple program.  It won't use the
gamepad, `gizmo` tool, or any other hardware.  This program will
introduce the concepts of the `loop()`, `setup()`, `pinmode()`,
`delay()`, `digitalWrite()`, and `digitalread()` primitives which are
fundamental to more complicated programs.

Within the Arduino language specification there are several important
functions.  Lets look at each one now:

  * `pinMode(pin, <INPUT|OUTPUT>)` - This function tells the processor
    which way we want signals to go on a particular pin.  In general
    pins that you want to read status from are inputs and pins you
    want to export a state to are outputs.

  * `delay(millis)` - This function causes the processor to wait an
    amount of time before proceeding to the next instruction.  You can
    specify the amount of time to wait in milliseconds.  Recall that 1
    second is composed of 1000 milliseconds, so if we want our program
    to wait for 1 second, we would write `delay(1000);`.

  * `digitalWrite(pin, <HIGH|LOW>)` - This function sets the value of
    a digital I/O pin.  The `pin` refers to a pin number, or a named
    pin constant as provided by the Gizmo library.  The value the pin
    will be set to should be either `HIGH` or `LOW` referring to
    whether or not the pin should source current or sink it.  Phrased
    differently, setting the pin `HIGH` will raise the voltage present
    at the pin to a nominal logic level of 3.3v.  Setting it `LOW`
    will ground the pin to a nominal voltage of 0v.

  * `digitalRead(pin)` - This is the inverse of the `digitalWrite()`
    function and reads the value of a pin.  If the pin has voltage
    being applied to it, the function will return the special symbol
    `HIGH`.  In all other cases, the return value will be `LOW`.
    Using an external circuit to provide either voltage or ground to a
    signal pin can be a powerful tool to detect the state of a
    mechanism that has 2 possible states, such as a limit switch.

In addition to the above functions, most Arduino programs will make
use of two special functions that are part of the Arduino frameowrk.
These are the `setup()` and `loop()` functions.  The `setup()`
function is fairly intuitive based on the name, as you might guess, it
is where you put code that is going to perform an initialization task
to prepare for further work to be done.

The `loop()` function is slightly more complicated, and is the primary
entrypoint to code you write.  This means that once the Arduino
framework has performed global initialization, processed all the
functions that you defined in `setup()` and processed any internal
housekeeping, the `loop()` function will be called and, as the name
implies it will be called in a loop.  This means that code you put in
this loop function will be run in a repeated nature until you remove
the power from the Gizmo.

> [!NOTE]
>
> In these examples, you'll see the keyword `void` used in front of
> `setup()` and `loop()`.  This is called a type and instructs the
> compiler that processes our program that these functions won't
> return any values.  Learn more about the `void` keyword
> [here](https://www.arduino.cc/reference/en/language/variables/data-types/void/).

Lets put this into practice and write a program that will blink the
onboard status LED that's part of the User Processor.  First, we'll
write the `setup()` function.

```C
void setup() {
    pinMode(LED_BUILTIN, OUTPUT);
}
```

Above we learned that the `pinMode()` function takes a number as its
first argument, but here we passed the symbol `LED_BUILTIN`, how does
that work?  Within the Arduino world lots of boards have a small light
built in for user applications.  You can blink this light, use it to
indicate a status, or ignore it.  To make these lights easy to use,
they are connected to a pin that has a name, in this case called
`LED_BUILTIN`.

Next, lets write the `loop()` function that will turn the light on,
wait 1 second, then turn it off again and wait another second.  This
will result in an even blinking pattern where the light is on half the
time and off half the time.

```C
void loop() {
    digitalWrite(LED_BUILTIN, HIGH);
    delay(1000);
    digitalWrite(LED_BUILTIN, LOW);
    delay(1000);
}
```

> [!WARNING]
>
> While the processor is waiting it can't do anything else!  This
> can be okay in some circumstances, but is usually not what you
> want to do because it means that other tasks such as maintaining
> motor controllers, reading sensor values, or processing gamepad
> inputs won't be processed.

Altogether, our program looks like this:

```C
void setup() {
    pinMode(LED_BUILTIN, OUTPUT);
}

void loop() {
    digitalWrite(LED_BUILTIN, HIGH);
    delay(1000);
    digitalWrite(LED_BUILTIN, LOW);
    delay(1000);
}
```

You can compile and upload this program to the Gizmo by plugging a USB
cable into the right hand USB port on the top of the board and
clicking the "Upload" arrow in the Arduino IDE. Note that you may need
to select the serial port in the Tools menu if it was not
automatically detected.

This program works, but can we improve it?  We know that if the light
is on we want to turn it off and if its off we want to turn it on.
Logically, we want the inverse of whatever state the light is in
currently.  Lets refactor our loop to use this knowledge by reading
the value of the light and inverting it.

```C
void loop() {
    digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
    delay(1000);
}
```

Our 4 line loop is only 2 lines long now, that's a 50% reduction in
code to achieve the same result!  How does it work though?  Recall
that the `digitalWrite` function needs to know whether or not we want
to drive the pin to a `HIGH` or `LOW` state.  Further recall that the
`digitalRead` function checks what state a pin is in and reports that
as a `HIGH` or `LOW` status.  Since we want the logical opposite of
whatever state the LED is in, we can use the logical `!` operator
which means "NOT".

If we were to read out the line in English we might say it like so:

> Perform a digital write to the pin connected to the LED with the
> opposite value of the pin connected to the LED.

This works well for our blinking light because we don't care what
state the light is in each second, we just want it to transition to
whatever state its not in now.  This also reveals part of how `HIGH`
and `LOW` work, since the `!` operator only makes sense to use on
boolean values.  The `HIGH` and `LOW` names are just synonyms for
`true` and `false`.

Try changing the delay statements or creating patterns with different
combinations of delays and `HIGH`/`LOW` commands to get a better
understanding of what each change effects.

## Recap

In this program we learned about the `setup()` function, the `loop()`
function, and functions related to input and output of digital values.
We also learned how to combine the results of a digital read with a
digital write to compact our code into fewer lines.
