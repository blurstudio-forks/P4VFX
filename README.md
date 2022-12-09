# P4VFX: A Perforce Toolset for VFX Software

## Fork
This is a fork of [P4VFX](https://github.com/TomMinor/P4VFX) modified for non-user distribution.

## Description
P4VFX is a toolset that is intended to make working with Perforce from within VFX content creation applications simple and intuitive for artists. It achieves this by stripping out complex features such as branching and only providing the tools an artist actually needs to checkout assets, submit/manage changelists, view local and remote file history and revert work as necessary.

![Alt text](images/p4vfx_revision_view.png?raw=true "Perforce for Maya Changelist Interface")
![Alt text](images/nuke_p4vfx_menu.png?raw=true "Perforce for Maya")
![Alt text](images/maya_p4vfx_menu.png?raw=true "Perforce for Nuke")

Supported functionality:
* Depot/Client browser to clearly show what files are to be added, edited or removed (can also view deleted files for restore).
* Visual progress bar feedback on changelist submission and sync, ideal for when large/numerous assets are shared amongst the team.
* Submit changes, choose exactly which files you want to submit and add a description.
* Find an older scene revision in the depot browser, temporarily save it to disk and preview it in Maya with one button.
* Revert to older versions of assets without deleting history at the click of a button

As this was developed for an actual project, various pipeline functions were added to support a complex pipeline structure:
* On scene save and reference operations, ensure all filepaths are relative to an environment variable pointing to the root of the project. This ensures that relative paths to references and assets work on every artist's machine.
* Automatically strip out the student flag from saved Maya scenes (handy!)
* Asset and shot creation wizards to simplify the process of ensuring everything follows a consistent structure

It uses [QtPy](https://github.com/spyder-ide/qtpy) to allow use in both Qt4/Qt5 applications and the [P4Python API](https://www.perforce.com/downloads/helix#product-54).

## Install

### Installing for Maya

The maya integration is done with a Maya module instead of installing into the user's home directory. This makes it easier to distribute to multiple users.

Add the path to the repository root(folder containing [P4Maya.mod](P4Maya.mod)) to the environment variable `MAYA_MODULE_PATH`.

Note, this module is only valid for python 2 versions of Maya.  It also forces QtPy to use PySide2 even if PyQt5 is installed for Maya. This can be disabled by removing the `P4Maya-blur` section in [P4Maya.mod](P4Maya.mod).

### Installing for Houdini

Add the full path to [src/AppPlugins/P4Houdini](src/AppPlugins/P4Houdini) to the `HOUDINI_PATH` environment variable. If you are creating this variable you likely will want to add `;&`. Also make sure to use forward slashes on windows as Houdini often won't recognize them.

For example:
```
HOUDINI_PATH = C:/dev/checkouts/P4VFX/src/AppPlugins/P4Houdini;&
```

### Nuke

The installation process is now automated by **install.py**, it will support all out of the box plugins (Maya, Nuke, etc) as they are added. It handles symlinking the module files to the places necessary for each app, and creating/updating P4CONFIG if necessary.

To use it, simply call it like so:
```python install.py```

(For convenience, P4Python is bundled within the repo.)

**Note: The Mac P4Python library isn't bundled within the repo yet, but it is just a case of placing it in appropriate PATH so the application can find it.**

## Removing previous install.py configuration

If you previously ran install.py you likely will need to remove the old installation.

For Houdini you need to edit the houdini.env file for each houdini preferences. On windows an example path `C:\Users\<username>\Documents\houdini19.0\houdini.env`. Make sure to edit the houdini.env file for each of the houdiniXX.X folders.

For Maya, there are several symlinks/file copies in your global maya directory in your user directory. For windows remove these files/folders if they exist:
* `C:\Users\<username>\Documents\maya\scripts\perforce`
* `C:\Users\<username>\Documents\maya\scripts\P4.py`
* `C:\Users\<username>\Documents\maya\scripts\P4.pyc`
* `C:\Users\<username>\Documents\maya\scripts\P4API.pyd`
* `C:\Users\<username>\Documents\maya\plug-ins\P4Maya.py`

## Configuring

When possible the default Perforce functionality is used for determining the environment.

This typically involves setting a P4CONFIG env var to something like '.p4config', Perforce will then search the current directory and it's parents for the existence of this file. This behaviour allows you to determine which workspace is used depending on where the p4config file is placed, typically in the settings folder for your app of 


## License

This project is licensed under the MIT license.
