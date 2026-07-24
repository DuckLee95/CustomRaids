# PlaceholderAPI Placeholders

When PlaceholderAPI is installed, CustomRaids registers the `customraids` expansion.

The placeholder format is `%customraids_<name>%`.

## Omen Placeholders

| Placeholder | Purpose / Return Value | When No Data Is Available |
| --- | --- | --- |
| `%customraids_omen_id%` | Current Omen ID. | Empty string |
| `%customraids_omen_name%` | Current Omen display name. | Empty string |
| `%customraids_omen_level%` | Current Omen level. | `0` |
| `%customraids_omen_source%` | Current Omen source. | `none` |
| `%customraids_omen_has%` | Whether the player has an Omen; returns `true` or `false`. | `false` |
| `%customraids_omen_next_preset%` | Preset selected from the current Omen by weight. | Empty string |

## Raid Placeholders

These placeholders resolve from the active raid in the player's current region:

| Placeholder | Purpose / Return Value | When No Raid Is Active |
| --- | --- | --- |
| `%customraids_raid_active%` | Whether the current region has an active raid; returns `true` or `false`. | `false` |
| `%customraids_raid_name%` | Current raid preset name. | Empty string |
| `%customraids_raid_region%` | Current raid region name. | Empty string |
| `%customraids_raid_state%` | Current `RaidState`. | `NONE` |
| `%customraids_raid_type%` | Current raid type name. | Empty string |
| `%customraids_raid_wave%` | Current wave. | `0` |
| `%customraids_raid_waves%` | Total number of waves. | `0` |
| `%customraids_raid_alive%` | Number of currently tracked living mobs. | `0` |
| `%customraids_raid_countdown%` | Raid countdown in seconds. | `0` |
| `%customraids_raid_wave_remain%` | Seconds remaining in the current wave. | `0` |
| `%customraids_raid_remain%` | Seconds remaining in the entire raid. | `0` |

## Challenge Raid Placeholders

| Placeholder | Purpose / Return Value | When No Applicable Raid Exists |
| --- | --- | --- |
| `%customraids_raid_kills%` | Total challenge kills. | `0` |
| `%customraids_raid_target_kills%` | Target challenge kill count. | `0` |
| `%customraids_raid_phase%` | Localized challenge phase text from the current language file. | Empty string |
| `%customraids_raid_phase_raw%` | Stable challenge phase key: `kills`, `boss`, or `waves`. | Empty string |
| `%customraids_raid_deaths%` | Total recorded player deaths. | `0` |
| `%customraids_raid_personal_kills%` | Current player's challenge kills. | `0` |
| `%customraids_raid_personal_deaths%` | Current player's challenge deaths. | `0` |

## Resolution Notes

The expansion requires player context. If PlaceholderAPI requests a placeholder without a player, the current implementation returns an empty string.
