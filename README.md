
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

## Firmware structure ##

The firmware should be in .zip file format, prepared using [nRF Util](https://github.com/NordicSemiconductor/pc-nrfutil). 

 - *manifest.json*: Must have at least one manifest file
 
 - *application.bin*: Must have this file name, otherwise it get error during update process 

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


## Testing to Production Flow ##

The typical flow to introduce a new firmware is as follows:

1. Update this repository with the firmware zip file & json file.
Ensure the json file includes the firmware with `"status": "beta"` and `"priority": "optional"`

       {
           "version": "7.8.9",
           "firmwarePath": "/hopper/dfu_application-7.8.9.zip",
           "status": "beta",
           "priority": "optional"
       }

2. Commit the changes to the repository.
3. Test the latest firmware in the app.
The app will need to be in 'developer mode' 
Firmware updates will need to be through the 'update firmware' screens.
3. Assuming everything worked well, change the json file to reflect `"status": "production"`

       {
           "version": "7.8.9",
           "firmwarePath": "/hopper/dfu_application-7.8.9.zip",
           "status": "production",
           "priority": "recommended"
       }

4. Commit the changes to the repository.
5. Test the app with additional users. 
This will be available to all users, 
but people must go to the 'Update Firmware' 
section of the app to get the update. 
6. Assuming everything worked well, change the json file to reflect `"priority": "recommended"`
7. Commit the changes to the repository.
This will now show as a recommended firmware update when people connect to the drone.

