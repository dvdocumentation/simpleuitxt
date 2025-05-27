.. SimpleUI documentation master file, created by
   sphinx-quickstart on Sat May 16 14:23:51 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Localization of standard solutions
=================================

It is possible to develop solutions that will include a multilingual interface. Displayed names of processes, screens, any inscriptions and visual elements are subject to automatic translation.

An example for this article can be found at https://github.com/dvdocumentation/simpleui_samples/tree/main/Sample%204%20localization

To work with multiple dictionaries, you need to create and specify in the "Dictionaries" configuration property a JSON string of approximately the following type:

``{"en":{"key 1": "translation 1 en", "key 2": "translation 2 en"},{"ru":{"key 1": "translation 1 ru", "key 2": "translation 2 ru"}}``

That is, dictionaries are listed - en, ru, etc., keys are indicated in the interface, and translations of keys are indicated in the dictionaries

So, in addition to simple screen forms and inscriptions, complex elements specified in variables (for example, tables, card lists, drop-down lists, etc.) are translated. The translation elements (for example, table headers) must be included in the value of such a variable. This is achieved using a template engine. Templates are specified between square brackets. For example, here Barcode and Qty act as keys in dictionaries, and if there is a translation in the dictionaries, they will be translated.

.. code-block:: Python

  table = {
    "type": "table",
    "textsize": "20",

    "columns": [
    {
        "name": "barcode",
        "header": "[Barcode]",
        "weight": "2"
    },
    {
        "name": "name",
        "header": "Name",
        "weight": "2"
    },
      {
        "name": "qty",
        "header": "[Qty]",
        "weight": "1"
    }
    ]
    }

All key translations must be set this way. If there is no translation, just the key will be displayed.

It is also necessary to give the user the opportunity to choose the interface of interest.

There are several commands for this:

**DEVICE_LOCALE** - a constant in hashMap that stores the language specified in the system settings on the device

**setLocale** - a command that sets the user interface language - i.e. tells the application which dictionary from the list of dictionaries should be used when loading. For example, ``hashMap.put("setLocale","en")``

