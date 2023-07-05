# Minecraft Modding

## Minecraft .mcpack structure
A minecraft .mcpack file is a renamed .zip file conforming to specific structural guidelines:

* optional `subpacks/`
* `textures/`
    + `blocks/`
    + `entity/`
    + `terrain_texture.json`
* `biomes_client.json`
* `blocks.json`
* optional `LICENSE.url` pointing to the license for the pack
* `manifest.json`
* `pack_icon.png`

### `manifest.json`

``` json
{
    "format_version": 2,
    "header": {
        "description": "Human-readable brief description of the pack.\nNewline for linebreaks.",
        "name": "Pack Name",
        "uuid": "Unique_UUID",
        "version": [1,0,0],
        "min_engine_version": [1,16,0]
    },
    "modules": [
        {
            "description": "Human-readable brief description of the module.",
            "type": "resources",
            "uuid": "Unique_UUID_2",
            "version": [1,0,0]
        }
    ],
    "metadata": { "authors": ["Author0"] },
    "subpacks": [
        {
            "folder_name": "SubpackFolderName0",
            "name": "Fogless", "memory_tier": 1
        },
        {
            "folder_name": "SubpackFolderName1",
            "name": "Default", "memory_tier": 1
        }
    ],
    "capabilities" : 
        [ "raytraced" ]
}
```

## Minecraft RTX Texturing
From https://www.nvidia.com/en-us/geforce/guides/minecraft-rtx-texturing-guide/

Texture data is stored in several files:
* texture set description
    - json
    - named for the the asset being described with ".texture_set" before the extension (e.g. `glass.texture_set.json`)
    - uses `minecraft:texture_set` property object to describe the names of the files that will be used, e.g.:
        ```json
        {
            "format_version":"1.16.100",
            "minecraft:texture_set":{
                "color":"glass",
                "metalness_emissive_roughness":"glass_mer",
                "heightmap":"glass_heightmap"
            }
        }
        ```
* color file
    - tga or png
    - conventionally named for the asset being described (e.g. `glass.png` for the glass texture)
    - stores the RGB color values for the texture
    - transparency stored in the alpha channel (nvidia recommends tga files for explicit alpha channel, but png alpha appears to be recognized)
        + colors apply for light shining through even if fully transparent, so transparent pixels should be white
        + documentation indicates that RTX textures applied without RTX will treat textures as less transparent than stored (0-50% as fully opaque, 50-100% translated to 0-100%)
* mer file
    - tga or png
    - conventially named as per the color file with "_mer" appended after the asset name (e.g. `glass_mer.png` for the glass mer)
    - Red channel describes metalness (0% non-metallic, 100% fully metallic)
        + typically will be either 0% or 100%
    - Green channel describes emissiveness (0% non-emissive, 100% fully emissive)
        + color of pixel will determine color of light emitted
    - Blue channel describes roughness (0% highly polished, 100% completely rough)
    - A mer file at entirely 0% roughness and 100% metalness is recognized by RTX as a perfect mirror, which simplifies rendering
* heightmap or normalmap
    - tga or png
    - heightmap uses red channel only for pixelated depth effect where:
        + 0% is fully recessed into block
        + 50% is level with block surface
        + 100% is fully extruded from block surface
    - normal map typically gets exported from a rendering program


## Helpful Resources

* RTX expo museum - world created to demo RTX textures https://www.minecraft.net/content/dam/games/minecraft/software/RTX-Expo-Museum.zip
    - note: custom RTX texturing is embedded in this world, more useful to remove or augment to test desired texture pack