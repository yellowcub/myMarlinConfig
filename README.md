# My Marlin Configuration Files

This repository contains the configuration files I have used to build firmware for my Ender 3 Pro with a BTT SKR E3 mini v3.0 controller board.  Please exercise caution, and do not use these configuration files for your own build unless you are confident your configuration matches, or at least know how to modify the `Configuration.h` and `Configuration_adv.h` files to reflect any differences.

There are a number of useful videos for getting started updating firmware.  I found the following video useful:

[![](https://img.youtube.com/vi/fIl5X2ffdyo/hqdefault.jpg)](https://youtu.be/fIl5X2ffdyo)

I also recommend the following link at [marlinfw.org](https://marlinfw.org/docs/configuration/configuration.html) for an explanation of each of the settings, however it may not entirely reflect the 2.1.x version of parameter names.

These configuration files are based on the [2.1.2 repository](https://github.com/MarlinFirmware/Configurations/releases/tag/2.1.2), and the original versions for comparison can be found in the fork by navigating to the Creality Ender 3 Pro examples.  The files are versioned, and will only work with this fork of the [Marlin Firmware](https://github.com/MarlinFirmware/Marlin/tree/2.1.2) code.

Compiling Marlin Firmware requires the use of VS Code and the PlatformIO extension.  PlatformIO needs to be configured with the model of the board, which can be looked up in the `boards.h` file.  For the BTT SKR E3 mini v3.0 the board is `STM32G0B1RE`, which can be selected within the PlatformIO extension.  Alternatively, you can look up the `platformio.ini` file and change `default_envs` to `STM32G0B1RE_btt`.  The four header files (`.h`) here need to be copied to the Marlin/Marlin directory replacing any existing files of the same name.

It is possible I may have missed some minor changes that did not get described below.  This is a work in progress, and I may be changing other parameters as I get more comfortable with modifying the firmware.

## Configuration

Changes to the `Configurations.h` file consist of:

  - Updating the author and machine names
  - Changing the extrudor heater maximum temperature
  - Enabling Z-Axis probing via the BLTouch
  - Geometry changes
  - Auto Bed Leveling (ABL)
  - Change E0 steps/mm to 400 for Microswiss NG extruder

### Author and Machine Name
The author and machine settings are optional for keeping track of personal firmware builds.

https://github.com/yellowcub/myMarlinConfig/blob/d8b16303a764c005113de82d7aa24b312a7c47fb/src/Configuration.h#L64-L65

https://github.com/yellowcub/myMarlinConfig/blob/d8b16303a764c005113de82d7aa24b312a7c47fb/src/Configuration.h#L90-L93

### Extrudor Maximum Temperature
The maximum temperature for extrudor 0 was changed to 315.

https://github.com/yellowcub/myMarlinConfig/blob/d8b16303a764c005113de82d7aa24b312a7c47fb/src/Configuration.h#L621-L633

This was intended to support the capability of the MicroSwiss all metal hot end upgrade, which should allow requesting temperatures up to 300&deg; C (This accounts for a 15&deg; C overtemp specified further down in the configuration).  In actuality the controller would only permit a maximum temperature of 275&deg; C because the stock thermistor does not permit higher temperatures - for good reasons!  I should be able to increase this if I upgrade the thermistor and select the appropriate thermistor code for `TEM_SENSOR_0`.  For example, for the Creality Sprite Pro Kit thermistor the code should be changed from 1 to 13.

https://github.com/yellowcub/myMarlinConfig/blob/d8b16303a764c005113de82d7aa24b312a7c47fb/src/Configuration.h#L450-L554

### Z-Axis Probing

Z-Axis probing is enabled by uncommenting the line for `USE_PROBE_FOR_Z_HOMING`.

https://github.com/yellowcub/myMarlinConfig/blob/d8b16303a764c005113de82d7aa24b312a7c47fb/src/Configuration.h#L1287-L1288

As well as `BLTOUCH`

https://github.com/yellowcub/myMarlinConfig/blob/d8b16303a764c005113de82d7aa24b312a7c47fb/src/Configuration.h#L1339-L1342

I also changed the defaults for `NOZZLE_TO_PROBE_OFFSET` to reflect my configuration using a Hero Me v6 cooling system, but everyone's defaults will vary here.

https://github.com/yellowcub/myMarlinConfig/blob/d8b16303a764c005113de82d7aa24b312a7c47fb/src/Configuration.h#L1494

The following lines were changed to slightly increase the ABL probing speed.

https://github.com/yellowcub/myMarlinConfig/blob/d8b16303a764c005113de82d7aa24b312a7c47fb/src/Configuration.h#L1500-L1504

Enabling Z-Axis probing also requires the following line uncommented from a different section.

https://github.com/yellowcub/myMarlinConfig/blob/d8b16303a764c005113de82d7aa24b312a7c47fb/src/Configuration.h#L2100-L2107

### Geometry

I changed the following lines to reflect the limits that my gantry axes could reach before binding with other hardware.  The X-axis zero position is very close to the bed edge, but allowing an additional 10 mm past the bed should allow the ABL to mesh more of the bed accounting for the probe offsets.  The y axis endstop is in front of the bed and requires a negative offset [bed corner is at (0,0)].  The Z-axis was based on my comfort level for the E0 filament extruder and bowden tube meeting the top frame.

https://github.com/yellowcub/myMarlinConfig/blob/d8b16303a764c005113de82d7aa24b312a7c47fb/src/Configuration.h#L1712-L1714

### Auto Bed Leveling (ABL)

ABL was enabled by uncommenting the `AUTO_BED_LEVELING_BILINEAR` among the options.

https://github.com/yellowcub/myMarlinConfig/blob/d8b16303a764c005113de82d7aa24b312a7c47fb/src/Configuration.h#L1882-L1886

And changing the number of mesh points from 3 to 5 per axis.

https://github.com/yellowcub/myMarlinConfig/blob/d8b16303a764c005113de82d7aa24b312a7c47fb/src/Configuration.h#L1969-L1971

I will probably enable the following option in the near future - once I have read more on it.

https://github.com/yellowcub/myMarlinConfig/blob/d8b16303a764c005113de82d7aa24b312a7c47fb/src/Configuration.h#L1976-L1992

### Notes

The BTT SKR mini currently only compatible with the `CR10_STOCKDISPLAY`, which is already selected in the file.  This is the default menu system that is still compatible with the BTT TFT35 touchscreen.  That system requires its own firmware for configuring the touch menu system.

I did not change the default steps per axis within `Configuration.h` as these can be adjusted easy enough in the settings menu.  However, for my records my actual geometry adjustments are: `{ 80.37, 80.26, 400.26, 93 }`.  I already accidentally deleted this once ????.

https://github.com/yellowcub/myMarlinConfig/blob/d8b16303a764c005113de82d7aa24b312a7c47fb/src/Configuration.h#L1168-L1173

For movement, I have gone back to the max feedrate and acceleration values for the original CrealityV422 setup.  There is no reason any of these values for the BTT board should be slower.  You should also make sure the settings in your slicer reflect the values here, or you will get inaccurate time to complete estimates.

## Advanced Configuration

Only a few changes were made to the `Configuration_adv.h` file.

### Babystepping

Uncommented `BABYSTEP_MILLIMETER_UNITS` to define units in mm instead of micro steps.  This also required the changing of `BABYSTEP_MULTIPLICATOR_Z` and `BABYSTEP_MULTIPLICATOR_XY` as these values need to reflect the small sub-millimeter number for each step.

https://github.com/yellowcub/myMarlinConfig/blob/d8b16303a764c005113de82d7aa24b312a7c47fb/src/Configuration_adv.h#L2067-L2083

### Arc support

Disabled ARC support per a recommendation I had seen online that suggested this was commonly used for other tooling, but not really for 3D printing.  If anyone has other recommendations here, I would love to hear them.

https://github.com/yellowcub/myMarlinConfig/blob/d8b16303a764c005113de82d7aa24b312a7c47fb/src/Configuration_adv.h#L2298-L2310

### Firmware Retraction

Enabled firmware retraction by uncommenting `FWRETRACT`.

https://github.com/yellowcub/myMarlinConfig/blob/d8b16303a764c005113de82d7aa24b312a7c47fb/src/Configuration_adv.h#L2518-L2550

