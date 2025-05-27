.. SimpleUI documentation master file, created by
   sphinx-quickstart on Sat May 16 14:23:51 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Python in handlers
=======================

Simple UI applications can use the Python interpreter to execute handler code in any mode. You can write handler code using procedures, classes, libraries. The list of libraries and the current version of the interpreter may change from release to release and information about this will be updated.

Currently in use:

 * Python version - 3.11 (for building with OpenCV+PyTorch – version 3.8)

Connected libraries:
 
 * Standard libraries
 * Requests
 * Lxml
 * pony
 * Pillow
 * Jinja2
 * python-barcode
 * pymongo
 * qrcode
 * tinydb
 * tinyrecord
 * zpl
 * escpos-gen
 * pelicandbms
 * python-dateutil

Since GooglePlay does not allow publishing packages larger than 100 MB, periodically, as separate apk builds, a build is published with:

 * Pandas
 * BeautifulSoup
 * OpenCV
 * Pytorch
 * torchvision

The interaction of Python handlers with the platform and interface occurs through a stack of variables (hashMap object), slices, and also objects directly - Datasets, DBMS, files, direct calls to platform functions

hashMap (variable stack) is an object through which variables and so-called variable commands (ShowScreen, FinishProcess, etc.) are passed. This is not a Python dictionary, as one might think, but a Java object HashMap<String, String>. It is accessed through the specified methods:
 * put(key,value) – put a variable
 * get(key) – get variable
 * remove(key) – remove variable
 * containsKey(key) – check if the collection contains such a key

Moreover, from the point of view of execution, the stack itself (and the commands in it) are analyzed at the end of the handler, that is, the handler is interpreted (with all the put, remove), then the system reads the keys in the stack and executes the commands.

There are several types of python handlers with different purposes.

Pythonscript handler type
----------------------------------

The simplest handler. It is executed as excec(<handler code>). It is not stored anywhere, in fact, it is a string launched for interpretation. This is the reason why variables are not stored between calls of this handler either. It is similar to JavaScript. It could be compared to running scripts in different terminals.

The distinctive feature of this handler is that the script itself in base64 format is stored in the array of handlers in method and is edited directly in the designer window. This simplifies working with small scripts and better structures the architecture of handlers. All examples are written using this type of handlers.

.. note:: Simple strives for simplicity and minimal code, so instead of piling up modules, it is recommended to limit yourself to small scripts of 10-15 lines per event

The handler is entered by:
 * hashMap – stack of variables
 * process_slice – process slice (for reading)
 * screen_slice – screen slice (for reading)

Also connected:

1) The **get_body** function allows you to cut code from the current handler in a simplified mode to run handlers (for example, RunEvent or for download)

Example

.. code-block:: Python

 def after_download_1():
     with open(hashMap.get("DownloadedFile"), encoding='utf-8-sig') as f:
         toast("Downloaded file: "+hashMap.get("DownloadedFile"))
         hashMap.put("RestartScreen","")
	
 postExecute = json_to_str([{"action": "run", "type": "pythonscript","method":get_body(after_download_1) }])
 su.download(download_url,None,None,'goods.txt',postExecute)

2) functions **json_to_str** and **str_to_json** wrappers for json.dumps(j,ensure_ascii=False, indent=4) and json.loads(s)

3) functions for working with datasets:
 
 * CreateDataSet(name,options=None)
 * GetDataSet(name)
 * DeleteDataSet(name)
 
4) GetDataSetData function

5) SendBus function for sending to the bus

Modules:
 * **android** – system functions such as toast, beep, speak, etc.
 * **json**
 * **base64**

Variables:

 * **_local** – key-value DBMS (see Storage section)
 * **_data_dir** , **_downloads_dir** – configuration folders for internal storage
 
General module general
~~~~~~~~~~~~~~~~~~~~~~~~

The general module is also available for editing in the designer (in the Configuration section). Its behavior in the system is somewhat different. It is saved to a file when loading the configuration and is available via import general. This module can store common functions and variables.

python handler type
-------------------------

Unlike pytonscript, these handlers are saved to the handlers_conf.py file when loaded into the configuration, and then the functions are called from this module. That is, in the method array of handlers, it is not the text of the script itself that is stored, but the name of the function that is launched from the module.
Handlers of this type (as well as pythonargs and pythonbytes) are connected to the configuration as files. There must be at least a handler file (these are calls to functions declared in the handler array), it is written to the configuration in PyHandlers. Plus there can be one or more plug-ins. They are written to the PyFiles array of the configuration

.. code-block:: JSON

 "PyFiles": [
            {
                "PyFileKey": "ext",
                "PyFileData": "base64 file contents"
            }
        ]

All files are saved to files in the internal folder when loading the configuration. Additional files can be imported by key (they are saved to the same folder with the name <key>.py

In user mode, all this saving happens through the constructor. This can be done on the Module Files tab:

 * or via regular file loading (Load main py file, Add additional py file). With this method, you will have to do this action every time you change the python files
 * or by connecting GitHub (instructions on the designer page). Both open and closed repositories are available. This method is good because you can also connect your IDE to the repository and commits will be automatically saved from the IDE and when saving the configuration. By the way, the configuration itself (ui) can also be saved on GitHub (User tab). This method is more convenient.

**When should you use python and when pythonscript?** The answer is that python should be used in large projects with more than one python module. For simple projects, pythonscript is still preferable.

Functions of this type must have a certain format:

.. code-block:: Python

 def foo(hashMap,_files=None,_data=None):
     hashMap.put("toast",hashMap.get("barcode")) #example of writing to and reading from stack
     return hashMap

 **_data** – for SUIP files, data packed into the file

 **_files** – deprecated, but must be specified for compatibility

Functions with arbitrary number of arguments pythonargs
-----------------------------------------------------
picture

You can use functions with an arbitrary number of unnamed arguments (usually called args). To do this, select **pythonargs** in the handler settings.

An example of the code for such a handler:

.. code-block:: Python

  def foo(hashMap,*args):
   param1 = args[0]
   param2 = args[1]
   param3 = args[2]

   return hashMap

Function for working with OpenCV pythonbytes
--------------------------------------------

When capturing an image for further analysis in OpenCV, the image is passed to the handler as a byte array. The handlers have the following format:

.. code-block:: Python
 
 def detect_face(hashMap,imageBytes):
        np_data = np.asarray(imageBytes,np.uint8)
        img = cv2.imdecode(np_data,cv2.IMREAD_UNCHANGED)
   #Here is work with the image
        return hashMap

Platform functions that work run-time (android module)
----------------------------------------------------------------------

All functions are accessible via variable commands, but this is often not very convenient. For example, you need to display a toast inside a long handler or update a notification inside a cycle, and not after the handler tick is executed (and the stack machine executes variable commands at the end of the tick). This is exactly run-time at the system level, i.e. it is executed directly at the moment of the call. All such functions are collected in a module in python - android module and they can be used like this:

.. code-block:: Python

 import android
 android.toast("hello")

Interface commands:

 * toast(String toast) – display Android message
 * speak(String text) – speak text (TTS engine)
 * listen() – start waiting for speech recognition
 * vibrate() and vibrate(int duration) – vibration and vibration of a given duration
 * beep()/beep(int tone)/ beep(int tone,int beep_duration,int beep_volume) – sound signal, including the ability to select tone (from 1 to 99), duration and volume (default – 100)
 * notification(String message)/ notification (String message,String title)/ notification(String message,String title,int number) – notification in the notification shade. Number – notification identifier, by which it can be accessed later to either remove or rewrite (update)
 * notification_progress(String message,String title,int number,int progress) – notification with a progress bar (from 0 to 100) notification_cancel(int number) – hide notification

Control commands:

 * refresh_screen() triggers a screen refresh. It is assumed that the stack will be set by the runtime stack methods.
 * refresh_screen(hashMap) - starts refresh and passes the stack.
 * RunEvent(String handlers) – run an array of handlers
 * BackgroundCommand(command) – run a background command
 * stop() or stop(hashMap) – breakpoint for debugging

Working with the stack:

 * get_process_hashmap() – gets the screen variable stack from any location
 * get_cv_hashmap() – gets the ActiveCV variable stack from any location
 * get_service_hashmap() – Gets the background service hashmap stack from anywhere
 * process_started() – gets an indication whether the process is currently running
 * cv_started() – gets the flag whether ActiveCV is currently running
 * put_process_hashMap(key,value) pushes the value onto the process stack
 * remove_process_hashMap(key) – removes a value from the process stack

Additional Java modules accessible from Python handlers
--------------------------------------------------------------------

In general, it would be more correct to say that any Java classes are available from Python Handlers. But some of them have practical value.

ImportUtils class (from ru.travelfood.simple_ui import ImportUtils as iu):

 * getView(String tag) – get the Java widget object by its variable (and then do whatever you want with its methods)
 * getContext() – get context (context may be needed in other SDK functions)
 * getRootLayout() – get the root container of the screen

Module SimpleUtilites(from ru.travelfood.simple_ui import SimpleUtilites as su):

 * get_stored_file(String key) – get absolute path to media file by key
 * download(String url,String user,String password,String filename,String postExecute) – start downloading in a worker with a progress bar (example above). Upon completion, the postExecute handler is executed
 * deleteCache() – deletes the application cache
