Simple UI Version History
==========================

Version 11.75.25 from 10/03/2023
------------------------------
* python-SimpleBase integration
* Native SimpleBase integration
* Working with files
* html2image
* manual work with dialogue
* manual control of timers
* manual control of recognition settings
* improved Multiscanner
* other improvements and modifications


Version 11.70.00 from 09/12/2023
------------------------------
* work with equipment via USB/BT/WIFI
* functions for direct printing on POS printers

Version 11.60.30 from 06/20/2023
------------------------------
* working with WebSocket
* command-variable ErrorMessage

Version 11.42.00 from 04/05/2023
------------------------------
* runprogress handler
* Notification with progress bar
* Biometrics check
* PIN code verification dialog
* Floating buttons
* Simplified autofill support
* Object field
* BackScreen
* ShowProcessResult/FinishProcessResult, SetResultListener
* parent_screen,current_process_name,current_screen_name
* SetRed, SetGreen
* disable_events
* Redefining search in toolbar
* Opens the vector editor in edit mode for the specified file
* Support for functions with arbitrary number of arguments
* Pymongo support

Version 11.25.00 from 03/15/2023
------------------------------
* Background tasks with WorkManager and workers


Version 11.00.30 from 02/13/2023
------------------------------
* **SQLQueryMany** - for very large selections (close to a million rows and above). Writes to a file and provides a link to the temporary file.
* **RefreshMenu** - similar to UpdateMenu, but only rereads the menu without rereading the configuration, handlers at startup. Faster
* **UpdateConfigurations** - updates configurations (similar to pressing the corresponding button through the main menu)
* **InstallConfiguration** - pins the current configuration to the repository (analogous to the pin button in the toolbar)
* changes in vector graphics: line added, rectangle editing changed, menu in editor redrawn
* added debugging for new type handlers (via "_")
* console in the editor for working with several DBMS


Version 11.00.05 from 02/03/2023
------------------------------
* New handler architecture, common event management and asynchronous execution
* New model of working with SQL
* New model of working with HTTP
* Commands SetSettingsJSON, RunEvent, BreakHandlers, RefreshScreen, StartCaptureCamera, StartMediaGallery
* Change configurator

Version 10.35.00 from 11/19/2022
------------------------------
* Improvements to customcards, customtable
* Manage screen visibility and refresh
* New PostLaunch Event
* New container properties: Variable, Background Color, Padding and Stroke Weight
* Manipulate elements from Java, Python
* Control for Urovo TSD


Version 10.00.00 from 10/4/2022
------------------------------
* Vector graphics

Version 9.75.00 from 08/24/2022
------------------------------
* Printed forms, reports, HTML
* Handlers in HTML
* Expanding the capabilities of the web service


Version 9.50.00 from 06/22/2022
------------------------------
* Active list elements: buttons, menus, checkboxes
* Remembering the position in the list
* Search in the list
* Groups in the list
* Transfer data to the SimpleUI web server (POST request)
* Header redefinition
* Solving the problem with screen flipping



Version 9.42.00 from 05/10/2022
------------------------------
* New approach to NoSQL - direct calls from Python

Version 9.35.00 from 04/28/2022
------------------------------
* Compatibility with new development environment features: automated debugging and localization
* **Fully custom lists** based on container markup
* Tools for localizing ready-made solutions for different languages
* Modern input fields - customizable fields with animation, etc.
* Buttons with progress bar - automatic launch of asynchronous long processes in dedicated threads
* Progressbar blocking long process
* New tile definition - similar to new custom lists
* Specify the default dropdown item


Version 9.25.00 from 04/19/2022
------------------------------
* Compatibility with the new configuration format and the new development environment

Version 9.00.01 from 02/01/2022
------------------------------
* **update separate configuration mode and handlers**
* **using multiple configurations, standalone configurations**
* **main menu override**
* screen menu
* publishing configurations via GitHub or any service
* control of connection quality and ping
 

Version 7.95.00 from 12/25/2021
------------------------------
* **Output of images directly from files** in all visual elements - tiles, cards, images on the form, etc.
* **Slider Gallery**
* Awesome font for icons
* Working with files online
* Database backup commands
* Logging and dump commands (application log)
* DB_PATH - variable path to SQL DBMS
* _files - dictionary to access the list of files


Version 7.75.10 from 11/29/2021
------------------------------
* Fixed: When clicking multiple times to delete one photo, the counter increases
* Fixed: delete link remains on screen if you don't exit the process


Version 7.75.05 from 11/27/2021
------------------------------
* Fixed: Photos in gallery remained from previous screen if you do not exit the process


Version 7.75.00 from 11/24/2021
------------------------------

* **Added support for working with DBMS via ORM** Now it has become much easier to work with SQL via Python. For this, PonyORM is used, which you can read about here `docs.ponyorm.org <https://docs.ponyorm.org/firststeps.html>`_ . In short, you can forget about SQL when working with SQLite and work with tables via objects. Examples in the demo database "Examples with ORM"
* Added platform version history. News and changes on the platform can now be read in the online documentation or in the application via the main menu "Version History"

Version 7.55.05 from 2.11.2021
------------------------------

* **dynamic change of screen structure**
* **change configuration on the fly**
* **".suip process files" - a new storage and exchange format**
* new useful application settings

Version 7.10.00 from 09/11/2021
------------------------------

* **new JSON configuration format** - now the configuration is stored as a JSON string - easier to read, work with and opens up new possibilities.
* **working with multimedia via galleries** - so far only photos are available, but the gallery greatly simplifies working with photos on the screen. In fact, it automates the process completely, providing flexible management options.
* **support for working with files in their pure form.** Added **mm_local** mode - photos (or some other file) are saved on the disk, the platform works with them directly as with files, and the developer operates only with a link
* **common Python module**. Now you can put your methods and classes in a common module
* commands for setting photo resolution and compression level. For example, hashMap.put("mm_compression","70"), hashMap.put("mm_size","65")
* dates in containers. Date input fields can be added.
* additional mode for registering all keyboard events
* keyboard reading in menu - you can set up hotkeys for the main menu.
* close screen without dialog. You can disable confirmation at screen level
* html in Active CV headers
* new way to work with drop-down lists
* focus on any input element
* blue list for ActiveCV
* dequeue multiple objects
* added Python debugging tools


Earlier versions
----------------------

Earlier version history is not present in this document, but I try to describe all significant service packs in my profile on Infostart: `Dmitry Vorontsov on Infostart <https://infostart.ru/profile/129563/>`_

