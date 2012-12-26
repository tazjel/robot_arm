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

>>> arm.reset()

This will tell you if all the dependencies work, and will throw an exception if it fails to find the arm and connect
 to it.

Now lets test it by turning on the LED

>>> arm.moveArm(arm.LedOn)

It will turn on for 1 second, and automatically turn off. The moveArm function automatically turns off after each
move. You can optionally specify another time, but since the Maplin arm doesn't have any sensors, beware that if
it reaches limits before the time finishes, then it won't stop.

Actual movement
---------------

>>> arm.moveArm(arm.ElbowUp)

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

>>> arm.moveArm(arm.ElbowDown | arm.BaseClockWise, 0.5)

The arm should turn clockwise and bring the elbow up simultaneously for half a second.

Gear Lash
---------

The unmodified arm has a few flaws - it has fairly loose gear chains in the "servos" it uses for the movements.
To see what I mean try the following:

>>> arm.moveArm(arm.ShoulderUp, 0.5)
>>> arm.moveArm(arm.ShoulderDown, 0.5)

You will note the arm moves, but when it returns, it does not quite return to the same position - there is an error,
which you will need to account for as you use the arm and in programmed sequences.

You should now know enough to move the arm to any location.

Sequences of Actions
--------------------
You can create programmed sequences of actions for the robot. However, before you issue one of these, ensure you
know the position of the arm, and wont move it past its limits - which could cause damage to it,

Sequences are created as arrays of commands. Each command is an array of the bitpattern, followed by the
optional time (defaulting to 1 second):

>>> actions = [[arm.ElbowDown, 0.5], [arm.CloseGrips, 0.5], [arm.ElbowUp]]

To issue the action list:

>>> arm.doActions(actions)

Note you can ctrl-c stop the movements.
