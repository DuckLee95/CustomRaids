# 02. 襲击预设配置

袭击预设是 `plugins/CustomRaids/raids/` 下的 YAML 文件。内置示例来自 `src/main/resources/raids/`。

## 顶层结构

```yaml
name: example_raid
type: WAVE_RAID # 可选；默认 WAVE_RAID，也支持 CHALLENGE_RAID
raid_type: WAVE_RAID # 可选兼容别名

global:
  temp_flags: {}
  temp_flags_ttl_ticks: 2400
  prepare_between_seconds: 10
  time_limit_seconds: 0
  failures:
    fail_after_empty_seconds: 0
    max_deaths: 0
  mob_glow:
    enabled: false
    interval_seconds: 8
    duration_seconds: 5
  spawn:
    strategy: random_in_region
    tries_per_point: 60
    min_dist_between: 3.0
    min_dist_from_players: 6.0
    center_on_block: true
    ring_radius: 10.0
    safe_check: true

hooks: {}
display: {}
rewards: {}
waves: []
challenge: {}
```

`global` 必须存在。`WAVE_RAID` 必须有至少一个有效 `waves` 条目。`CHALLENGE_RAID` 必须有至少一个有效 `challenge.mobs` 条目。解析器接受 `type`、`raid_type`、`raid-type` 或 `mode`；如果没有波次列表且存在 `challenge.mobs`，会按挑战袭击处理。

## global 配置

- `temp_flags`：袭击期间对区域临时设置的 flag。示例里常见 `monsters`、`explode`、`pvp`，实际效果取决于区域提供器。
- `temp_flags_ttl_ticks`：临时 flag 持续 tick。
- `prepare_between_seconds`：波次默认准备时间。
- `time_limit_seconds`：整场袭击时间限制；波次袭击中 `0` 表示不限时。
- `failures.fail_after_empty_seconds`：大于 `0` 时，区域内无人持续达到该秒数后袭击失败。当前解析为所有袭击类型的全局失败条件。
- `failures.max_deaths`：大于 `0` 时，记录到的玩家死亡次数达到该值后袭击失败。当前解析为所有袭击类型的全局失败条件。
- `mob_glow.enabled`：是否周期性让场上袭击怪物发光。
- `mob_glow.interval_seconds`：发光刷新周期。
- `mob_glow.duration_seconds`：每次发光持续时间。

失败条件还兼容部分旧键名：`empty_region_seconds`、`empty_seconds`、`death_limit`。

## 刷怪配置

- `strategy`：`random_in_region`、`near_players`、`ring`。
- `tries_per_point`：每个刷怪点的尝试次数。
- `min_dist_between`：计划刷怪点之间的最小距离。
- `min_dist_from_players`：波次刷怪点距离玩家的最小距离。
- `center_on_block`：是否放到方块中心。
- `ring_radius`：`ring` 策略使用的半径。
- `safe_check`：检查已加载区块、区域包含、实体脚下方块、头部空间、流体和部分危险方块。`check_safe_block` 也是可接受别名。

挑战袭击使用 `SpawnPointPlanner.pickAroundPlayers`，根据挑战距离配置围绕当前仍在区域内的玩家动态寻找合法刷怪点。

## hooks

支持的 hook：

- `hooks.start`
- `hooks.wave_cleared`
- `hooks.victory`
- `hooks.defeat`
- `hooks.defeat_empty_region`
- `hooks.defeat_timeout`
- `hooks.defeat_max_deaths`

`hooks.defeat_empty_region`、`hooks.defeat_timeout`、`hooks.defeat_max_deaths` 适用于所有袭击类型。未配置具体失败原因 hook 时，会回退使用 `hooks.defeat`。

兼容别名也会被读取：`defeat_empty`、`defeat_leave`、`defeat_leave_region`、`defeat_time_limit`、`defeat_time`、`defeat_deaths`、`defeat_death_limit`。

每个 hook 可包含：

```yaml
scope: server # server 或 region
chat: "&a包含 %region% 的消息"
sound:
  id: "minecraft:entity.player.levelup"
  volume: 1.0
  pitch: 1.0
title:
  main: "&a标题"
  sub: "&7副标题"
  fadein: 10
  stay: 40
  fadeout: 10
```

## display 配置

```yaml
display:
  bossbar:
    color: RED
    style: SEGMENTED_10
    countdown_title: "&e%region% %countdown%s"
    in_progress_title: "&c%region% Alive: %alive%"
    preparing_title: "&e%region%"
    victory_title: "&a%region%"
    defeat_title: "&c%region%"
  actionbar:
    countdown: "&eWave %wave%/%waves% starts in %countdown%s"
    in_progress_timed: "&cAlive %alive% | Wave left %remain%s"
    in_progress_raid_timed: "&cAlive %alive% | Raid left %remain%s"
    in_progress_unlimited: "&cAlive %alive%"
  ranking:
    header: "&6Damage Ranking"
    line: "&e#%rank% &f%player% &c%damage%"
    empty: "&7No damage recorded."
    max_entries: 5
  scoreboard:
    enabled: true
    max_entries: 5
  titles:
    wave_title_prefix: "&eWave"
    wave_subtitle: "&7%wave%/%waves%"
```

计分板显示走当前兼容层。TAB 或 ProtocolLib 存在时可能使用对应实现，否则使用空实现。

## rewards

```yaml
rewards:
  participation_commands:
    - "say %player% participated"
  top1_commands:
    - "say TOP1 %player%"
  top2_commands:
    - "say TOP2 %player%"
  top3_commands:
    - "say TOP3 %player%"
```

`rewards.commands` 作为旧版参与奖励命令列表兼容。前三名奖励按伤害排行发放。

常见奖励占位符：

- `%player%`
- `%rank%`
- `%damage%`
- `%region%`
- `%preset%`
- `%participants%`
- 失败相关：`%defeat_reason%`、`%defeat_reason_raw%`、`%fail_after_empty_seconds%`、`%empty_seconds%`、`%time_limit%`、`%total_deaths%`、`%max_deaths%`
- 挑战袭击相关：`%kills%`、`%deaths%`、`%total_kills%`、`%target_kills%`、`%phase%`、`%phase_raw%`

`%phase%` 使用 `raid.display.challenge.phase.*` 语言键；`%phase_raw%` 保留稳定原始键。

## 波次型袭击

```yaml
type: WAVE_RAID

waves:
  - title: "&eWave 1"
    prepare_seconds: 5
    time_limit_seconds: 0
    sound:
      id: "minecraft:raid.horn"
      volume: 1.0
      pitch: 1.0
    mobs:
      - { id: EX_Walker, count: 6 }
      - { id: EX_Thrower, count: 2 }
```

波次怪物使用 MythicMobs id 和整数数量。当前波所有已追踪怪物不再存活时，该波被视为清理完成。

## 区域挑战型袭击

```yaml
type: CHALLENGE_RAID

challenge:
  time_limit_seconds: 300
  target_kills: 60
  max_alive_mobs: 18
  spawn_count_per_round: 5
  spawn_period_seconds: 5
  min_spawn_distance: 8.0
  max_spawn_distance: 24.0
  pvp_disabled: true
  block_vanilla_mobs: true
  keep_inventory_on_death: false
  keep_experience_on_death: false
  cleanup_mobs_on_end: true
  mobs:
    - { id: EX_Walker, probability: 0.62 }
    - { id: EX_Rusher, probability: 0.23 }
    - { id: EX_Thrower, probability: 0.15 }
  boss:
    id: EX_King
    count: 1
  victory_commands:
    - "say %region% challenge cleared"
  defeat_commands:
    - "say %region% challenge failed"
```

挑战怪物条目使用相对概率。解析器接受 `probability`、`chance`、`weight`、`count` 作为权重键。

当前代码中的挑战流程：

- 区域内玩家会被视为参与者。
- 刷怪点围绕当前仍在区域内的玩家动态寻找。
- BossBar、ActionBar、排行和计分板显示使用区域附近受众范围，所以玩家刚离开区域边界但仍在附近时仍能看到袭击信息。
- 普通怪从 `challenge.mobs` 按相对概率选择。
- 记录总击杀、个人击杀、个人死亡、总死亡和场上怪物数量。
- 达到 `target_kills` 后，如果配置了 `boss.id` 则进入 Boss 阶段。
- 没有 Boss 时，达到 `target_kills` 直接胜利。
- 失败可来自挑战时间结束、全局无人超时、全局死亡次数上限或强制失败。
- `victory_commands` 和 `defeat_commands` 是挑战袭击自己的结算命令；普通 hook 和奖励仍会按结算流程执行。
