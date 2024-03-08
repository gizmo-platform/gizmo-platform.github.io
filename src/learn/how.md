# How did the Gizmo get built?

Like everything, the Gizmo had to be conceived of as an idea,
designed, and then fabricated.  This section will give you an overview
of how the team behind the Gizmo designs the hardware, software, and
arranges for these designs to turn into real physical devices.

## Tools and Opinions

Keep in mind that the Gizmo team is largely composed of engineers with
wildly varied professional careers.  These are the tools that we use,
and these are based on our opinions around support, capability, and
ease of use.  If you want to do something similar to the Gizmo,
evaluate what your team knows how to use and select your tools
accordingly.  We largely run on top of Linux which influences our tool
choices significantly.

The Gizmo also leverages a large amount of Open Source software and
hardware tooling.  This tooling is available for free for anyone to
inspect, learn from, use, or build on top of to create some thing new.
If this sounds interesting to you, learn more about Open Source
[here](https://opensource.com/resources/what-open-source).

Across all our workflows, we make use a Distributed Version Control
System (DVCS).  Our DVCS of choice is called Git and is very popular
across industry, education, and hobbyist markets.  Using a DVCS allows
our team to each contribute to different components of the system from
our individual places of work be it at home, in an office, or even a
coffee shop.  Git tracks all the changes that we make to each portion
of our systems, and to revert changes that we don't like.  In order to
collaborate, we make use of a hosted platform for Git called GitHub.
Even the docs you're reading right now are hosted on GitHub for us to
collaboratively edit, review, and publish as changes happen.  If you
want to look at all our different work-spaces (called "repositories" on
GitHub), you can start from our [organization
page](https://github.com/gizmo-platform/).

## Hardware

The Gizmo is physically comprised of a circuit board and a case for
that circuit board.  We use different tools to design each part.

### Printed Circuit Board

We design the Gizmo using an Electronic Design Automation suite called
[KiCAD](https://www.kicad.org/).  This software allows us to lay out
our printed circuit boards, generate the files that our fabrication
partners require, and automatically verify that our designs conform to
certain rules.  KiCAD is functionally similar to other Computer Aided
Design (CAD) suites used for designing mechanical systems such as
SolidWorks, AutoCAD, TinkerCAD or OnShape, just to name a few.

When designing a circuit board, its important to verify that not only
the wires go where we expect, but also that we're observing any rules
that are imposed by the company that manufactures our circuit boards.
We're able to configure KiCAD to automatically check these Design
Rules with Design Rule Checks (DRC).  Design Rule Checks are just like
spellcheck.  We run our designs through both standard DRC workflows,
as well as specific sets of rules that are provided by manufacturers
that validate our designs can be built by their machines.

### Case

We use a couple of different tools to design and fabricate cases,
depending on the manufacturing technology that will be used to produce
the case.  Our standard 3d printed case is designed using
[Blender](https://www.blender.org/).  Blender is an extremely powerful
3d modeling, VFX, rendering, and video workbench.  Best of all, its
Open Source!  Blender allows us to make very fast changes to the
design of the case and directly export a design into the file format
used by most 3d printers.

## Software

The Gizmo ecosystem is comprised of a number of different software
components.  There's firmware that gets loaded onto the System
Processor on the Gizmo board, support libraries for all the languages
that you can use to program your own code on the User Processor and
software that has to run on a computer to orchestrate the data for
multiple robots.  This software is all broadly developed using the
same workflows.

We start with a description of what the software should do, this can
be a description written in English such as "the software should
retrieve gamepad data and make that available to the user", or it can
be a more machine oriented description such as this interface
definition:


```go
// JSController defines the interface that the control server expects
// to be able to serve
type JSController interface {
        GetState(string) (*gamepad.Values, error)
}
```

After we know what the software needs to do we usually write a really
rough first draft of it that is functional, but isn't pretty.  This
rough draft will inform us as to whether the description is complete,
and how the different parts need to fit together.  After writing the
rough draft, we'll circulate it amongst other developers to receive
feedback and advice on how to change or improve the code.  This is a
crucial step that often catches errors in both design and
implementation.

Incorporating the feedback as well as the better understanding we
gained from the rough draft, we then revise our code to a more
polished version.  This may include refactoring code, creating new
modules to encapsulate specific tasks, or scrapping the code and
starting over if our initial assumptions weren't correct.  What's
important is that throughout this process we're creating checkpoints
our work in Git (called 'commits').  These are specific changes that
we write a message describing and can revert to later.

After we've reached a point where we're happy with the code, we need
to release it and distribute it for people to use.  Depending on the
project, we have various ways to do this.  For this `gizmo` tool
itself, we use GitHub Actions to compile it for multiple operating
systems and architectures, then compress these compiled artifacts for
download directly to end users.  For components like our Arduino
library, we create a release tag that the Arduino IDE uses to identify
a new version available for download.

Most of our team works from an operating system based on the [Linux
Kernel](https://www.linux.com/what-is-linux/).  This allows each
member of the team incredible freedom to configure their environments
based on personal taste.  We also have access to Windows computers to
test and document how things work, but most of our development is done
from Linux environments.  Our team is pretty evenly split in which
editors we use, but we almost all use text editors instead of
Integrated Development Environments (IDEs).  An IDE is a complete
environment with toolbars, language documentation, and debugging tools
that you can use to develop programs.  Some languages, like Java, are
almost always programmed from inside an IDE.  For the languages that
the Gizmo platform components are built in (C, Arduino/Wiring, Go) we
prefer to use simpler text editors.  Notepad is an example of a text
editor you may be familiar with.  While most of our team uses either
[`emacs`](https://www.gnu.org/software/emacs/) or
[`vim`](https://www.vim.org/), we recommend that if you want to start
out with a text editor you try [Visual Studio
Code](https://code.visualstudio.com/) which comes with lots of useful
features to help you write and debug software.  Its free and widely
used with lots of tutorial information only a short Google search
away.
