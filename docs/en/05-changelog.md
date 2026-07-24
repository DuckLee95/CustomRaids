# Changelog

## 1.2.0 (2026-07-20)

### New Features

- Added challenge trial raids (`CHALLENGE_RAID`, Pro edition):
  - Configure weighted mob pools and dynamically spawn MythicMobs around players inside the region.
  - Track target kills, personal kills, and player deaths.
  - Display kill, Boss, and wave-compatible phases.
  - Configure challenge victory and defeat commands, reward payouts, and damage rankings.
- Added a challenge raid GUI editor (Pro edition):
  - Create `CHALLENGE_RAID` presets in game.
  - Edit challenge parameters, weighted mob pools, Boss settings, and victory or defeat commands.
  - Switch between `WAVE_RAID` and `CHALLENGE_RAID`.
- Added per-region mob spawning areas:
  - `/raids region setrange`: Set a dedicated raid spawning area for a supported region.
  - `/raids region viewrange`: View the current region's spawning area.
  - `/raids region clearrange`: Clear the current region's spawning area.
  - Supports Custom, Lands, Residence, and Towny regions.
  - The spawning area only limits spawn locations; it does not affect announcements, BossBars, ActionBars, Titles, or scoreboard audiences.
  - Added the runtime data file `raid_ranges.yml`.

### Improvements

- Added failure-reason hooks: `defeat_empty_region`, `defeat_timeout`, and `defeat_max_deaths`.
  - Falls back to `defeat` when a specific failure-reason hook is not configured.
  - Added failure placeholders: `%defeat_reason%`, `%defeat_reason_raw%`, `%fail_after_empty_seconds%`, `%empty_seconds%`, and `%time_limit%`.
- Improved placeholders:
  - Added the challenge-phase placeholder `%phase_raw%`.
  - Added the PlaceholderAPI placeholder `%customraids_raid_phase_raw%`.
  - `%phase%` now uses phase text from the language file.
- Expanded the public API:
  - Added support for external region providers.
  - External plugins can start raids through the API.
  - Added challenge raid and failure-reason data to `CustomRaidHandle` and `CustomRaidFinishEvent`.
- Improved raid plans:
  - Schedule weekly raid events within specified time windows.
  - Improved the raid plan GUI editor.
  - Raid plans are now a Pro feature.
- Improved Folia and scoreboard compatibility:
  - Folia servers display scoreboards through the ProtocolLib API.
  - Improved fallback behavior with TAB, ProtocolLib, or no compatibility plugin.

### Bug Fixes

- Fixed challenge placeholders such as `%target_kills%` not resolving in challenge raid start hooks.
- Fixed BossBars, ActionBars, and scoreboards disappearing after a player left the region boundary.
- Fixed raid mobs sometimes disappearing after player death teleports or chunk unloads, which could stall raids.
- Active raid mobs are now tracked continuously, and their chunks are kept loaded.

### Upgrade Notes

- Updated the Chinese and English documentation.
- Updated raid configuration, raid plan, API, PlaceholderAPI, and command documentation.
- Added or updated challenge raid examples, complete configuration examples, and MythicMobs examples.

> This release adds language-file fields, challenge raid examples, and `raid_ranges.yml`. Back up existing configuration files and merge the new fields before upgrading.

## 1.1.0 (2026-05-24)

- Added Towny support.
- Added Folia support. Folia servers require ProtocolLib for scoreboards; other server types do not.
- Added raid plans (Pro edition):
  - Schedule raid events by weekday, time window, world, region provider, and raid preset.
  - Only regions containing online players are targeted.
  - Added the default configuration file `raid-plans.yml`.
- Added a raid plan GUI editor (Pro edition) with create, delete, enable, disable, and edit operations.
- Improved the command system.
- Made MythicMobs a required dependency.
- Fixed some built-in MythicMobs health bars remaining after raid mob cleanup.

## 1.0.6 (2026-05-16)

- Added anonymous bStats metrics.
- Fixed an incorrect plugin version display.

## 1.0.5 (2026-05-09)

- Improved Omen GUI configuration.

## 1.0.4 (2026-05-09)

- Added `milk_clears_custom_omen` to Omen configurations to control whether milk clears custom Omens.
- Updated documentation and example configurations.

## 1.0.3 (2026-05-08)

- Improved `/raids stopall` so it also removes mobs spawned by the stopped raids.
- Remaining mobs are now removed automatically when a raid is defeated.
- Added `/raids omen bottle <player|selector> <omenId> [level]` to give a potion bound to a custom Omen.
- Servers older than 1.21.5 use a regular potion as the Omen potion fallback.
- Added a "Give Omen Potion" button to the Omen details page.
- Improved TAB scoreboard compatibility:
  - Added the TAB API dependency.
  - Temporarily disables the TAB scoreboard while raid rankings are visible, then restores it to reduce Sidebar conflicts and flickering.
- Improved Omen potions:
  - Configure the display name with `omen.bottle_item_name`.
  - Configure lore with `omen.bottle_lore`.
  - Drinking the potion applies the custom Omen logic.

> This release adds language-file fields. Delete the old language files and let the plugin regenerate them when upgrading.

## 1.0.1 (2026-05-03)

- Improved raid mob spawning and performance.
