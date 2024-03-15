# Programming Languages

Just like languages that people speak, computers speak languages too.
These programming languages are used to provide specific instructions
to the computer for what you want it to do, how you want it to do it,
and what to do if things go wrong.  The Gizmo's User Processor is a
Raspberry Pi Pico microcontroller which can be programmed in a variety
of languages.

The Gizmo team provides explicit support for some of these languages
with pre-fabricated support libraries, documentation, and a decent
idea of what may have broken when you reach out for help.  If you
don't like any of the languages that we've chosen to support, it
doesn't mean you're out of luck!  The Raspberry Pi Pico is based on an
ARM Cortex-M0+ processor which has broad support across many popular
and niche programming languages.  If you want to use a different
language than what we've provided, we'd love to see what you come up
with!  For more information on how to do that, see [adding a new
language](add_new.md).

For using an existing language, there's several choices to choose
from.  See below for a list of available options, and consider trying
several before you decide which one is right for you.

## Arduino

Arduino is an integrated system comprising an Integrated Development
Environment (IDE), compiler, linker, and vast selection of libraries.
The firmware for the System Processor was developed using frameworks
provided by the Arduino ecosystem.

Programming with the Arduino environment is done using the C
programming language, though it is also possible to use C++ for
additional capabilities.  Arduino is a text based environment that
runs on your computer and produces a software image that gets
installed onto the Gizmo.  While often positioned as an advanced
language, C does not have to be.

The Gizmo team recommends using Arduino if you have existing
experience writing software, or require the highest performance in
your programs.

## Circuit Python

Python is an extremely popular language for building programs quickly
as well as small tools.  Python is an interpreted language, so it
won't go as fast as a compiled language like C, but if you don't need
raw speed its a great choice to get up and running quickly.  Circuit
Python is a dialect of Python that is built specifically for
microcontrollers such as those used on the Gizmo.  Circuit Python also
has a rich suite of documentation and activities maintained by its
sponsor, [Adafruit](https://adafruit.com).

Circuit Python is a text based environment which does not require any
special tools on a computer to use, you can even write your code using
Notepad, though the Gizmo team recommends you use a more powerful
editor such as Notepad++ or Visual Studio Code for Python.

The Gizmo team recommends Circuit Python for users that have some
understanding of how programs work, but don't want to deal with the
more rigorous requirements of programming in C.  Python is a great
choice for first time programmers as well as its used in a number of
introductory curriculums ranging from 6th grade through to the
collegiate level.

## Microblocks

Microblocks is a drag-and-drop programming environment reminiscent of
the Scratch programming language.  This programming language requires
nothing more than a browser to get started as the entire environment
runs as a web-page.  Do note that Google Chrome is required.

The Gizmo team recommends Microblocks for anyone who's never written a
program before, or environments where students writing code may not be
proficient with touch typing.
