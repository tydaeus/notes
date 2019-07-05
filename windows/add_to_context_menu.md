# Add a Command to the Context Menu
Based on the guide at from [howtogeek](https://www.howtogeek.com/107965/how-to-add-any-application-shortcut-to-windows-explorers-context-menu/).

Future: build utility script

## Common Usage

1. In the registry, find the target file extension under `HKEY_CLASSES_ROOT`, and examine the data under the `(Default)` value (e.g. `HKEY_CLASSES_ROOT\.gif` has value `giffile`). This indicates where configuration for this file type is located, and will be referenced as `{fileTypeConfig}` for this guide.
2. Create a new key under `HKEY_CLASSES_ROOT\{fileTypeConfig}\shell`. Name this key after what you want the menu option to be named. This will be referenced as `{menuCommand}` for this guide.
3. Create a new key under `{menuCommand}` named `command`. Edit the `(Default)` value for this key to contain the desired command. `%1` will reference the full path to the right-clicked file. E.g. `"C:\Program Files\myProgram\myProgram.exe" "%1"`. The new option will appear immediately.

The final key will look something like `HKEY_CLASSES_ROOT\{fileTypeConfig}\shell\{menuCommand}\command` with value `"C:\Program Files\myProgram\myProgram.exe" "%1"`.

*Note*: unlike listed in guide, it appears that backslashes do not need to be doubled. However, the command path and the %1 parameter should probably be quoted if there's any chance of spaces being present.

*Note*: if multiple files are selected when the context menu option is selected, the command will be invoked once for each selected file, with that file as `%1`.

### SystemFileAssociations
Some registry file types don't appear to allow new context items to be added to them. It appears that these files need to have the `shell/{menuCommand}/command` structure added to their extension as it is listed within `HKEY_CLASSES_ROOT\SystemFileAssociations\`.

### Shift Key - Extended
To have a shortcut appear only on shift+right-click, define a string under value `Extended` within the created key.

### All File Types
Create the key structure under `HKEY_CLASSES_ROOT\*\shell` instead of under the file type instead of under the `{fileTypeConfig}` key.

### Desktop Menu
Create the key structure under `HKEY_CLASSES_ROOT\DesktopBackground\shell` key instead of the `{fileTypeConfig}` key for a shortcut that appears when right-clicking the desktop.

### Dir Menu
Create the key structure under `HKEY_CLASSES_ROOT\Directory\shell` to appear when right-clicking a directory. Add under `HKEY_CLASSES_ROOT\Directory\Background\shell` using `%V` instead of `%1` to appear when right-clicking the directory background in explorer.
