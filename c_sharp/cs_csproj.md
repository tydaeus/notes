# `.csproj` Files

C-Sharp projects store the settings required for project build; a .csproj can be used in absence of a .sln, but a .sln requires a .csproj.

The .csproj file is XML following a specific format. Documentation of the format seems poor; some slightly outdated configuration item documentation is available at: https://learn.microsoft.com/en-us/previous-versions/visualstudio/visual-studio-2015/msbuild/common-msbuild-project-items?view=vs-2015


## Visual Studio Config Display vs. `.csproj` Contents
Most/all configuration handled in a `.csproj` file is displayed somewhere in Visual Studio, but the correspondence is poor.

### References
Properties of references.

#### VS CopyLocal vs `.csproj` Private
CopyLocal corresponds to the `.csproj` `<Private>` property; if True, the referenced dll will be copied to output.

If no `<Private>` property is specified, CopyLocal default gets calculated per Microsoft wizardry. There is no difference in the Visual Studio property viewer display between default calculation and explicit configuration.

#### VS Path vs `.csproj` HintPath
VS Path property shows where the dll is being referenced from on the current system.

The `.csproj` `<HintPath>` tag provides a suggestion as to where the dll should get referenced from, but the build process will pull the dll from somewhere else (if found) if it isn't present on the `<HintPath>` and copy it to the output.