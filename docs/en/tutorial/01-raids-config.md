# Raid Configuration Tutorial

## Editing in the In-Game GUI

![Raid configuration menu](../../../images/raid-config.png)

Use `/raids gui` to open the in-game management menu and create raids quickly.

## Editing Configuration Files

### File Location

Raid presets are YAML files stored in `plugins/CustomRaids/raids/`.

Built-in examples are located in `src/main/resources/raids/`.

### Top-Level Structure

```yaml
# Raid preset name
name: example_raid

# Raid type: WAVE_RAID or CHALLENGE_RAID
# Optional; defaults to WAVE_RAID
type: WAVE_RAID

# Optional compatibility alias for type; raid-type and mode are also accepted
# In most configurations, only type should be used
raid_type: WAVE_RAID

# Required global configuration
global: {}

# Lifecycle hook configuration
hooks: {}

# BossBar, ActionBar, ranking, scoreboard, and Title configuration
display: {}

# Participation and ranking rewards
rewards: {}

# Wave list for a wave raid
# WAVE_RAID requires at least one valid entry
waves: []

# Regional challenge raid configuration
# CHALLENGE_RAID requires at least one valid challenge.mobs entry
challenge: {}
```

If no wave list exists but `challenge.mobs` is present, the parser treats the preset as a challenge raid.

### Global Configuration

```yaml
global:
  # Flags temporarily applied to the region during the raid
  # Common flags include monsters, explode, and pvp; behavior depends on the region provider
  temp_flags: {}

  # Duration of temporary region flags, in ticks
  temp_flags_ttl_ticks: 2400

  # Default preparation time before each wave, in seconds
  prepare_between_seconds: 10

  # Time limit for the entire raid, in seconds
  # For wave raids, 0 means unlimited
  time_limit_seconds: 0

  failures:
    # Fail after the region remains empty for this many seconds
    # Set to 0 to disable; applies to all raid types
    fail_after_empty_seconds: 0

    # Fail after the total recorded player deaths reach this value
    # Set to 0 to disable; applies to all raid types
    max_deaths: 0

  mob_glow:
    # Whether active raid mobs periodically receive a glowing effect
    enabled: false

    # Glow refresh interval, in seconds
    interval_seconds: 8

    # Duration of each glow effect, in seconds
    duration_seconds: 5

  spawn:
    # Wave spawn-point strategy
    # random_in_region: random locations inside the region
    # near_players: locations near players
    # ring: locations around a ring of the configured radius
    strategy: random_in_region

    # Maximum attempts to find a valid location for each spawn point
    tries_per_point: 60

    # Minimum distance between planned spawn points
    min_dist_between: 3.0

    # Minimum distance between wave spawn points and players
    min_dist_from_players: 6.0

    # Whether spawn locations are aligned to block centers
    center_on_block: true

    # Radius used by the ring strategy
    ring_radius: 10.0

    # Whether to check loaded chunks, region boundaries, floor blocks, headroom, fluids, and hazards
    # check_safe_block is an accepted compatibility alias
    safe_check: true
```

Failure conditions support the legacy keys `empty_region_seconds`, `empty_seconds`, and `death_limit`.

Challenge raids do not use the wave spawning strategy above. Instead, `SpawnPointPlanner.pickAroundPlayers` dynamically finds valid points around players who remain inside the region, using the challenge distance settings.

### Hook Configuration

```yaml
hooks:
  # Runs when a raid starts
  start:
    scope: server
    chat: "&a[Example] Raid started in &f%region%&a."
    sound: { id: "minecraft:raid.horn", volume: 1.0, pitch: 1.0 }

  # Runs when a wave is cleared
  wave_cleared:
    scope: region
    chat: "&a[Example] Wave %wave%/%waves% cleared."
    sound: { id: "minecraft:entity.player.levelup", volume: 1.0, pitch: 1.1 }

  # Runs when a raid is won
  victory:
    scope: server
    chat: "&a[Example] %region% survived all waves."
    sound: { id: "minecraft:ui.toast.challenge_complete", volume: 1.0, pitch: 1.0 }

  # Runs when a raid is defeated; also used as the fallback for unconfigured failure reasons
  defeat:
    scope: server
    chat: "&c[Example] %region% raid failed."
    sound: { id: "minecraft:entity.wither.death", volume: 1.0, pitch: 0.9 }

  # Runs when the region remains empty long enough to fail
  defeat_empty_region:
    scope: server
    chat: "&c[Example] %region% raid failed because all players left for %fail_after_empty_seconds%s."
    sound: { id: "minecraft:entity.wither.death", volume: 1.0, pitch: 0.9 }

  # Runs when the raid times out
  defeat_timeout:
    scope: server
    chat: "&c[Example] %region% raid timed out."
    sound: { id: "minecraft:entity.wither.death", volume: 1.0, pitch: 0.9 }

  # Runs when the total player death limit is reached
  defeat_max_deaths:
    scope: server
    chat: "&c[Example] %region% raid failed because deaths reached %total_deaths%/%max_deaths%."
    sound: { id: "minecraft:entity.wither.death", volume: 1.0, pitch: 0.9 }
```

`defeat_empty_region`, `defeat_timeout`, and `defeat_max_deaths` apply to every raid type.

Compatibility aliases include `defeat_empty`, `defeat_leave`, `defeat_leave_region`, `defeat_time_limit`, `defeat_time`, `defeat_deaths`, and `defeat_death_limit`.

### Display Configuration

```yaml
display:
  bossbar:
    # BossBar color
    color: RED

    # BossBar segment style
    style: SEGMENTED_10

    # Text shown during the countdown phase
    countdown_title: "&e%region% %countdown%s"

    # Text shown while the raid is in progress
    in_progress_title: "&c%region% Alive: %alive%"

    # Text shown while a wave is preparing
    preparing_title: "&e%region%"

    # Text shown when the raid is won
    victory_title: "&a%region%"

    # Text shown when the raid is defeated
    defeat_title: "&c%region%"

  actionbar:
    # Countdown before a wave starts
    countdown: "&eWave %wave%/%waves% starts in %countdown%s"

    # In-progress text when the current wave has a time limit
    in_progress_timed: "&cAlive %alive% | Wave left %remain%s"

    # In-progress text when the entire raid has a time limit
    in_progress_raid_timed: "&cAlive %alive% | Raid left %remain%s"

    # In-progress text when there is no time limit
    in_progress_unlimited: "&cAlive %alive%"

  ranking:
    # Damage ranking header
    header: "&6Damage Ranking"

    # Format for each ranking entry
    line: "&e#%rank% &f%player% &c%damage%"

    # Text shown when no damage has been recorded
    empty: "&7No damage recorded."

    # Maximum number of ranking entries
    max_entries: 5

  scoreboard:
    # Whether the scoreboard is enabled
    enabled: true

    # Maximum number of scoreboard entries
    max_entries: 5

  titles:
    # Main Title prefix for a wave
    wave_title_prefix: "&eWave"

    # Wave subtitle format
    wave_subtitle: "&7%wave%/%waves%"
```

### Reward Configuration

```yaml
rewards:
  # Console commands executed for all eligible participants
  participation_commands:
    - "say %player% participated"

  # Console commands executed for first, second, and third place in the damage ranking
  top1_commands:
    - "say TOP1 %player%"
  top2_commands:
    - "say TOP2 %player%"
  top3_commands:
    - "say TOP3 %player%"
```

`rewards.commands` is a compatibility key for the legacy participation reward command list. The top three rewards are awarded by damage ranking.

Common reward placeholders:

- `%player%`
- `%rank%`
- `%damage%`
- `%region%`
- `%preset%`
- `%participants%`
- Failure-related:
  - `%defeat_reason%`
  - `%defeat_reason_raw%`
  - `%fail_after_empty_seconds%`
  - `%empty_seconds%`
  - `%time_limit%`
  - `%total_deaths%`
  - `%max_deaths%`
- Challenge-related:
  - `%kills%`
  - `%deaths%`
  - `%total_kills%`
  - `%target_kills%`
  - `%phase%`
  - `%phase_raw%`

`%phase%` uses the `raid.display.challenge.phase.*` language keys. `%phase_raw%` preserves the stable raw key.

### Wave Raids (`WAVE_RAID`)

```yaml
# Use a wave raid
type: WAVE_RAID

waves:
  # Each list entry represents one wave
  - # Current wave title
    title: "&eWave 1"

    # Preparation time before this wave, in seconds
    # Defaults to global.prepare_between_seconds when omitted
    prepare_seconds: 5

    # Time limit for this wave, in seconds; 0 means unlimited
    time_limit_seconds: 0

    sound:
      # Sound resource ID played when this wave starts
      id: "minecraft:raid.horn"

      # Sound volume
      volume: 1.0

      # Sound pitch
      pitch: 1.0

    mobs:
      # id: MythicMobs mob ID
      # count: integer number spawned in this wave
      - { id: EX_Walker, count: 6 }
      - { id: EX_Thrower, count: 2 }
```

The wave is considered cleared when none of its tracked mobs remain alive.

### Regional Challenge Raids (`CHALLENGE_RAID`)

> This feature requires the Pro edition.

```yaml
# Use a regional challenge raid
type: CHALLENGE_RAID

challenge:
  # Total challenge time limit, in seconds
  time_limit_seconds: 300

  # Total kills required to enter victory resolution or the Boss phase
  target_kills: 60

  # Maximum number of ordinary raid mobs alive at the same time
  max_alive_mobs: 18

  # Number of ordinary mobs planned for each spawning round
  spawn_count_per_round: 5

  # Interval between spawning rounds, in seconds
  spawn_period_seconds: 5

  # Minimum distance between spawn points and players inside the region
  min_spawn_distance: 8.0

  # Maximum distance between spawn points and players inside the region
  max_spawn_distance: 24.0

  # Whether PvP is disabled during the challenge
  pvp_disabled: true

  # Whether vanilla mob spawning is blocked inside the region
  block_vanilla_mobs: true

  # Whether players keep their inventory on death
  keep_inventory_on_death: false

  # Whether players keep their experience on death
  keep_experience_on_death: false

  # Whether remaining raid mobs are removed when the challenge ends
  cleanup_mobs_on_end: true

  mobs:
    # id: MythicMobs mob ID
    # probability: relative selection weight
    # chance, weight, and count are also accepted as weight keys
    - { id: EX_Walker, probability: 0.62 }
    - { id: EX_Rusher, probability: 0.23 }
    - { id: EX_Thrower, probability: 0.15 }

  boss:
    # MythicMobs Boss ID spawned after target_kills is reached
    # Without an id, reaching the target kill count results in immediate victory
    id: EX_King

    # Number of Bosses spawned
    count: 1

  # Console commands executed when the challenge is won or lost
  # Standard hooks and rewards still run through the normal settlement process
  victory_commands:
    - "say %region% challenge cleared"
  defeat_commands:
    - "say %region% challenge failed"
```

Challenge raid flow:

- Players inside the region are treated as participants.
- Spawn points are found dynamically around players who remain inside the region.
- BossBars, ActionBars, rankings, and scoreboards use a nearby audience range. Players who have just crossed the region boundary can still see raid information while nearby.
- Ordinary mobs are selected from `challenge.mobs` by relative weight.
- The system tracks total kills, personal kills, personal deaths, total deaths, and living mob counts.
- After `target_kills` is reached, a configured `boss.id` starts the Boss phase; otherwise, the raid ends in immediate victory.
- Defeat can result from the challenge timer, the global empty-region timeout, the global death limit, or forced defeat.
