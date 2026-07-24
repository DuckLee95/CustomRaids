# PlaceholderAPI 占位符

安装 PlaceholderAPI 时，CustomRaids 会注册 `customraids` 扩展。

占位符格式为 `%customraids_<name>%`。

## Omen 占位符

| 占位符 | 用途 / 返回值 | 无数据时 |
| --- | --- | --- |
| `%customraids_omen_id%` | 当前 Omen ID。 | 空字符串 |
| `%customraids_omen_name%` | 当前 Omen 显示名。 | 空字符串 |
| `%customraids_omen_level%` | 当前 Omen 等级。 | `0` |
| `%customraids_omen_source%` | 当前 Omen 来源。 | `none` |
| `%customraids_omen_has%` | 是否拥有 Omen，返回 `true` 或 `false`。 | `false` |
| `%customraids_omen_next_preset%` | 当前 Omen 按权重解析出的预设名。 | 空字符串 |

## 襲击占位符

这些占位符从玩家当前所在区域的活跃袭击中解析：

| 占位符 | 用途 / 返回值 | 无活跃袭击时 |
| --- | --- | --- |
| `%customraids_raid_active%` | 当前区域是否有活跃袭击，返回 `true` 或 `false`。 | `false` |
| `%customraids_raid_name%` | 当前袭击的预设名。 | 空字符串 |
| `%customraids_raid_region%` | 当前袭击的区域名。 | 空字符串 |
| `%customraids_raid_state%` | 当前 `RaidState`。 | `NONE` |
| `%customraids_raid_type%` | 当前袭击的类型名。 | 空字符串 |
| `%customraids_raid_wave%` | 当前波次。 | `0` |
| `%customraids_raid_waves%` | 总波次数。 | `0` |
| `%customraids_raid_alive%` | 当前追踪的存活怪物数。 | `0` |
| `%customraids_raid_countdown%` | 袭击倒计时秒数。 | `0` |
| `%customraids_raid_wave_remain%` | 当前波剩余秒数。 | `0` |
| `%customraids_raid_remain%` | 整场袭击剩余秒数。 | `0` |

## 挑战袭击占位符

| 占位符 | 用途 / 返回值 | 无适用袭击时 |
| --- | --- | --- |
| `%customraids_raid_kills%` | 挑战总击杀数。 | `0` |
| `%customraids_raid_target_kills%` | 挑战目标击杀数。 | `0` |
| `%customraids_raid_phase%` | 当前语言文件中的挑战阶段显示文本。 | 空字符串 |
| `%customraids_raid_phase_raw%` | 稳定的挑战阶段键，可能为 `kills`、`boss` 或 `waves`。 | 空字符串 |
| `%customraids_raid_deaths%` | 已记录的玩家总死亡次数。 | `0` |
| `%customraids_raid_personal_kills%` | 当前玩家的挑战击杀数。 | `0` |
| `%customraids_raid_personal_deaths%` | 当前玩家的挑战死亡次数。 | `0` |

## 解析说明

当前扩展需要玩家上下文。如果 PlaceholderAPI 在没有玩家的情况下请求占位符，当前实现返回空字符串。
