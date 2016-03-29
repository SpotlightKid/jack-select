# jack-select

A systray application to quickly change the [JACK] configuration from QjackCtl
presets via DBus.

Overview
--------

This application displays an icon in the system tray (also known as
notification area) of your desktop, which shows the status of the JACK audio
server and when you click on it, a menu pops up which lets you quickly select
from the JACK configuration presets you created with [QjackCtl]. When you
select a preset, its JACK engine and driver configuration settings are loaded
via DBus into JACK and then the server is restarted. This allows you to switch
between different audio setups with just two mouse clicks.

![Screenshot of the pop menu](screenshot.png)

When you hover with the mouse pointer over the systray icon and JACK is
running, a tooltip will show you the name of the active preset (if known), the
most important parameters of the current setup and some server statistics.

![Server status tooltip](tooltip.png)

Lastly, there are menu entries to stop the JACK server and to quit the
application.

To create or edit presets, just use the QjackCtl configuration dialog and make
sure you close it with "Ok" so the changes are saved. jack-select will pick up
the changes automatically.


DBus Interface
--------------

jack-select also has a DBus interface, which means you can use any generic DBus
client to tell jack-select to open its menu, activate a preset by name or to
terminate itself. You can also run the `jack-select` command while another
instance is already running, to access some of the DBus service methods.

When `jack-select` starts up, it first checks whether there is already an
existing application providing the jack-select DBus service. If yes, when
called with no command argument arguments, it tells the running jack-select
instance to open its menu. If a preset name is given as the first command line
argument, it tells the running jack-select instance to activate this preset.
An invalid preset name is silently ignored.

For details about the DBus interface, please use DBus introspection facilities
to examine the `de.chrisarndt.JackSelectService` service on the session bus.


Installation
------------

To install jack-select on your system for everybody, check and install the
requirements below and then run:

    $ git clone https://github.com/SpotlightKid/jack-select
    $ cd jack-select
    $ [sudo] make PREFIX=/usr install

This will install the `jack-select` program, the `jackselect` Python package
and the `jack-select.desktop` file and the `jack-select.png` icon to provide a
desktop start menu entry. It will also install the required Python dependencies
if they haven't been installed yet. Installing `PyGObject` probably won't work
this way, so make sure it is installed some other way beforehand, e.g. via
your distributions package management.

If you want to install jack-select only for the current user, replace the
last command above with:

    $ make install-user


You can start jack-select from your desktop's XDG-compatible start menu or add
it to your autostart folder (e.g. `~/.config/autostart`) to have it started
along your with your desktop.


Requirements
------------

This application works with the DBus-version of JACK only.

It written in Python 3 using the [PyGobject] bindings for GTK 3. In addition to
this, the following third-party Python libraries are required:

* [pyxdg](http://freedesktop.org/Software/pyxdg)
* [dbus-python](https://www.freedesktop.org/wiki/Software/DBusBindings/)

These may be available from the package repository of your distribution as
`python-gobject`, `python-xdg` and `python-dbus` respectively.

Python 2 is not supported.


[JACK]: http://jackaudio.org/
[PyGObject]: https://wiki.gnome.org/Projects/PyGObject
[QjackCtl]: http://qjackctl.sourceforge.net/
