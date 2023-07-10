# Working with the PowerShell Console UI

## Working with `$Host`
(sourced from https://www.pdq.com/blog/change-powershell-colors/)

The automatic `$Host` variable, which can also be retrieved via `Get-Host` can be used to retrieve and manipulate the PowerShell console window.

### `$Host.PrivateData` Properties
`$Host.PrivateData` holds the settings for foreground and background color for the error, warning, debug, verbose, and progress output streams.

```
ErrorForegroundColor    : Red
ErrorBackgroundColor    : Black
WarningForegroundColor  : Yellow
WarningBackgroundColor  : Black
DebugForegroundColor    : Yellow
DebugBackgroundColor    : Black
VerboseForegroundColor  : Yellow
VerboseBackgroundColor  : Black
ProgressForegroundColor : Yellow
ProgressBackgroundColor : DarkCyan
```

### `$Host.UI.RawUI` Properties
`$Host.UI.RawUI` holds settings for general foreground and background colors, window size, title and position.

```
ForegroundColor       : DarkYellow
BackgroundColor       : DarkMagenta
CursorPosition        : 0,235
WindowPosition        : 0,196
CursorSize            : 25
BufferSize            : 120,3000
WindowSize            : 120,40
MaxWindowSize         : 120,61
MaxPhysicalWindowSize : 168,61
KeyAvailable          : False
WindowTitle           : Administrator: Windows PowerShell
```

Note that changes to background color will require a `Clear-Host` (aka `cls`) before displaying properly. All changes to these properties persist only for the current session, unless added to the user's PowerShell profile.

Set console window title by modifying the WindowTitle property.


## Working with Console Colors
(source: https://stackoverflow.com/a/12340136/2939139)

The console colors are a set of system-defined enums with user-customizable color values, so their names don't always match how they display. I have not found a way to lookup what the in-effect color values are.

Get info about a given color name e.g. 'Red' via `[System.Drawing.Color]::FromName('Red')`:

```
R             : 255
G             : 0
B             : 0
A             : 255
IsKnownColor  : True
IsEmpty       : False
IsNamedColor  : True
IsSystemColor : False
Name          : Red
```

Note that 'DarkYellow' gets some strange special treatment.
