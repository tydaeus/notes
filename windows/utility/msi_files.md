# MSI Files
Microsoft's `MsiExec.exe` command-line tool creates and manages `.msi` (Microsoft Installer) files. Note that some vendors may bundle their installer as a `.exe` file, but in these cases most/all msi operations should be the same.

Run `msiexec` without any parameters to get a dialog box listing msi command-line options.

Note that operations can be performed by either running `msiexec` and targeting the `msi` file, or by running the `msi` file itself.

## Viewing/Extracting MSI Files
(See https://www.raymond.cc/blog/how-to-view-and-extract-contents-from-a-msi-file/).

Using msiexec: `msiexec /a <msiSourcePath> /qb TARGETDIR=<outputPath>`. Use `/qn` instead of `/qb` for silent unpacking.

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