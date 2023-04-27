# MSI Files
Microsoft's `MsiExec.exe` command-line tool creates and manages `.msi` (Microsoft Installer) files. Note that some vendors may bundle their installer as a `.exe` file, but in these cases most/all msi operations should be the same.

Run `msiexec` without any parameters to get a dialog box listing msi command-line options.

Note that operations can be performed by either running `msiexec` and targeting the `msi` file, or by running the `msi` file itself.

## Viewing/Extracting MSI Files
(See https://www.raymond.cc/blog/how-to-view-and-extract-contents-from-a-msi-file/).

Using msiexec: `msiexec /a <msiSourcePath> /qb TARGETDIR=<outputPath>`. Use `/qn` (silent UI) instead of `/qb` (basic UI) for silent unpacking.

Some general archiving tools can read and extract msi contents, including 7-zip and HaoZip.

Some dedicated tools also exist, such as Universal Extractor, Less MSIerables, MSI Unpacker.

## Analyzing MSI Installation
use the `/log` or `/L` option followed immediately by log control characters, a space, and then the path to the desired log file to enable logging during installation.

E.g. `msiexec /i myinstaller.msi /L*V C:\temp\myinstaller.log`.

Sample log control characters:

* `*V` - log everything + verbose
* `p!` - log properties only

In particular, the log output will include a list of the properties set and calculated by the installation process, e.g `Property(S): MYPROPERTY = My property value`. Public properties set in the GUI appear to get tracked as properties whose names are all uppercase, so you can use this information to determine what custom properties need to be set to convert from GUI to command-line installation.

Commonly, you can look for the log line that starts with `Command Line`, and the `ADDLOCAL` property will capture user preferences.

## Setting Properties
Set public install properties for installation from the command-line by specifying them after other parameters in `NAME=VALUE` format.

E.g. `msiexec /i myinstaller.msi MYPROP=Value MYOTHERPROP="Value with spaces"`

## Uninstalling
Typically, the installer can be used to uninstall with the the `/x` option. Add the `/S /v/qn` options to perform a fully silent uninstall.

``` cmd
msiexec /x myinstaller.msi /S /v/qn
:: or
myinstaller.msi /x /S /v/qn
```

### Using Registered Uninstaller
If a product was installed using a standard msi installer, the uninstaller will be registered with Windows to allow easy uninstall.

Look up the product's `IdentifyingNumber` by using PowerShell: 

``` PowerShell
Get-WmiObject Win32_product -Filter "name = `"$ProductName`""
```

With variable `IdentifyingNumber` set to contain the IdentifyingNumber property discovered in the previous step, including its surrounding `{}`, use the following PowerShell command to uninstall:

``` PowerShell
Start-Process 'msiexec' "/qn /norestart /x $IdentifyingNumber" -Wait -NoNewWindow
```

Add the `-PassThru` param and assign to a variable if you want to check the exit status (recommended).


## Extracting MSP Files from InstallShield exe Installer
Microsoft Patch files may be distributed within an InstallShield-generated standalone exe installer. These files can then be extracted from the exe for use by msiexec.

First extract the files:

``` bat
:: With:
::   UPDATER as path to executable
::   TARGETDIR as path to (nonexistent) dir to contain extracted files
%UPDATER% /s /e /f %TARGETDIR%
```

The MSP file will be in TARGETDIR, within a MSP subdir.

Then install:

``` bat
:: With:
::  MSPFILE as path to the .msp file
::  LOGPATH as filepath to write log to
msiexec /update %MSPFILE% /qb /l*xv %LOGPATH%
```

Some instructions indicate that you may need to be operating from within same dir as the .msp file; I have not tested this.