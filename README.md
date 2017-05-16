# hud-menu

Provides a way to run menubar commands through a searchable list.


## Dependencies
* python-dbus
* rofi [(or alternatively: dmenu)](https://github.com/jamcnaughton/hud-menu/blob/dev/README.md#using-dmenu-in-place-of-rofi)
* appmenu-qt
* unity-gtk-modules
  * Some systems may need _unity-gtk-module-standalone-bzr_ installed instead.
    * If using the bzr packge you may need to set _com.canonical.unity-gtk-module gtk2-shell-shows-menubar_ to false under dconf. 
 

## Setup
1. ```hud-menu-service.py``` should be started (with python 3+) on the session's startup.
2. The following should be added to the user's ```.profile```: 

    ```
    if [ -n "$GTK_MODULES" ]
    then
      GTK_MODULES="$GTK_MODULES:unity-gtk-module"
    else
      GTK_MODULES="unity-gtk-module"
    fi
    
    if [ -z "$UBUNTU_MENUPROXY" ]
    then
      UBUNTU_MENUPROXY=1
    fi 
   ```
   You can also add ```export APPMENU_DISPLAY_BOTH=1``` to this file on some distributions to show the menubar in apps that may otherwise hide it when the hud-menu-service is running.
    
3. ```hud-menu.py``` should be bound to run (with python 3+) with a shortcut (such as a keyboard shortcut). 

## Usage
The user should active the shortcut when the window they wish to show the application menu entries for has focus.  This will open the dmenu at the top.  The user can then use the keyboard to search and navigate the entries.  Pressing enter will execute the selected entry and pressing escape will close the dmenu without executing anything.

### Explanation
hud-menu-service.py  is an implementation of the com.canonical.AppMenu.Registrar DBus service.  Applications exporting their menu through dbusmenu need this service to run.
hud-menu.py tries to get the menu of the currently focused X11 window, lists possible actions and asks the user which one to run.

### Using dmenu in place of rofi
If you wish to use dmenu in place of rofi ensure all calls to ```hud-menu.py``` (shortcuts and bindings) supply ```dmeny``` as an argument.

### Warning
Installation of unity-gtk-modules may disable the global-menu on some distributions as its constituent packages conflict with those the menus may depend on. This is very problematic for applications that can't support the hud-menu like FireFox and Libreoffice.
