# Minecraft Modding

## Detection
Applies when:
- Fabric: `fabric.mod.json`
- Forge/NeoForge: `mods.toml` / `META-INF/mods.toml`
- Quilt: `quilt.mod.json`
- Gradle: Dependencies like `net.fabricmc`, `net.minecraftforge`, or `net.neoforged`.

## Sidedness & Safety
- **Strict Separation**: Never call Client-only code (e.g., `Minecraft.getInstance()`, `Renderer`) from common/server code.
- Use `@Environment(EnvType.CLIENT)` (Fabric) or `@OnlyIn(Dist.CLIENT)` (Forge) to mark client-side classes/methods.
- Use proxy patterns or `@EventBusSubscriber(value = Dist.CLIENT)` where necessary.

## Registry & Events
- **Registration**: Use the specific loader's system (e.g., `DeferredRegister` for Forge/Neo, `Registry.register()` for Fabric).
- **Event Priority**: Always specify event priority if the mod needs to override or act after other mods.
- **Resource Locations**: Always use `new ResourceLocation("modid", "path")` (or `Identifier` for Fabric) instead of hardcoded strings.

## Mixins
- **Inject over Overwrite**: Never use `@Overwrite` unless absolutely necessary. Use `@Inject`, `@ModifyVariable`, or `@Redirect`.
- **Shadowing**: Use `@Shadow` for accessing private fields/methods of the target class.
- **Cancellable**: Use `cancellable = true` only if you intend to stop the original method execution.
- **Remap**: Always specify `remap = true` unless targeting a non-obfuscated method.

## Reliability & Knowledge
- **Source Verification**: If a `libs-src` folder exists, the AI MUST check the actual source code/mappings before proposing an implementation.
- **Mappings**: Follow the project's mapping style (Mojang, Yarn, or MCP). Do not mix them.
- **Data Generation**: Prefer using Data Generators (`DataGenerator`) for JSON files (recipes, blockstates, models) instead of manual JSON creation.

## Networking
- Use the loader's Networking API (e.g., `CustomPayload` or `SimpleChannel`).
- Always validate data on the server side received from a client packet.

## Questions to Ask
- Which mod loader: Fabric, Forge, NeoForge, or Quilt?
- Target Minecraft version?
- Target side: Client, Server, or Both?
- Use a Config API (Cloth Config, Forge Config, etc.)?
- Generate a `libs-src` folder for source reference?
- Should the mod integrate mod menu API ?

### Code Reliability
- if libs-src is present and Minecraft/Yarn mappings happen to be here, always check out the existance of the methods, signature and class before any kind of usage.