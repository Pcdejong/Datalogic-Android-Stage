# Datalogic-Android-Stage
Batch file to Stage a Datalogic Android terminal over USB Debugging connection.

Disclaimer: This file is an un-official tool and is not officially supported by Datalogic.




# Datalogic Android Stage tool

The purpose of this file is to easily stage Datalogic terminals with firmware/configuration/apk's etc with an usb cable.

# Requirements
Turn on USB debugging on your device. You can use Scan2Deploy (from Google home screen) for this using the enclosed pdf or do this manually.

# Warning
Please be aware that this script will remove all spaces in your config and apk files! This is needed for a correct installation. (no longer applies on version 4.9 or higher)

# Firmware update (1.0)
To update the firmware off your device simply place the zip file in the same folder as the bat file and run the bat file.
The device will perform a check if the firmware is the same as on the device. If the firmware is the same the update will not proceed.
By default the file will perform a factory reset after the update. If you don't want to reset change the RESET parameter inside the bat file to FALSE

A check is build in to see if the battery is charged sufficiently to perform the update.

When a firmware update is performed the process is started. You have to run the file again for addional configuration.

# Fixed firmware folder (3.9)
With setting this parameter to TRUE:
		set usefixedfwfolder=FALSE
	You can set a fixed folder where you can put all your Datalogic firmware in one folder. 
	Define the path of this folder with this parameter:
		set fixedfwfolder="C:\Datalogic\LatestFirmware"
	You can use different folders in this folder. So no problem to use separate folders for separate terminals. 
	When multiple firmware are found for the same terminal the batch file will select the firmware with the highest version number.
	

# DXU Config files (1.0)
You can also add a dxu configuration file in the folder. This will be applied to the device.

# Apk files (1.0)
To install your application(s) you can add one or multiple apk files in the folder.

# Multiple Devices (1.8)
You can stage/firmware update multiple devices at once. Just connect more than 1 device with your pc with usb debugging on. Great for speedy firmware updates!

# Log file (2.0)
By default the logging function is enabled. This will create a log file with date/time and serialnumber and the process are performed.
To disable the log file change the LOG parameter inside the bat file to FALSE

# Autocopy files (2.7)
With the "set autocopy=*.jpg;*.jpeg;*.bmp" parameter the file copies every file with this extension to the sdcard of the terminal.
You can change the parameter offcourse to copy your file to the sdcard if you want.

# Folders (3.3)
Optional you can use the Firmware, Config and APK folder to store your files instead of putting everything in the root. 
The config files can be used for dxu config files but also for Visual formatter, Scan2Deploy, proconfig files etc.

# Handscanner (3.1)
If you have handscanner files (*.proconfig) in your root (or config folder) they will be automaticly copied and installed. Make sure you install the Insights application

# Visual Formatter (3.2)
If you have a seperate Visual Formatter file it will be automaticly copied and installed. Excellent way of testing your VF projects!

# Velocity config files (3.4)
Velocity config files that are found in the root (or config folder) will be automaticly copied to the right folder of the device.

# Xtralogic config files (3.4)
Xtralogic config file (xtralogic-config.xml) that is found in the root (or config folder) will be automaticly copied to the right folder of the device. For more info and an example file see the MDM section in the documenation: https://www.xtralogic.com/remote-desktop-client-documentation/

# Scan2Deploy Studio files (4.1)
With Scan2Deploy Studio 1.5 it is now possible to store files locally on your device. With this file you can now
copy and install these projects over usb. For instance in pre-sales projects where a Scan2Deploy studio server is
not yet installed. If a tar file is found it will be automaticly copied. A payload will be asked to enter. 
Choose the Copy Data > Copy encrypted Data. This data will be stored in a text file with the same name as the tar file
If you re-create the tar file make sure you delete the text file or renew the data with the payload from Scan2Deploy Studio.

# Surelock/Surefox settings file (4.3)
Automatic copy and import of Surelock and Surefox Settings file into the device. (Removed in 4.9 and reverted to the autoimport function)

# Fixed ip adress (4.4)
With this function it is possible to configure a fixed ip-address (beta function)
You need to install DLintents apk first to use this function. Please contact support if you don't have this application.
Fixedip.txt is needed in the config folder (if exists) or in the same folder as the stage file. The file has to contain:

    SSID=_setyourssidhere_
    subnet=255.255.255.0
    gateway=10.0.0.10
    dns1=8.8.8.8
    dns2=8.8.8.4

Configure your SSID, Subnet, gateway and DNS to your likings.
You can either preconfigure your fixed ip-adresses on serialnumber like this.
    G20L44748=10.0.0.223
If the serialnumber is not known the batch file will prompt you for the serialnumber. It also saves the serialnumber and ipadress to the fixedip.txt file 
so the second time you use this file with the same terminal you don't have to enter the serialnumber again.

# Espresso packages (4.5)
If you want to install Espresso packages please create a folder called Espresso and put the espresso firmware in there. Please be aware that no check on compatibility is performed on this file so make sure you put a correct espresso package in there. 

Since it is not possible (yet?) to see which Espresso package is installed on the device a logfile named espresso.txt is created to log the serialnumbers that already have been processed. 

# Debug mode (4.6)
If you want to debug the batch file in case of problems. Alter the batch file setting set Debug=FALSE to set Debug=TRUE
A log file with the serialnumber of your device is created with the output of the batch file. Please be aware that if you set the debug mode to true the output is not longer shown in the batch file itself and the multiple devices function will no longer work (only on the first device)

# Optional things
Sometimes you have additional wishes. Below you can find three examples if for example you want to copy some files also to the device.
You can copy these examples just above the :exit part in the bat file

    :Copy
    ::Copy files
    adb -s %device% push files.ext /%sd%/

    :Run
    :: Run Application (like Surelock)
    adb -s %device% shell am start -a android.intent.action.MAIN -n com.gears42.surelock/.ClearDefaultsActivity

    :Log
    ::Turn on logcat
    adb -s %device% logcat -c
    adb -s %device% logcat -G 16M
    adb -s %device% logcat -g

# Bugs and enhanchements
If you encounter bugs or something is not working. Just send me an email: peter.dejong@datalogic.com 
If you have wishes for new enhancements also do not hesitate to contact me.
	

# Changelog:

    Version 1.0 Initial setup.
    Version 1.1 Updated current directory. Build in check for multiple zip files.
    Version 1.2 Default use of Datalogic ADB. Support for 32 and 64 bit. Remove spaces from DXU files
    Version 1.3 Added firmware support for Memor 10
    Version 1.4 Added reset support
    Version 1.5 FW support for Memor 1 Gun
    Version 1.6 Added support for incremental updates
    Version 1.7 Added check for unauthorized devices. Grant permissions on installation of APK files. Added support for check on build version
    Version 1.8 Support to stage multiple devices at once. (make sure they are all authorized) Added aditional Options at the bottom. (Copy/Start/Log)
    Version 1.9 Fix for unauthorized devices. Clean up code.
    Version 2.0 Implemented logging
    Version 2.1 Added support for incremental firmware check on Memor 10. Added a check on battery level > 20% for firmware updates.
    Version 2.2 Bugfix
    Version 2.3 Bugfix on multiple devices. Created a readme.txt for a better documentation. Rename to Datalogic Android Stage tool (DAS)
    Version 2.4 Better handling on zip files.
    Version 2.5 Initial support for Memor 20.
    Version 2.6 Automatic reboot after firmware update for Android 9 and higher.
    Version 2.7 Added check on installation on Datalogic Android Drivers. Added a autocopy function for jpg,jpeg and settings folder to SDCARD
    Version 2.8 Added support for Memor K
    Version 2.9 Fixed a bug finding zip files
    Version 3.0 Fixed a bug supporting AM/PM timezones
    Version 3.1 Added support for Handscanner/SkorpioX5
    Version 3.2 Added support for Visual Formatter files. 
    Version 3.3 Added support for separate folders for firmware/apk and config files
    Version 3.4 Added support for Velocity
    Version 3.5 Added support for Memor 20 Wifi models (and build in check for loading correct version)
    Version 3.6 Added support for loading correct firmware version (ROW/AOSP/US) on Memor 10
    Version 3.7 Build in check for applying correct firmware version type for JTA6, Memor 1, 10, 20, K and SkorpioX5
    Version 3.8 Changes on firmware selection
    Version 3.9 Initial release on fixed firmware folder
    Version 4.0 Implementation of fixed firmware folder with selecting the hightest firmware
    Version 4.1 Implementation of deploying local Scan2Deploy files
    Version 4.2 Fix for offline devices (ADB).
    Version 4.3 Implement direct import of Surelock and Surefox settings files (instead of autoimport)
    Version 4.4 Implement possibility to set fixed ip adresses with the DLintentSDK + Fix for autoreboot
    Version 4.5 Build in support for Espresso packages
    Version 4.6 Created a debug mode for troubleshooting purposes
    Version 4.7 Fixed Surefox import. Fixed Memor K firmware update.
    Version 4.8 Removed renaming of spaces in dxu/apk and firmware files
    Version 4.9 Added Xtralogic support (RDP application) and introduced SkorpioX5 sideload method. Removed direct import for Surelock and Surefox settings (4.3)
