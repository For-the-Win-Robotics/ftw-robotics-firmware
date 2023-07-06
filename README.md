
# ftw-robotics-firmware

This is to house the latest firmware for the FTW Robotics Hopper drone and controller

The main file is the /firmwares.json file.  This contains the information for which firmwares are available to download.

## Definitions ##
***version***: The firmware version number

***firmwarePath***: The relative path of where to download the firmware for a specific version

***status***: Describes who the firmware is available to

- *beta*: Only available to developers and testers
- *production*: Available for general public

***priority***: Denotes how much or little the firmware should be forced on the user.

 - *optional*: Optional firmware can be updated, but it is not necessary to update
 - *recommended*: Recommended firmware prompts the user to update the firmware, but it is not required.
 - *critical*: Critical firmware forces the user to update the firmware.


## Example Code ##

		{
		    "hopperFirmwares": [
	        {
	          "version": "0.0.1",
	          "firmwarePath": "/hopper/dfu_application-0.0.1.zip",
	          "status": "beta",
	          "priority": "optional"
	        },
	        {
	          "version": "0.0.2",
	          "firmwarePath": "/hopper/dfu_application-0.0.2.zip",
	          "status": "beta",
	          "priority": "recommended"
	        },
	        {
	          "version": "0.0.3",
	          "firmwarePath": "/hopper/dfu_application-0.0.3.zip",
	          "status": "beta",
	          "priority": "optional"
	        },
	        {
	          "version": "0.0.4",
	          "firmwarePath": "/hopper/dfu_application-0.0.4.zip",
	          "status": "production",
	          "priority": "critical"
	        }
	    ],
	    "controllerFirmwares": [
	        {
	          "version": "0.0.1",
	          "firmwarePath": "/controller/dfu_application-0.0.1.zip",
	          "status": "production",
	          "priority": "critical"
	        }
	    ]
	}