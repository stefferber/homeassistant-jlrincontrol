[![hacs_badge](https://img.shields.io/badge/HACS-Default-orange.svg?style=for-the-badge)](https://github.com/custom-components/hacs)

[![GitHub license](https://img.shields.io/github/license/msp1974/homeassistant-jlrincontrol)](https://github.com/msp1974/homeassistant-jlrincontrol/blob/master/LICENSE)
[![GitHub release](https://img.shields.io/github/release/msp1974/homeassistant-jlrincontrol)](https://GitHub.com/msp1974/homeassistant-jlrincontrol/releases/)

# JLR Home Assistant Integration (v2.0.0)
This repository contains a Home Assistant integration for the Jaguar Landrover InControl system, allowing visibility of key vehicle information and control of enabled services.

Due to changes in Home Assistant, this integration requires a minimum of HA0.110.0.

# Functionality
Currently this loads a series of sensors for
* Vehicle Info
* Status
* Alarm
* Doors
* Windows
* Tyres
* Range
* Location
* EV Battery Sensor (EVs only - deprecated!)
* Battery Sensor (EVs Only)
* Service Info
* Last Trip

And has services for
* Update Health Status (forces update from vehicle)
* Honk/Flash
* Lock/Unlock
* Start Engine/Stop Engine
* Start Charging/Stop Charging
* Reset Alarm
* Start Preconditioning/Stop Preconditioning
* Set Max Charge (Always and One Off)

**Note:** Not all services are available on all models and the error log will show this if not available on your vehicle.

**Note 2**: When calling a service, HA will monitor the status of the service call and report in the error log if it failed.  Debug log will show this checking and the success/failure reason.

Also, due to lack of a fleet of Jaguars and LandRovers/RangeRovers (donations welcome!), there maybe issues with some models not supporting some funtions.  Please raise an issue for these and say what vehcile you have and post the log.

# Sample Images
![](https://raw.githubusercontent.com/msp1974/homeassistant-jlrincontrol/master/docs/panel1.png)


## Configuration
Add via Configuration -> Integrations in the UI

**Required Parameters**
```
  email: <your InControl email address>
  password: <your InControl password>
```
**Config Options**
1. scan_interval - in minutes. Default update interval is 5 minutes.  Use this to change that.  Minimum is 1 minute.
2. pin - set this to be able to use the lock/unlock on the lock sensor.
3. distance_unit - set this to 'mi' or 'km' to override the HA default metric for mileages (mainly for funny UK system of miles and litres!).
4. pressure_unit - set this to 'bar' or 'psi' to override the HA default unit for pressure (mainly for UK also).
5. health_update_interval - see health update section.
6. debug_data: - see debugging below.

### Migrating From Previous Versions
The new config flow will import your settings from configuration.yaml.  It is recommended to remove them after this has happened, otherwise changes via the UI can be reverted by the entries in configuration.yaml.
Required Parameters


# Health Status Update

This integration has the ability to perform a scheduled health status update request from your vehicle.  By default this is disabled.  Adding the entry to your configuration.yaml as above will enable it (after a HA restart).

I do not know the impact on either vehicle battery or JLRs view on running this often, so please use at your own risk.  I would certainly not set it to anything too often.

Alternatively, you can make a more intelligent health update request automation using the service call available in this integration and the output of some sensors.

I.e. on EV vehicles you could only call it if the vehicle is charging, or on all vehicles, only call it during the day and it was more than x period since the last update.

# Installation

**Installing via HACS and configuring via the UI is the recommended method.**

## Manual Code Installation
1. On your server clone the github repository into a suitable directory using the git clone command.<br>
`git clone https://github.com/msp1974/homeassistant-jlrincontrol.git`
2. Copy the jlrincontrol folder to the custom_components directory of your Home Assistant installation.
3. In your configuration.yaml file, add the following entry.


```
jlrincontrol:
  username: <your InControl email address>
  password: <your InControl password>
```

## Branch Versions
As of v1.0.0, the dev branch is now where the very latest version is held.  Please note that there maybe issues with new functions or fixes that have not been fully tested.  It will also be updated regularly as new fixes/functions are developed, so please check you have the latest update before raising an issue.  The master branch is the current release.


# Community Recipes
The [recipes](https://github.com/msp1974/homeassistant-jlrincontrol/blob/master/Recipes.md) page is a collection of ideas contributed by the community to help you get the most out of using this integration.

# Suggestions
I am looking for suggestions to improve this integration and make it useful for many people.  Please raise an issue for any functionality you would like to see.

# Contributors
This integration uses the jlrpy api written by [ardevd](https://github.com/ardevd/jlrpy).  A big thanks for all the work you have done on this.

# Debugging
1. To enable debug logging for this component, add the following to your configuration.yaml
```
logger:
  default: critical
  logs:
    custom_components.jlrincontrol: debug
```

2. To enable logging of the attributes and status data in the debug log, add the following to you configuration.yaml.
```
jlrincontrol:
  username: <your InControl email address>
  password: <your InControl password>
  debug_data: true
```


# Change Log

## v2.0.0
* Deprecated: EV_Battery sensor - this will be removed in furture version
* Added: Setup and options via config flow
* Added: New Battery Sensor for EVs
* Fixed: Deprecation warnings in HA0.110.x

## v1.3.1
* Added: Does not load trip sensor if privacy mode enabled
* Fixed: Integration errors if no position or trip data


## v1.3.0
* Added: New status sensor to show engine running status
* Added: Tyre pressure unit override to cope with UK units system
* Added: New recipes
* Updated: Icon for vehicle info sensor
* Updated: Now available as HACS integration
* Fixed: Error in service descriptions
* Fixed: Tyre pressure units should now show correctly - feedback wanted for your vehicle
* Fixed: Tidy up of attribute values in service sensor


## v1.2.0
* Updated: Improved debug logging messages
* Updated: Handled errors from service calls are now debug instead of warning
* Fixed: Errors in HA v109.0 due to new IO monitoring in event loop
* Fixed: Better handling of multiple concurrent service calls
* Fixed: Scheduled health update now calls 30s after HA start


## v1.1.0
* Added: Last trip sensor
* Fixed: Multiple vehicles on account only showed first one.

## v1.0.0
* First official release - yeah!

## v0.5alpha
* Added: Alarm sensor.
* Added: jlrincontrol services as listed in functionality to control vehicle.
* Added: Ability to schedule health status update - see health status section.
* Added: Ability to set the update interval fro the JLR servers.
* Updated: Use HA units to display data instead of relying on InControl user prefs as not reliable.
* Fixed: **BREAKING CHANGE** Issue with unique ID could cause duplicates.
* Fixed: Renamed last updated to last contact and displayed in local time.
* Fixed: Unlock/Lock on door sensor did not work. Need to add pin to configuration.yaml.  See additional parameters.
* Fixed: Device tracker not updating state.

## v0.4alpha
* Added: Improved debugging info to aid diagnosing differences in models

## v0.3alpha
* Fixed: Range sensor now handles EVs
* Added: New EV Charge sensor to show charge information (not fully tested)

## v0.2alpha
* Updated to use jlrpy 1.3.3
* Added a bunch of new sensor information
* Better handles vehicles that do not present some sensor info

## v0.1alpha
Initial build of the component to read basic sensors

### Known Issues
* Distance to service only shows in KMs.
* Service Info sensor shows ok even if car is needing service or adblue top up.



