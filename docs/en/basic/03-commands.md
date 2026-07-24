# Commands

`plugin.yml` defines one root command: `/raids`. Every command below requires the `customraids.raids` permission, which defaults to operators only.

## General Commands

| Command | Purpose | Permission |
| --- | --- | --- |
| `/raids` | Display the command menu. | `customraids.raids` |
| `/raids reload` | Reload configuration and raid data through the plugin reload process. | `customraids.raids` |
| `/raids stopall` | Stop all active raids. | `customraids.raids` |
| `/raids gui` | Open the management GUI for a player to edit raid presets, Omens, and raid plans. | `customraids.raids` |
| `/raids input <value...>` | Submit a value requested by the current GUI prompt. | `customraids.raids` |

The GUI can edit `WAVE_RAID` and `CHALLENGE_RAID` presets, including waves, challenge settings, weighted mob pools, Bosses, and commands.

## Starting and Controlling Raids

| Command | Purpose | Permission |
| --- | --- | --- |
| `/raids startpreset <name> [customRegion]` | Start a raid in the target region with the specified preset. | `customraids.raids` |
| `/raids force skipwave [customRegion]` | Skip the current wave of a wave raid. | `customraids.raids` |
| `/raids force victory [customRegion]` | Force the current raid to settle as a victory. | `customraids.raids` |
| `/raids force defeat [customRegion]` | Force the current raid to settle as a defeat. | `customraids.raids` |

When `customRegion` is omitted, the sender must be a player standing inside a supported target region. When provided, `customRegion` is resolved as a built-in Custom region by name.

## Custom Regions

| Command | Purpose | Permission |
| --- | --- | --- |
| `/raids region pos1` | Set the current location as the first corner of a Custom region selection. Player only. | `customraids.raids` |
| `/raids region pos2` | Set the current location as the second corner of a Custom region selection. Player only. | `customraids.raids` |
| `/raids region create <name>` | Create a named Custom region from the current selection. Player only. | `customraids.raids` |
| `/raids region delete <name>` | Delete the named Custom region. | `customraids.raids` |
| `/raids region list` | List all Custom regions. | `customraids.raids` |
| `/raids region info <name>` | Show information about the named Custom region. | `customraids.raids` |

Custom regions are stored in `custom_regions.yml`.

## Raid Spawning Area Overrides

| Command | Purpose | Permission |
| --- | --- | --- |
| `/raids region setrange` | Use the current cuboid selection as the raid spawning area for the current region. Player only. | `customraids.raids` |
| `/raids region clearrange` | Clear the spawning area override and restore the region's default boundary. Player only. | `customraids.raids` |
| `/raids region viewrange` | View the spawning area override for the current region. Player only. | `customraids.raids` |

These commands affect the player's current Lands, Residence, Towny, or Custom region. Before using `setrange`, select a cuboid with `/raids region pos1` and `/raids region pos2`.

The override **only limits spawn-point selection**. ActionBars, scoreboards, Titles, chat announcements, and similar displays continue to use the original region and nearby audience logic. Overrides are stored in the runtime file `raid_ranges.yml`.

## Omen Commands

| Command | Purpose | Permission |
| --- | --- | --- |
| `/raids omen give <player\|selector> <omenId> [level]` | Record a custom Omen of the specified level for the target. | `customraids.raids` |
| `/raids omen bottle <player\|selector> <omenId> [level]` | Give the target a bottled item for the specified Omen. Level suggestions range from 1 to 5. | `customraids.raids` |
| `/raids omen clear <player\|selector>` | Clear the target's custom Omen state. | `customraids.raids` |
| `/raids omen info [player\|selector]` | Show Omen information for yourself or the specified target. | `customraids.raids` |

Target arguments support exact online player names, `@a`, `@p`, `@r`, `@s`, and comma-separated targets such as `Steve,Alex,@s`.

## Tab Completion

The command handler completes subcommands, preset names, Custom region names, Omen IDs, selectors, online players, and suggested values for active GUI input prompts.
