# Setup

While no specific editor or tools are _required_ for using Circuit Python,
this documentation will make use of the recommended tools, namely the Mu
editor and CircUp library management tools.

## Installing the Circuit Python Runtime

In order for your microcontroller to be able to use Circuit Python, you
have to install board-specific firmware.

1. Download the Circuit Python v9.0 runtime (.uf2 file) for the Raspberry Pi
Pico from
<https://circuitpython.org/board/raspberry_pi_pico/>.

   > [!NOTE]
   > 
   > Only versions within 9.0.X (ie. 9.0.4) have been tested with the
   > Gizmo. Newer or older versions may not work.

1. Hold your Gizmo's student processor BOOTSEL ("Boot Select") button down
while connecting the USB programming cable to your computer. Once you've
connected the cable, release the BOOTSEL button.

   You should now see an external drive on your computer called "RPI-RP2".

1. Copy the .uf2 file you downloaded to the "RPI-RP2" drive.

   Your device will automatically reboot and reconnect as a new drive
   called "CIRCUITPY".

> [!WARNING]
>
> Each time you reinstall the CircuitPython runtime, any programs on your
> Gizmo will be erased.

> [!TIP]
> 
> If your Gizmo ever gets stuck in a way that prevents you from saving new
> Circuit Python code to it or if it stops showing up as the CIRCUITPY
> drive when you connect it, you may need to re-install the runtime by
> following the above steps again.

## Installing the Mu Editor

[Mu](https://codewith.mu/) is a simple Python editor and is the
recommended editor for Circuit Python code.

If you would like more information about using the Mu editor, including an
introduction to the user interface, checkout the
[Mu tutorial articles](https://codewith.mu/en/tutorials).

1. Download and run the installer for your operating system from
   [the Mu download page](https://codewith.mu/en/download).

1. When prompted for what mode to run Mu in, make sure to select Circuit
   Python.

   ![Mu mode selection screenshot](https://cdn-learn.adafruit.com/assets/assets/000/105/681/medium640/circuitpython_WtCP_Mu_mode_dialogue.png)

1. Mu tries to automatically connect to Circuit Python devices attached to
   your computer. By default, when you save the code you're writing in Mu,
   it will save directly to your Circuit Python device. If it can't find
   one, it will show you this warning explaining where the code you write
   will be saved on your computer.

   ![Mu device not found screenshot](https://cdn-learn.adafruit.com/assets/assets/000/105/679/medium640/circuitpython_WtCP_Mu_device_not_found.png)

## Installing the CircUp Library Manager

Like your code, libraries for Circuit Python devices are managed by
copying files onto the CIRCUITPY drive. While this can be done manually,
it involves quite a bit of downloading files from different websites and
extracting zipped folders. The CircUp tool manages these processes for you
to keep your installed libraries up to date.

Adafruit provides detailed installation instructions for Windows, Mac, and
Linux. You can find these instructions
[here](https://learn.adafruit.com/keep-your-circuitpython-libraries-on-devices-up-to-date-with-circup).
Specifically, follow the steps in the "Prepare for Install" and "Install
CircUp" sections.

## Installing the Gizmo Circuit Python Library

To install the Gizmo Circuit Python library with the CircUp tool, run
these commands in the terminal on your computer:

1. Add the bundle to your local list.

    ```Shell
    $ circup bundle-add gizmo-platform/CircuitPython_Gizmo
    ```

1. Install the module to your connected device.

    ```Shell
    $ circup install circuitpython_gizmo
    ```
