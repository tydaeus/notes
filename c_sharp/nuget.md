# NuGet Notes

NuGet is the default packagemanager for Visual Studio / .Net projects

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



## Sources
* https://docs.microsoft.com/en-us/nuget/consume-packages/configuring-nuget-behavior