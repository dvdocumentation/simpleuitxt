.. SimpleUI documentation master file, created by
   sphinx-quickstart on Sat May 16 14:23:51 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Vector graphics
========================


.. image::_static/vector.PNG
       :scale: 35%
       :align:center


Starting with version 10.00, the ability to use vector graphic objects in screens, drawn in a vector editor or generated from code, manage them from handlers (online and offline), receive events has been added. The general idea is the ability to place graphic fields in containers, which contain a certain drawing (scheme), consisting of objects that can be accessed from code by tags (tags are set in the editor), which interact with the user (the user can, for example, click) - i.e. objects are not only displayed but also generate events that can be processed in the handler.

In general, working with graphics begins with a vector editor built into the application, where a sug file is created (essentially JSON with objects placed on the diagram). Then this file can be attached to the configuration via Media files in the designer, a link to the diagram is placed in the container and then you can work with it from handlers. But this option is optional - the file may not exist. A warehouse diagram, for example, can be generated automatically. There is also the so-called "edit mode" directly in the "screens", where you can work with the host in user mode.

You can see the general principles of work in the video: `Vector graphics in Simple UI <https://youtu.be/cJ2_QtHgZ7c>`_

It is also recommended to look at the examples in the `Examples <https://github.com/dvdocumentation/simpleui_samples/tree/main/vector%20assets>`_ configuration

And an article with an overview of the mode: `Infostart: Vector graphics in Simple UI <https://infostart.ru/public/1736200/>`_

Below is reference information on the commands used in various modes.

Important principles for working with vector graphics:

* The container element is called "Map Field"
* The screen must be **with disabled scrolling of the root container**, or rigidly define the dimensions of the map. You cannot enter this Map Field element if it has no width or height.
* Vector editor saves maps to the "Downloads" folder (may change in other releases)
* You can specify the map name explicitly (by the media file key), or through a variable

Working with cells and storage schemes
--------------------------------------

**map_highlight_cells** – setting the color of cells by addresses, parameter - JSON array of colors and cells, for example ``[{"color":"#f56042","cells":["1-1","1-2"]},{"color":"#2a60b8","cells":["2-4"]}]``
**map_add_move** – setting a route (step) from cell to cell. Example ``[{"from":"1-2","to":"2-2"},{"from":"2-2","to":"3-5]``
**map_nav_mode** - enables navigation mode using algorithms
**map_set_size** - sets the relative dimensions of the canvas. Example parameter ``{"width":1000,"height":2000}``
**map_add_rows** – adds a new row of cells with parameters x,y – upper left corner, number of rows and rows, and cell size. Cell addresses are also specified at once. All variables are required. Example of a command parameter: ``[{"x":20,"y":30,"row_count":5,"column_count":1,"cell_size":50,"tags":[["1-1","1-2","1-3","1-4","1-5"]]}]``
**map_clear_rows** - clear the list of rows

.. note:: It makes sense to use this command together with the **map_set_size** command, since the sizes and coordinates specified for the cells must be proportional to the size of the virtual canvas.

Working with events and tags
-----------------------------

When working with objects signed with tags, the following events occur with event = MapAction:

* map_action_down – click on an object
* map_action_up - object release
* map_text_input – input text into an object

Interacting with objects other than cell rows
------------------------------------------------

**map_set_color** – set the color of an element. Example ``[{"tag":"arr1","color":"#000000"},{"tag":"btn1","color":"#000000"}]``
**map_map_set_visibility** – enable/disable visibility of an element. Example ``[{"tag":"arr2","visible":1},{"tag":"btn2","visible":1}]``

Working with the map
---------------------

**map_edit_mode** – enable user editing mode. With empty parameter
**map_background_picture** – setting the background – pictures. The principles of operation are the same as with pictures in Simple UI
**map_background_html** – setting as an html image on the screen, essentially a screenshot of the html content. Important! This method can only be called on a screen where there is an html field, i.e. before the screen in which the map field is present. Thus, if you need to transfer a printed form/html report for editing, there must be 2 screens – first html, then the editing screen
**map_save** – saves what the user has drawn along with the background. In the case of the mm_local mode enabled, the **map_saved_file** variable is returned with the name of the file where it was written, if this mode is not enabled, then it is returned to **map_saved_base64**
**map_zoom** – map zoom. Example "1.5" - zoom by 1.5 times
**map_move_x** and **map_move_y** – axis shifts. Makes sense to use together with **map_zoom**. You can specify positive (right, down) and negative (left, up) values
**map_edit_mode** – enable user editing mode. With empty parameter


Launching the map editor in user mode
--------------------------------------------------

**RunVectorEditor**, <path to map file *.sug> - opens the vector editor in the editing mode of the specified file, without the ability to change (if the file does not exist, it creates it). Only the save button is available. In case of successful saving, the "vector_editor" event will be generated
