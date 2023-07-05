# Minecraft World Files
Minecraft worlds in Windows get installed to `%APPDATA%\Local\Packages\Microsoft.Minecraft*\LocalState\games\com.mojang\minecraftWorlds`. Locally created worlds will use software-generated strings that are not user-readable, but you can copy downloaded worlds directly in with a chosen name.

## Embedded Resources
Embedded resource packs get stored in the `resource_packs` subdir, and the `\world_resource_packs.json` file defines the order in which the packs get applied. Each pack entry has properties `pack_id` and `version`, which identifies the uuid and version array of the referenced pack. Leaving the list empty results in only the global resource packs being applied, regardless of bundled resource packs.