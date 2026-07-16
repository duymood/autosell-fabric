# AutoSell (standalone)

A standalone Fabric client mod containing just the **AutoSell** feature,
extracted from the [anbatukang](https://github.com/mioclient/anbatukang-ported)
client base (MIT licensed).

It repeatedly runs a configurable "sell" chat command, waits for the shop GUI
to open, quick-moves every item from your hotbar/inventory into it, closes
the screen, waits a short delay, and repeats — until you turn it off.

> ⚠️ This sells **everything** in your inventory. Only enable it with an
> empty inventory, or on a server/gamemode where that's what you want.

## Origin

The original `AutoSellModule.java` was tightly coupled to anbatukang's full
module/event/settings framework (`Module`, `Setting`, `Command`,
`ConfigManager`, `EventBus`, a ClickGui, etc). To make it buildable on its
own, this repo:

- Ports the logic from intermediary (`class_1713`, `field_1724`, ...) to
  standard **Yarn**-mapped Fabric names (`ItemStack`, `SlotActionType`,
  `MinecraftClient`, ...) so it reads like normal Fabric mod source.
- Replaces the framework's `Module`/`Setting`/ClickGui with a minimal
  in-memory `Setting<T>` holder and three `/autosell ...` chat commands
  (`toggle`, `command <name>`, `delay <seconds>`, `confirm`).
- Drops everything unrelated to AutoSell (PacketFly, ESP, AutoTotem, etc.)
  entirely — this repo only contains the sell-automation feature.

The core state machine (send command → wait for GUI → quick-move items →
close → delay → repeat) is unchanged from the original.

## Requirements

- JDK 21
- Minecraft 1.21.11 (matching the original mod's target version)

## Building

```bash
./gradlew build
```

The output jar will be in `build/libs/`.

> **Note:** the exact `yarn_mappings` / `fabric_version` values in
> `gradle.properties` should be double-checked against
> https://fabricmc.net/develop for the Minecraft version you're targeting —
> they were filled in from the original mod's stated dependencies and may
> need bumping by the time you build this.

## Commands

| Command                        | Effect                                   |
|---------------------------------|-------------------------------------------|
| `/autosell toggle`               | Enable/disable AutoSell                   |
| `/autosell command <name>`       | Set the sell command (default: `sell`)    |
| `/autosell delay <seconds>`      | Delay between cycles, 0.5–10s (default 2s)|
| `/autosell confirm`              | Toggle the "sell everything" safety warning |

## License

MIT — see [LICENSE](LICENSE). Original authors: 3arthqu4ke, alpha432, cattyn.
