.. SimpleUI documentation master file, created by
   sphinx-quickstart on Sat May 16 14:23:51 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Working with equipment
================================

Working with barcode scanners via standard settings.
-----------------------------------------------------------------

Working with TSD scanners
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Connect via event subscription (recommended)
""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Manufacturers of mobile devices supply software on their devices that works as a service, which distributes messages (intent) when scanning a barcode. In the mobile device settings, this may be called Intent broadcast or something similar with the word “Intent”. In order for SimpleUI to be able to receive such messages, you should activate “Use subscription to scanner events” in the settings, fill in the fields “Scanner message”, “Scanner variable” and, in some cases, “Value length” (for cases when the barcode is transmitted not as a string but as a byte array and the length of the value is transmitted separately. This information can be gleaned as rules from the documentation or examples.
This connection method is recommended, because unlike various connections via keyboard emulation, this method does not intercept input fields on the screen.

Connection via different types of keyboard emulation
""""""""""""""""""""""""""""""""""""""""""""""""""""""""

By default, if the scanner is configured to output its output via keyboard emulation and if the barcode ends with one of the line separator suffix options (e.g. CR, CR/LF), then such input is perceived as a barcode - i.e. a barcode event will be generated. In case of barcode output settings specifically as generating keystrokes one after another (usually this is called with the word wedge in the settings), the Wedge as keys checkbox should be enabled.

Working with external Bluetooth barcode scanners via settings
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can connect Bluetooth scanners in direct pairing mode (not via keyboard emulation, but via reading scanner events directly). This is a more efficient way than via various types of keyboard emulation.

For such pairing, you need to check the “Use Bluetooth” box in the settings, select the Bluetooth device in the drop-down list and specify the barcode suffix (13 by default)

.. note:: This case can also be implemented independently using the methods described in the section "Working with Bluetooth devices".

Connection to external devices via Bluetooth, Wi-Fi and USB. For all types of devices.
--------------------------------------------------------------------------------------------

Working with Bluetooth devices
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In general, for different categories of Bluetooth devices, the operating scheme consists of:

 1. Reading connected devices to select a device. An optional step if you know the device's mac address. This step can analyze both already paired devices and discover new ones.
 2. Sending data to the device port – in the form of a byte array.
 3. Subscription to data received from the device

Commands for working with Bluetooth are available through importing the SimpleBluetooth java module from python handlers and partially through variable commands.

Methods and commands available from processes to select devices:

**BTGetBounded** – command variable for searching for paired devices. Receives a response in the **BTResult** variable as a JSON array of connected devices.

**BTStartScan** – a variable command for starting scanning of new devices (in addition to those already paired). Upon completion, it returns the result to **BTDiscoverResult** as a JSON array of devices. It also makes sense to use the **BTDiscoverHandlers** command with this command, with a standard array of handlers specified as a parameter. This event will be generated upon completion of the device search.

Connecting to a device in accessory mode (passive device mode, such as a printer)
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Connecting to a device (mac-mac address of the device selected from the list of devices, handlers – a line with handlers in case of failure)

.. code-block:: Python

  from ru.travelfood.simple_ui import SimpleBluetooth as BT

  bt = bt()
  device = bt.get_device(mac) #get device by mac address
  if device==None:
     hashMap.put("toast","Unable to connect")
  else:
     if bt.connect_to_client(device,handlers): #connect to device
                #actions with device                                
                bt.close_socket() #disconnect from device


Connecting to a device in host mode (active mode, such as a scanner)
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

.. code-block:: Python

  from ru.travelfood.simple_ui import SimpleBluetooth as BT

  bt = bt()
  device = bt.get_device(mac)
        if device==None:
            hashMap.put("toast","Unable to connect")
        else:
            if bt.connect_to_server(device):
		#actions with device

Sending data (SimpleBluetooth module methods)
""""""""""""""""""""""""""""""""""""""""""""""""""""

**write_data(<data>,<string-array of handlers>)** Data – data as a string, integer or byte array. Array of handlers – a string with handlers in case of failure

Subscribing to device data, unsubscribing (SimpleBluetooth module methods)
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

**begin_listen_for_data(<string-array of handlers>)** – connects an array of handlers to device events. Handlers must be pythonbytes

**stop_listen()** – disable subscription to device events

Connecting to the device via TCP/IP (send only)
-----------------------------------------------------------

Possible only from a python handler. The work consists of calling the Java function boolean write_socket(String host,int port, String message,String charset) from the SimpleUtilites module:

.. code-block:: Python

  from ru.travelfood.simple_ui import SimpleUtilites as su
  su.write_socket(IP,port,data,handlers)

, where IP,port is the IP address and port (9100 by default) of the printer

data – either a byte array or a string (UTF-8) of data

handlers – string-array of handlers in case of failure

The function returns True if sent successfully and False in all other cases.

Connect to device via USB (send only)
-------------------------------------------------------

The connection is made from the SimpleUSB module. The devices connected at that moment are scanned and the first one from the list or the first printer from the list of connected ones is returned.

Connecting and sending data to any device is done using the command **write_usb(data,handlers)** (data is a string or array of bytes, handlers are handlers in case of failure):

.. code-block:: Python

  from ru.travelfood.simple_ui import SimpleUSB as usbClass  

  usb = usbClass()
  usb.write_usb(data, handlers)

Connection to the printer via USB is carried out using a similar command **print_usb(data,handlers)**

.. code-block:: Python

  from ru.travelfood.simple_ui import SimpleUSB as usbClass  

  usb = usbClass()
  usb.print_usb(data, handlers)


Working with POS printers
-----------------------------

The work of outputting information to the printer consists of two steps:

When working directly with the printer port:

 1. Preparing the layout for printing - this can be a ZPL string, ESC-POS, CPCL or other data
 2. Connecting to a printer (in the case of Bluetooth – scanning and selecting a printer, USB – selecting the first device from the list of connected devices) and sending data to the printer port for printing in the form of a byte array. Then, if necessary, closing the socket.

In case of working with layouts and printing via Android print manager (optional - output to PNG file):

 1. Preparing an HTML layout of the document and calling the PrintPreview command
 2. *Optional*. In case of output in PNG – this stage can be continued – the image is embedded in the data transmitted to the printer port, for example in zpl and then the process will continue as in case of connection directly to the printer port

In case of working directly with the printer port (socket), the previously described methods of connecting via Bluetooth (as to an accessory), to a TCP socket and to a USB device (the print_usb function) are used. The data can be represented by a string or a byte array. The methods for preparing this layout are described below.

Preparing data for the printer offline using layouts
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In general, any printed forms (documents, invoices, labels) can be prepared offline as html layouts and filled in with a template engine in python. Further printing is carried out through the Android print manager.

This method with a demo example is described in the article `<https://infostart.ru/1c/articles/1716745/>`_

Output layout data to png
""""""""""""""""""""""""""""

In addition to preparing the layout as html, you can also save this html document as a png file for further printing without the print manager. This is done using the PrintToBitmap variable command together with PrintPreview. This command overrides the print button (which by default calls the print manager) so that the image is saved to a file (it is placed in PrintBitmapPath) and the event handler with listener=PrintBitmap is called. That is, by intercepting this handler, you can get the image from the file and continue working with it.

Work with a ready-made PDF or prepare online
""""""""""""""""""""""""""""""""""""""""""""""

If you need to print or preview a PDF file, you can use the PrintPDF and ViewPDF variable commands by passing the path to the PDF file as a parameter.
You can also prepare PDF on the web service side, described here `<https://uitxt.readthedocs.io/ru/latest/common_functions.html?highlight=pdf#id28>`_

Preparing data for the printer using ZPL, ESC POS, etc.
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
The printer expects a byte array that can be an encoded string, which can be either just text, or a page layout or control commands in a special markup language.
Any markup languages ​​can be used to mark up a page. You can prepare such markups outside the application (for example, use an online markup constructor, and then change the field values ​​in it by replacing them in the text). You can generate the code yourself in the handler code. There are also a number of libraries that generate markup based on the source data in a form convenient for the developer. The application package includes the zpl (`<https://pypi.org/project/zpl/>`_) and escpos-gen (`<https://pypi.org/project/escposgen/>`_) libraries.

Enhanced interaction with some device SDKs
---------------------------------------------------------

For devices from some manufacturers, SimpleUI integrates support for the Android SDK of these manufacturers, which provides additional functions. For example, for a TSD, this may be, in addition to simply scanning a barcode, the ability to turn the scanner on and off programmatically, block the scanner, and receive additional information in the application. Not all manufacturers implement such an SDK

TSD Urovo
~~~~~~~~~~~~~~

Functions for Urovo are implemented in the SimpleUtilites module (from ru.travelfood.simple_ui import SimpleUtilites)

**urovo_set_lock_trigger(<status>)** lock/unlock scanner (if locked, then the scan button is locked). Status – true (locked), false (unlocked)

** urovo_open_scanner()** - open the scanner object (does not start scanning, but simply initializes the scanner and checks availability). This function must precede urovo_start_decode

**urovo_start_decode()** turns the scanner into scanning mode

**urovo_stop_decode()** turns off scanning mode

**urovo_close_scanner()** terminates the scanner

**urovo_get_scanner_state()** returns the current state of the scanner

**urovo_get_lock_trigger_state()** returns the current scanner lock state

TSD Meferi
~~~~~~~~~~~~~~

The Meferi TSD has its own Android SDK that allows you to manage the scanner state and the TSD settings in general. The SDK itself is included in SimpleUI and is given to the developer in the form of objects of the class

.. code-block:: Python

  from ru.travelfood.simple_ui import Meferi as Meferi
  
  #getting an object of the ScanManager class
  manager = Meferi().getScanManager()
  #further work with the object, utilization of methods
  manager.setScanEnable(True)
  manager.keyScan(True)



