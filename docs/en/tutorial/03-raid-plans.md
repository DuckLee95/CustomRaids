# Scheduled Raid Plan Tutorial

> This feature requires the Pro edition.

## Editing in the In-Game GUI

![Raid plan management menu](../../../images/raid-plan-config.png)

Use `/raids gui` to open the in-game management menu and create scheduled raid plans quickly.

## Editing Configuration Files

### File Location

Scheduled raid plans are loaded from `plugins/CustomRaids/raid-plans.yml`.

### Purpose

Raid plans automatically start configured raid presets during specified time windows.

The scheduler only targets regions containing online players. Regions without players are ignored.

For example, with a Saturday `20:00`–`22:00` window, the plugin starts a raid when a player is inside an eligible region during that period.

### Configuration Structure and Fields

```yaml
# Whether scheduled raid plans are enabled globally
enabled: true

# How often the scheduler checks plans, in seconds
check_interval_seconds: 30

# Plan definitions; every plan ID must be unique
plans:
  saturday_evening:
    # Whether this plan is enabled
    enabled: false

    # Region provider: Custom, Towny, Residence, or Lands
    provider: "Towny"

    # World in which this plan runs
    world: "world"

    # Raid preset started by this plan
    preset: "example_raid"

    # Time zone used by this plan
    # Use server for the server time zone, or a Java time-zone ID such as Asia/Shanghai
    timezone: "server"

    # Weekdays on which this plan may run
    # Supports full weekday names, abbreviations such as MON, and numeric values 1–7
    days:
      - SATURDAY

    # Active time window in HH:mm format
    # Windows may cross midnight, for example 22:00–02:00
    time:
      # Window start time
      start: "20:00"

      # Window end time
      end: "22:00"

    # Whether this plan may start only once per time window
    # true prevents repeated starts; false allows an attempt on every scheduler check
    once_per_window: true
```

## Troubleshooting

### The Raid Does Not Start

- Global `enabled` or the plan's own `enabled` is `false`.
- No player is inside a matching region during the time window.
- `world`, `provider`, or `preset` is configured incorrectly.
- The target region already has an active raid.

### The Raid Starts Only Once

- `once_per_window: true` is enabled.
- Wait for the next matching time window or set it to `false`.
