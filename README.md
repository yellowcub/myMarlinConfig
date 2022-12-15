# My Marlin Configuration Files

This repository contains the configuration files I have used to build firmware for my Ender 3 Pro with a BTT SKR E3 mini v3.0 controller board.  Please exercise caution, and do not use these configuration files for your own build unless you are confident your configuration matches, or at least know how to modify the `Configuration.h` and `Configuration_adv.h` files to reflect any differences.

These configuration files are based on the [bugfix 2.1.x repository](https://github.com/yellowcub/Configurations), and the original versions for comparison can be found in the fork by navigating to the Creality Ender 3 Pro examples.

These configuration files are versioned, and will only work with this fork of the [Marlin Firmware](https://github.com/yellowcub/Marlin) code.

https://github.com/yellowcub/Configurations/blob/import-2.1.x/config/examples/Creality/Ender-3%20Pro/BigTreeTech%20SKR%20Mini%20E3%203.0/Configuration.h#L40

## Configuration

Changes to the `Configurations.h` file consist of:

  - Commenting error warnings
  - Updating the author and machine names
  - Changing the extrudor heater maximum temperature
  - Enabling Auto Bed Leveling via the BLTouch

### Error Warnings
The first change is to comment out the initial error warnings not to build with `import-2.1.x`.  As noted above, this was built with `bugfix-2.1.x`.

```
//#error "Don't build with import-2.1.x configurations!"
//#error "Use the 'bugfix...' or 'release...' configurations matching your Marlin version."
```
### Author and Machine Name
The author and machine settings are optional for keeping track of personal firmware builds.

### Extrudor Maximum Temperature
The maximum temperature for extrudor 0 was changed to 315.  This was intended to support the capability of the MicroSwiss all metal hot end upgrade, which should allow requesting temperatures up to 300&deg; C (This accounts for a 15&deg; C overtemp specified further down in the configuration).  In actuality the controller would only permit a maximum temperature of 275&deg; C because the stock thermistor does not permit higher temperatures - for good reasons!  I should be able to increase this if I upgrade the thermistor and select the appropriate thermistor code.

### Auto Bed Leveling
