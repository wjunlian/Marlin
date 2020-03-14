# FLSUN QQ-S Marlin 2.0.5 release

## Upgrade
To upgrade to Marlin, take the correct file from the directory below, put it on the sd card start the printer and let it self update.

To revert take the files from original firmware (robin_mini.bin, robin_mini_config.txt, mks_pic and mks_font) and let the printer update in same way as normal

## Configuring Marlin
(At least this is how I do it)
1. Start by initializing EEPROM (Configuration/Advanced Settings/Initialize EEPROM)
2. Mount the leveling switch, leveling plate is not recommended but _could_ work
3. Start the delta calibration (Configuration/Delta Calibration/Auto Calibration) This will take a long time but it is doing a good job.
4. Start bed leveling (Motion/Level Bed)
5. Remove the leveling switch.
6. Turn off soft end stops to make it possible to move z below zero (Motion/Move Axis/Soft Endstops)
7. Move the nozzle down until it is at a paper from the bed (Motion/Move Axis/Move Z)
8. Now note the Z height that the printer is at, should be negative, and use this value as Probe offset (Configuration/Probe Z offset)
As the menu goes 0.01 mm per touch this can be easier to do if you have it connected to the USB with pronterface, then this is done with M851, for example if the offset is 8.45 mm then send 'M851 Z-8.45'
9. Now configuration is done, save to EEPROM (Coinfiguration/Store Settings)

When starting the prints until you know your settings are correct, use a skirt and use babystep to trim that first layer (Tune/z babystep)
Calibration, E.steps

## Calibration

### Dimensional calibration
To check the dimensional calibration you start by printing a 100x100mm calibration square.
Measure the real values, if inaccurate calculate new delta rod lenght, new rod length = old rod lenght * 100/measured value ,
so if you printed the square and it was 101 mm then the new delta rod value would be 280.0*101/100 = 282.2

This setting is found under Configuration/Delta Calibration/Delta Settings/Diag Rods:

Remember to save to EEPROM when you have changed it.

### PID tune
PID tuning should be done if you want really stable temperatures, this _can_ be done for the hotend in the menu Configuration/Advanced Settings/Temperature
but the user feedback is totally missing so I recommend doing this with gcodes instead. For procedure just google it, there are no difference for the flsun delta printers than any other printer.


### Calibrating E steps
To calibrate the proper E-step value remove the bowden tube from either the hotend or the extruder, heat up the hotend to >170C,
extrude so the filament is sticking out of the extruder or the ptfe depending on where you disconnected it. 

Cut the filament flush to the opening.

Order the printer to extrude 100mm either with the menu or as I prefer with gcode byt first sending M83 to set it in relative mode and then G1 E100 F100

Now measure the extruded length and if it is not correct then you need to set the new E-step to old e-step * 100/length extruded in mm

So for example your old E-step was 400, you extruded 90 mm , this gives the new e-step 400*100/90 = 444.4 (This is not any real values just an example..)

To change your e-step (and to see what your current e-step is) you go into the menu Configuration/Advanced Configuration/Steps/mm and there you can find and change the extruder steps/mm

When you have changed the steps setting remember to save to EEPROM with Configuration/Store Settings

### Calibrate flow
Flow or extrusion multiplier is a claibbration to fix differences between different rolls of filament. Basically you print a hollow qube with one or two line width wall and measure the resulting cube wall width. 

For example you take a cube, slice it with Line Width 0.4, 0% infill, Wall line count 2, Top Layers 0, Flow 100% . Print the cube and measure the wall width, if the wall width is more than 0.8 mm then reduce flow, if less than 0.8 then increase flow.  


## The QQ-S board(s):
I have been using Marlin on my QQ-S with home buildt leveling switch using the rest as stock and below you will find a firmware for that combo.

However I was no fan of the stock board as it is very loud and gives a lot of salmon effect, you can get rid of most with TL-smoothers but I am no fan of that solution as they will reduce print quality. 

I also do not like the stepper motor dampers for the same reason and without them the printer is ridiciously loud.

So I have moved on and are currently using a skr 1.3 board with tmc2209, no smoothers, no dampers, and print quality is really good. I have included firmware for skr 1.3 with 2209 also.

The Flsun hispeed board was not available when I changed board or it could have been an option. But it also is totally lacking documentation which from a firmware point of view is really bad as we would have to guess on the hardware design of the board. Thirdly Flsun have delivered two totally different boards with step sticks or mounted drivers and both boards are named version 1.0 which is really bad practice as they are obviously not the same (no vref potentionmeters on the mountesd variant..)
So this is why I am not doing version for that board (If someone send my one including documentation that could maybe change)

I also have done some configuration for the Q5 but as I don't have one it has been tested by others. Unfortunatly the Q5 configuration is currently not building and I have no clue on what have happend with it in Marlin... But the config is here so if you have one and like to do some detective work please go ahead...

