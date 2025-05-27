.. SimpleUI documentation master file, created by
   sphinx-quickstart on Sat May 16 14:23:51 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Application Settings
=================================

Transferring settings via file or QR code
------------------------------------------

The settings must be formatted as a JSON string or a text file in JSON format. Examples of settings are attached.

**IntentScannerMessage** – scanner message name

**IntentScannerVariable** – name of the scanner message variable

**IntentScannerLength** – the name of the variable that stores the length of the barcode if it is passed as a byte array and not a string

**IntentScanner** – scanner operation mode via intent

**CategoryDefault** – filter by message categories for the barcode scanner

**ExchangeFolder** – exchange folder. Selects an exchange folder, creates it if necessary (creating a folder works in Android up to version 11)

**RawConfigurationServiceON** – arbitrary authorization

**GitFormat** – format for storing configurations on github.com

**RawConfigurationURL** – URL for the custom authorization option

**RawConfigurationServiceAuth** – manually set authorization string for custom authorization

**GitCommitsURL** – URL of commits from GitHub

**GitStoreURL** – URL of the GitHub repository to use as a store

**OnlineSplitMode** – "split mode" of configuration and handlers

**onlineURLListener** – URL handlers for split mode

**onlineURL** – Configuration URL for any mode

**onlineUser** – configuration user for any mode

**onlineUserListener** – user of handlers for split mode

**onlineCode** – code for the Mobile Clients directory

**onlinePass** – configuration user password for any mode

**onlinePassListener** – user password for handlers

**backendURL** – PostgREST URL – deprecated. For connecting to Postgre

**backendUser** – Postrgre user

**oDataURL** – OData URL

**Service_URL** – URL of the technical information service (subscription to change settings)

**offSettings** – disable settings

**offChat** – disable chat

**offToDo** – disables the to-do list

**offlineMode** – forced offline mode

**beep** – signal at each scan

**torch** – illumination when scanning with a camera

**dialogOnBackPressed** – ask a question when closing the main program

**gps** – getting GPS coordinates in Variables permanently

**timer** – timer interval

**connection_limit** – maximum connection attempt time for online handlers, 0 – unlimited

**hardwarescan** – disables the camera scanning button for screens with a barcode (for a hardware scanner)

**conf_id** – Configuration ID for requests of the type /get_configuration?confid=...

**configuration** - loading configuration text. You can pass the configuration in the settings (via file), it will be loaded immediately


Transferring configuration to the device in OFF-line mode via direct request to the server
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When working through a web server on the accounting system side, each time the configuration is downloaded, it is saved on the mobile device. In this case, in the settings you can see the date of the last download (last update)

If the work does not require the presence of a web server on the backend side, then the configuration can be transferred to the device by a direct HTTP request. To do this, you need to know the address of the mobile client web service, which can be found in the settings. Next, you need to send a POST request to this address **SetConf** in the body of which is the configuration text (the contents of the UI file) and the configuration will be written to memory, after which you can update or restart the program so that it is applied.



Centralized management of settings and devices via web service
---------------------------------------------------------------------------

Mobile workstations can be managed centrally, address-wise. That is, by AndroidID (unique device ID) you can send new settings to a mobile device or change the configuration. When the application starts, it sends a request service_broadcast/{AndroidID} to the URL specified in the Service_URL setting (if such a setting is not specified, then to the configuration URL). The request transmits the AndroidID and the device model for organizing the device directory database on the central side.

In response, the service can send a JSON of a certain structure to change the settings (it is possible that other commands will be transmitted through this channel later). This can be {"command":"update","uuid":UID,"value":JSON settings}, where UID is an arbitrary UID generated to identify a specific command for the response token. JSON settings - JSON string with settings from the previous section

After receiving the settings, if everything is OK, a request is sent in response /service_response/{AndroidID}/{uuid}

An example of organizing the change of settings is in the old developer kit in the 1C demo database. Document "Changing settings"
