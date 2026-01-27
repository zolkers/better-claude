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

## Project Structure

### Fabric

```
src/
├── main/
│   ├── java/com/author/modid/
│   │   ├── ModMain.java           # Entrypoint (implements ModInitializer)
│   │   ├── client/
│   │   │   └── ModClient.java     # Client entrypoint (ClientModInitializer)
│   │   ├── block/                  # Custom blocks
│   │   ├── item/                   # Custom items
│   │   ├── entity/                 # Custom entities
│   │   ├── world/                  # World generation
│   │   ├── registry/               # Registration helpers
│   │   ├── mixin/                  # Mixin classes
│   │   ├── network/                # Networking (packets)
│   │   ├── config/                 # Configuration
│   │   └── util/                   # Utilities
│   └── resources/
│       ├── fabric.mod.json
│       ├── modid.mixins.json
│       ├── assets/modid/
│       │   ├── blockstates/
│       │   ├── lang/
│       │   ├── models/
│       │   └── textures/
│       └── data/modid/
│           ├── loot_tables/
│           ├── recipes/
│           └── tags/
```

### Forge / NeoForge

```
src/
├── main/
│   ├── java/com/author/modid/
│   │   ├── ModMain.java           # @Mod annotation entrypoint
│   │   ├── client/
│   │   ├── block/
│   │   ├── item/
│   │   ├── entity/
│   │   ├── world/
│   │   ├── registry/              # DeferredRegister usage
│   │   ├── event/                  # Event handlers
│   │   ├── network/
│   │   ├── config/
│   │   └── util/
│   └── resources/
│       ├── META-INF/mods.toml
│       ├── assets/modid/
│       └── data/modid/
```

## Conventions

### Registry

- Use a dedicated `ModBlocks`, `ModItems`, `ModEntities` class per registry type.
- On Fabric: use `Registry.register()` with a helper method that takes `Identifier`.
- On Forge/NeoForge: use `DeferredRegister` and `RegistryObject` / `DeferredHolder`.
- Always use the mod ID as namespace: `new Identifier("modid", "my_block")`.
- Register everything in a single static `init()` method called from the entrypoint.

### Mixins

- One mixin class per target class -- never mix multiple targets.
- Name pattern: `<TargetClass>Mixin.java` (e.g. `PlayerEntityMixin.java`).
- Use the narrowest injection point possible: `@Inject` > `@Redirect` > `@Overwrite`. Avoid `@Overwrite` unless absolutely necessary.
- Always set `cancellable = true` only when you actually cancel the method.
- Add `@Mixin(TargetClass.class)` and keep mixin classes in a dedicated `mixin/` package.
- Reference mixin config in `fabric.mod.json` or `mods.toml`.

### Events

- On Fabric: use callbacks (`ServerLifecycleEvents`, `ServerTickEvents`, etc.).
- On Forge: use `@SubscribeEvent` annotation with `@EventBusSubscriber`.
- On NeoForge: use `@SubscribeEvent` with the mod event bus or Forge event bus.
- Always unregister or scope event handlers properly.

### Networking

- Define packet classes with a clear `encode` / `decode` / `handle` pattern.
- Always check side (client vs server) before handling.
- Use the mod's channel/namespace for packet IDs.

### Resources & Data

- Every block needs: blockstate JSON, block model, item model, loot table, language entry.
- Every item needs: item model, language entry.
- Use data generators when the mod loader supports them (Fabric Data Generation, Forge DataGen).
- Language files: always include `en_us.json` at minimum.
- Use lowercase_snake_case for all resource paths and registry names.

### Mod Metadata

- `fabric.mod.json` / `mods.toml`: always fill in name, description, version, authors, license.
- Declare dependencies with version ranges (e.g. `"fabricloader": ">=0.15.0"`).
- Set `environment` correctly: `*` for both, `client` for client-only, `server` for server-only.

### Common Mistakes

- Registering blocks/items outside the correct lifecycle phase -- always register during initialization, not later.
- Forgetting to add loot tables -- blocks won't drop anything without one.
- Missing language keys -- untranslated items show raw registry names.
- Using `@Overwrite` when `@Inject` or `@Redirect` would work -- reduces mod compatibility.
- Hardcoding mod ID strings instead of using a constant.
- Not checking physical side for client-only code -- causes server crashes.
