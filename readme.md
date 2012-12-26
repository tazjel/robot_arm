Python Code to drive the Maplin/OWI USB Robot arm

  Tested on:
  * Linux
  * OSX (Lion, Mountain Lion)
  May work on Windows,.

Requirements
============
* Python 2.7
* Libusb
* pyusb

Usage
=====
As a library:

>>> import arm

To initialise libusb and the arm

>>> arm = usb_arm.Arm()

This will tell you if all the dependencies work, and will throw an exception if it fails to find the arm and connect
 to it.

Now lets test it by turning on the LED

>>> arm.move(usb_arm.LedOn)

It will turn on for 1 second, and automatically turn off. The moveArm function automatically turns off after each
move. You can optionally specify another time, but since the Maplin arm doesn't have any sensors, beware that if
it reaches limits before the time finishes, then it won't stop.

Actual movement
---------------

>>> arm.move(usb_arm.ElbowUp)

The elbow will move up.
The movements possible:

OpenGrips
CloseGrips
WristUp
WristDown
ElbowUp
ElbowDown
ShoulderUp
ShoulderDown
BaseClockWise
BaseCtrClockWise

Stop

Combining Movements
-------------------
Movements are based upon the BitPattern class, and you can feed arbitrary bitpatterns to it, but all those the
arm is currently capable of are represented above.

However, you may want to make more than one movement at the same time. You can do this by combining patterns with the
or operator:

>>> arm.move(usb_arm.ElbowDown | usb_arm.BaseClockWise, 0.5)

The arm should turn clockwise and bring the elbow up simultaneously for half a second.

Gear Lash
---------

The unmodified arm has a few flaws - it has fairly loose gear chains in the "servos" it uses for the movements.
To see what I mean try the following:

>>> arm.move(usb_arm.ShoulderUp, 0.5)
>>> arm.move(usb_arm.ShoulderDown, 0.5)

You will note the arm moves, but when it returns, it does not quite return to the same position - there is an error,
which you will need to account for as you use the arm and in programmed sequences.

You should now know enough to move the arm to any location.

Sequences of Actions
--------------------
You can create programmed sequences of actions for the robot. However, before you issue one of these, ensure you
know the position of the arm, and wont move it past its limits - which could cause damage to it,

Sequences are created as arrays of commands. Each command is an array of the bitpattern, followed by the
optional time (defaulting to 1 second):

>>> actions = [[usb_arm.ElbowDown, 0.5], [usb_arm.CloseGrips, 0.5], [usb_arm.ElbowUp]]

To issue the action list:

>>> arm.doActions(actions)

Note you can ctrl-c stop the movements.
There are a couple of canned actions already in the module:

    block_left
    block_right
    left_and_blink
