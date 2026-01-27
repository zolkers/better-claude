# Minecraft Modding

## Detection

This environment applies when any of these are found:
- `fabric.mod.json` (Fabric)
- `META-INF/mods.toml` or `mods.toml` (Forge / NeoForge)
- `quilt.mod.json` (Quilt)
- `build.gradle` or `build.gradle.kts` with `net.fabricmc`, `net.minecraftforge`, or `net.neoforged` dependencies

## Questions to Ask

- Which mod loader: Fabric, Forge, NeoForge, or Quilt?
- Target Minecraft version?
- Do you want mixin support?
- Client-side only, server-side only, or both?
- Do you want a config system (Cloth Config, TOML, etc.)?
- Should we use mod menu API integration ?
- Should we make a gradle task generating a libs-src folder containing minecraft's code source and any other lib to your liking ?

### Reliability

- if libs-src exists, please check it before any kind of implementation to see if the class exist / the method signature and keep it in memory