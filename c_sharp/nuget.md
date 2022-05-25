# NuGet Notes

NuGet is the default packagemanager for Visual Studio / .Net projects. Official documentation at: https://docs.microsoft.com/en-us/nuget/.

## NuGet ConfigFiles
NuGet is configured by a hierarchical series of `Nuget.Config` files, applied in order:

1. The `NuGetDefaults.Config` file, which contains settings related only to package sources
2. The computer-level `Nuget.Config` file
3. The user-level `Nuget.Config` file (`%appdata%\NuGet\NuGet.Config` on Windows, `~/.config/NuGet/NuGet.Config`)
4. The file specified with `-configFile`
5. `Nuget.Config` files found in every folder in the path from the drive root to the current folder (where nuget.exe is invoked or the folder containing the Visual Studio project). E.g., if a command is invoked in `c:\A\B\C`, NuGet looks for and loads `Nuget.Config` config files in `c:\`, then `c:\A`, then `c:\A\B`, and finally `c:\A\B\C`.

Files applied later effectively add to or overwrite configuration from earlier files.

NuGet **ignores it silently if it doesn't like your NuGet.Config**; for this reason, it is strongly recommended to use the NuGet.exe commandline tool with the `-ConfigFile` parameter when editing config files as much as possible.

### Creating a NuGet ConfigFile
Despite its finickiness, nuget.exe doesn't provide a facility for creating a new config file. Instead, you must copy-paste the initial snippet:

``` XML
<?xml version="1.0" encoding="utf-8"?>
<configuration>
</configuration>
```

### Editing Nuget ConfigFile
NuGet configuration commands modify the user-level Nuget.Config file by default. Use the `-ConfigFile` parameter to specify a different config file to modify.

``` bat
:: retrieve a configuration key
nuget config <KeyName>

:: set a configuration key(s)
nuget config -set <KeyName>=<KeyValue> [-set <KeyName>=<KeyValue>...]
```

### Nuget Sources

``` bat
:: list configured sources
nuget sources

```



## List Packages
``` bat
nuget list [search terms] [options]
```

Options:
* `-ConfigFile` specify which nuget config file to use
* `-Source` specify which source to list packages from


## Install Package
For testing purposes, packages can be installed arbitrarily outside of a configured project.

``` bat
nuget install <packagename>
```



## Creating Legacy Nuget Packages
In ideal circumstances, nupkgs should be created through Visual Studio as part of the development process. However, it may be necessary at times to package a legacy 3rd-party dll as a nupkg.

1. Create a folder structure with the desired package name, ideally in nuget name.version format; this structure must have at minimum a `lib` subdir containing the dll(s) for the package
2. Create a .nuspec file for the package; `nuget spec <assembly-name>.dll` will create a basic nuspec with the dll's name and version, everything else needs to be manually filled (`nuget spec` on its own within the dir is another option)
3. `nuget pack <nuspec file>` will populate the remainder of the structure and output the resulting .nupkg to PWD

Visual Studio will not recognize nuget packages whose versioning doesn't match nuget standard version format -- e.g. version `1.2.1` will show up when browsing packages, but `1.2.1.1` will not.


## Sources
* https://docs.microsoft.com/en-us/nuget/
* https://docs.microsoft.com/en-us/nuget/consume-packages/configuring-nuget-behavior