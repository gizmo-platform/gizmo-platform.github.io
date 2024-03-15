# Adding Support for a New Programming Language

Want to program your Gizmo in a language that we don't currently
support?  You're in the right place!  Adding support for new languages
can broadly be split into two categories depending on if a toolchain
already exists or not.  A toolchain is just support for the Raspbery
Pi Pico's ARM Cortex-M0+.  You can check if your language supports
this or not by seeing if its already possible to program the Raspberry
Pi Pico.

While its possible to do the end-to-end development of a language
support package in a relatively short time span (the Gizmo Team wrote
most of the language support available today in about 2 weeks), we
don't recommend you do this during a competition season.

## Toolchain Exists

If a toolchain exists, you only need to write a small amount of code
that allows your program to talk to the System Processor.  For
reference, the entire library that supports the Arduino environment is
less than 200 lines of code.

Refresh your understanding of the
[architecture](/learn/architecture.md) of the Gizmo.  The library you
are going to write will need to communicate using the I2C protocol to
the System Processor and then decode the responses into a format that
will be useful for your program.

You can check the exact formats of data by reviewing the [firmware
source code](https://github.com/gizmo-platform/firmware/).  You'll
need to implement the otherside of the `wireRespond` function to
create an I2C/Wire request, send it, and then unmarshal the data that
you get back.

Lets look at the wireRespond function a little more closely:

```C
void wireRespond() {
    byte toSend[18] = {
    cstate.Axis0,
    cstate.Axis1,
    cstate.Axis2,
    cstate.Axis3,
    cstate.Axis4,
    cstate.Axis5,
    cstate.Button0,
    cstate.Button1,
    cstate.Button2,
    cstate.Button3,
    cstate.Button4,
    cstate.Button5,
    cstate.Button6,
    cstate.Button7,
    cstate.Button8,
    cstate.Button9,
    cstate.Button10,
    cstate.Button11,
  };
  Wire1.write(toSend, 18);
}
```

You can see that all this function does is compress the axis and
button data into an array and then write it to the I2C bus.  This is a
very simplistic protocol and doesn't include any error handling or
data framing.  This allows it to be extremely portable across
languages, and since this data can always be re-requested if a read
fails doesn't represent too much risk.

To implement support in your own language, you'll need to write code
that performs the inverse of the `wireRespond` function by creating a
request and sending it, then unpacking the data that you get in
return.  Here's what the `refresh` function looks like from the
[ArduinoGizmo](https://github.com/gizmo-platform/ArduinoGizmo)
library.

```C++
void Gizmo::refresh() {
  // The wire format of the Gizmo processor-to-processor interconnect
  // is the classic "bag of structs" variety, which makes it easy to
  // interact with from a variety of languages.
  size_t amountRead = Wire1.requestFrom(GIZMO_ADDR, sizeof(_state));

  if (Wire1.available() >= sizeof(_state)) {
    Wire1.readBytes(reinterpret_cast<uint8_t*>(&_state), sizeof(_state));
  } else {
    _state.Axis0 = 127;
    _state.Axis1 = 127;
    _state.Axis2 = 127;
    _state.Axis3 = 127;
    _state.Axis4 = 127;
    _state.Axis5 = 127;
    _state.Button0 = false;
    _state.Button1 = false;
    _state.Button2 = false;
    _state.Button3 = false;
    _state.Button4 = false;
    _state.Button5 = false;
    _state.Button6 = false;
    _state.Button7 = false;
    _state.Button8 = false;
    _state.Button9 = false;
    _state.Button10 = false;
    _state.Button11 = false;
  }
}
```

The `_state` variable is defined elsewhere by the following definition:

```C++
struct CState {
  byte Axis0;
  byte Axis1;
  byte Axis2;
  byte Axis3;
  byte Axis4;
  byte Axis5;

  byte Button0;
  byte Button1;
  byte Button2;
  byte Button3;
  byte Button4;
  byte Button5;
  byte Button6;
  byte Button7;
  byte Button8;
  byte Button9;
  byte Button10;
  byte Button11;
};

CState _state;
```

Because we know that we want to read the amount of data required to
fill the CState structure, we can tell the `Wire` library to read that
many bytes.  If the number of bytes read doesn't match, then we
provide neutral values instead of whatever got read to prevent any
runaway robots.  Once we're happy that the right amount of data was
read, we perform a `reinterpret_cast` which tells the processor that
we want it to take the raw information as a series of ones and zeros
and reintepret it as the given structure.  In this case, the `CState`
structure that contains all the button and axis information.

The ArduinoGizmo library includes helpful functions to fish values out
of the state structure, but this is not strictly required.  You've now
seen what it takes to add support for the Gizmo to a new language by
writing code that talks to the System Processor.  This is a relatively
straightforward process, but if you get stuck you can always [reach
out](/appendix/help.md) and ask for review of your code.


## Toolchain Does Not Exist

Unfortunately if a toolchain from your chosen language does not exist
for the Raspberry Pi Pico you're in for an uphill battle.  At minimum,
you'll need to implement the compiler backend, the low level board
support package, and I2C peripheral drivers in your language of
choice, all of which is beyond the scope of this document.  Still feel
free to [reach out](/appendix/help.md) and if its a language with
sufficient interest, the Gizmo Team may be able to help you build some
of this support.
