# 07. PlaceholderAPI 占位符

安装 PlaceholderAPI 时，CustomRaids 会注册 `customraids` 扩展。

占位符格式为 `%customraids_<name>%`。

## Omen 占位符

- `%customraids_omen_id%`：当前 omen id，没有则为空。
- `%customraids_omen_name%`：当前 omen 显示名，没有则为空。
- `%customraids_omen_level%`：当前 omen 等级，没有则为 `0`。
- `%customraids_omen_source%`：omen 来源，没有则为 `none`。
- `%customraids_omen_has%`：`true` 或 `false`。
- `%customraids_omen_next_preset%`：当前 omen 按权重解析出的预设名，没有则为空。

## 襲击占位符

这些占位符从玩家当前所在区域的活跃袭击中解析：

- `%customraids_raid_active%`：`true` 或 `false`。
- `%customraids_raid_name%`：预设名。
- `%customraids_raid_region%`：区域名。
- `%customraids_raid_state%`：当前 `RaidState`，没有则为 `NONE`。
- `%customraids_raid_type%`：袭击类型名。
- `%customraids_raid_wave%`：当前波次，没有则为 `0`。
- `%customraids_raid_waves%`：总波次数，没有则为 `0`。
- `%customraids_raid_alive%`：当前追踪的存活怪物数，没有则为 `0`。
- `%customraids_raid_countdown%`：倒计时秒数，没有则为 `0`。
- `%customraids_raid_wave_remain%`：当前波剩余秒数，没有则为 `0`。
- `%customraids_raid_remain%`：整场袭击剩余秒数，没有则为 `0`。

## 挑战袭击占位符

没有适用袭击时，这些占位符返回 `0` 或空字符串：

- `%customraids_raid_kills%`：挑战总击杀数。
- `%customraids_raid_target_kills%`：挑战目标击杀数。
- `%customraids_raid_phase%`：当前语言文件中的挑战阶段显示文本。
- `%customraids_raid_phase_raw%`：稳定的挑战阶段键，当前可能是 `kills`、`boss` 或 `waves`。
- `%customraids_raid_deaths%`：已记录玩家总死亡次数。
- `%customraids_raid_personal_kills%`：当前玩家挑战击杀数。
- `%customraids_raid_personal_deaths%`：当前玩家挑战死亡次数。

## 解析说明

当前扩展需要玩家上下文。如果 PlaceholderAPI 在没有玩家的情况下请求占位符，当前实现返回空字符串。
