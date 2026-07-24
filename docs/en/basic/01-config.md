# Global Configuration

`config.yml` stores the plugin's global defaults and is copied to `plugins/CustomRaids/config.yml`.

## Configuration Structure

```yaml
defaults:
  # Language: auto, en_US, or zh_CN
  language: auto

  bstats:
    # Whether anonymous bStats metrics are enabled
    enabled: true

  audience:
    # Horizontal and vertical ranges for nearby audiences
    range: 24.0
    vertical_range: 24.0

  bossbar:
    # Delay before hiding the result BossBar, in ticks
    hide_delay_ticks: 60

  ranking:
    # Default maximum number of ranking entries
    max_entries: 5

omen:
  # Default sound played when an Omen is obtained
  # Set id to "none" to disable the default sound
  default_obtain_sound:
    id: "minecraft:entity.experience_orb.pickup"
    volume: 1.0
    pitch: 1.0
```

## Related Files

Other configuration files are independent:

- `raids/*.yml`: Raid presets.
- `omens/*.yml`: Omen definitions.
- `raid-plans.yml`: Scheduled raid plans.
- `custom_regions.yml`: Generated custom regions.
