# Example Pack Guide

The example pack contains a local testing configuration that covers the following features:

- Wave raids
- Regional challenge raids
- Custom Omens
- Raid plans
- Custom regions
- Example MythicMobs
- Custom Omen drops
- Custom display elements
- Raid rewards, failure conditions, and settlement commands

## Directory Structure

- `examples/config.yml`
  - Example CustomRaids global configuration.
- `examples/custom_regions.example.yml`
  - Example built-in Custom region data.
- `examples/raid-plans.yml`
  - Example scheduled raid plans.
- `examples/raids/`
  - `example_border_patrol.yml`: Wave raid using `near_players`.
  - `example_siege_ring.yml`: Wave raid using `ring`.
  - `example_last_stand.yml`: High-pressure wave raid using `random_in_region`.
  - `example_challenge_trial.yml`: Regional `CHALLENGE_RAID`.
- `examples/omens/`
  - `example_omens_pack.yml`: Multiple Omen routes, notification modes, and level weights.
- `examples/mythicmobs/`
  - `ExampleMobs_AllFeatures.yml`: Test MythicMobs.
  - `ExampleDropTables_AllFeatures.yml`: Test Omen drop table.

## Installing on a Server

1. Copy `examples/config.yml` to `plugins/CustomRaids/config.yml`.
2. Copy `examples/raids/*.yml` to `plugins/CustomRaids/raids/`.
3. Copy `examples/omens/*.yml` to `plugins/CustomRaids/omens/`.
4. Copy `examples/raid-plans.yml` to `plugins/CustomRaids/raid-plans.yml`.
5. Optionally copy `examples/custom_regions.example.yml` to `plugins/CustomRaids/custom_regions.yml`.
   - Before copying, verify that its `world` and coordinates are valid for the test server.
6. Install the MythicMobs examples:
   - Copy `examples/mythicmobs/ExampleMobs_AllFeatures.yml` to `plugins/MythicMobs/Mobs/`.
   - Copy `examples/mythicmobs/ExampleDropTables_AllFeatures.yml` to `plugins/MythicMobs/DropTables/`.
7. Run `/mm reload`, followed by `/raids reload`.

## Recommended Test Flow

1. Create a Custom region:
   - `/raids region pos1`
   - `/raids region pos2`
   - `/raids region create test_arena`
2. Start wave raids:
   - `/raids startpreset border_patrol test_arena`
   - `/raids startpreset siege_ring test_arena`
   - `/raids startpreset last_stand test_arena`
3. Start a challenge raid:
   - `/raids startpreset challenge_trial test_arena`
4. Test forced controls:
   - `/raids force skipwave test_arena`
   - `/raids force victory test_arena`
   - `/raids force defeat test_arena`
5. Test Omens:
   - `/raids omen give @s omen_patrol_msg 1`
   - `/raids omen give @s omen_challenge_title 2`
   - `/raids omen bottle @s omen_laststand_title 3`
   - `/raids omen info @s`
   - `/raids omen clear @s`
6. Test MythicMobs Omen drops:
   - Spawn or kill `CR_OmenDrop_Messenger`.
   - Spawn or kill `CR_OmenDrop_Harbinger`.
7. Test scheduled plans:
   - Change `world`, `days`, and `time` in `raid-plans.yml` to match the current environment and time window.
   - Keep a player inside the corresponding provider region and wait for the scheduler check.

## Feature Coverage

- `WAVE_RAID` and `CHALLENGE_RAID`
- `near_players`, `ring`, and `random_in_region` spawning strategies
- Dynamic challenge spawning, weighted mob pools, target kills, and the Boss phase
- PvP blocking, vanilla mob blocking, inventory and experience retention on death, and end cleanup
- Global failure conditions for empty regions and death limits
- Whole-raid and per-wave time limits
- `temp_flags` and TTL
- `safe_check` safe spawning checks
- `mob_glow` mob highlighting
- Hook chat, sounds, and Titles
- BossBars, ActionBars, damage rankings, and scoreboards
- Participation and TOP 1, TOP 2, and TOP 3 reward commands
- Challenge victory and defeat commands
- Omen state, vanilla Bad Omen triggers, milk clearing, consumption after triggering, notification modes, default sounds, level ranges, and weighted selection
- PlaceholderAPI `%customraids_...%` placeholders

## Notes

- Hook `scope: region` sends messages to nearby players in the region. Other values broadcast to the server. The examples therefore use only `region` or `server`.
- Example reward commands use `say` so results are visible in the console. Replace them with economy, item, or permission commands on a production server.
- Avoid giving Omen drops to ordinary raid mobs, which could trigger consecutive raids. The example pack provides dedicated `CR_OmenDrop_*` mobs for Omen drop testing.
