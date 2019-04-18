# Add a Command to the Context Menu
Based on the guide at https://www.howtogeek.com/107965/how-to-add-any-application-shortcut-to-windows-explorers-context-menu/.

Future: summarize, build utility script

Note: unlike listed in guide, it looks like backslashes do not need to be doubled. However, the command path and the %1 parameter should probably be quoted if there's any chance of spaces being present.

Note: Some registry file types don't appear to allow new context items to be added to them? Maybe a feature of `_auto_file` types that were generated automatically? EditFlags was added to the file type I'm using as a test case, but hiding it doesn't appear to modify this behavior.
