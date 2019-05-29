# Add a Command to the Context Menu
Based on the guide at https://www.howtogeek.com/107965/how-to-add-any-application-shortcut-to-windows-explorers-context-menu/.

Future: summarize, build utility script

Note: unlike listed in guide, it looks like backslashes do not need to be doubled. However, the command path and the %1 parameter should probably be quoted if there's any chance of spaces being present.

Note: Some registry file types don't appear to allow new context items to be added to them. It appears that these files need to have the `shell/{CommandName}/command` structure added to their extension as it is listed within `HKEY_CLASSES_ROOT\SystemFileAssociations\`.

Note: if multiple files are selected when the context menu option is selected, the command will be invoked once for each selected file, with that file as `%1`.
