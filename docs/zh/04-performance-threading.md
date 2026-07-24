# 04. 性能与线程

CustomRaids 面向 Paper 风格服务端编写，并在 `plugin.yml` 中声明支持 Folia。运行期调度集中通过 `FoliaScheduler` 处理。

## 调度模型

代码通过调度工具处理：

- 绑定实体的任务
- 绑定区域的任务
- 全局延迟或循环任务

这对 Folia 很重要，因为实体和区域操作需要在正确的执行上下文中运行。

## 刷怪计算

刷怪位置由 `SpawnPointPlanner` 选择。

波次袭击使用 `global.spawn.strategy`：

- `random_in_region`
- `near_players`
- `ring`

挑战袭击通过 `pickAroundPlayers` 围绕玩家动态选点。开启安全检查时，会检查区域边界、区块加载、脚下方块、头部空间、流体和部分危险方块。

影响性能的主要配置：

- `global.spawn.tries_per_point`
- `global.spawn.min_dist_between`
- `global.spawn.min_dist_from_players`
- `challenge.spawn_count_per_round`
- `challenge.spawn_period_seconds`
- `challenge.max_alive_mobs`
- `challenge.min_spawn_distance`
- `challenge.max_spawn_distance`

较高的刷怪数量、较短的刷怪周期或较高的 `tries_per_point` 会增加 CPU 搜索开销。

## 显示更新

BossBar、ActionBar、排行和计分板更新属于活跃袭击 tick 的一部分。全局默认排行数量是 `defaults.ranking.max_entries`，单个预设可覆盖排行和计分板条目数量。

显示受众使用 `defaults.audience.range` 和 `defaults.audience.vertical_range`。挑战袭击刷怪仍只围绕当前区域内玩家，但处于配置显示范围内的附近玩家也能看到活跃袭击信息。

## 清理与状态

`RaidInstance` 会追踪袭击怪物 UUID，并在运行过程中同步场上怪物状态。MythicMobs 桥接层会尽量将袭击怪物设为持久实体，运行实例会为已追踪怪物所在区块保留插件 chunk ticket，减少玩家死亡传送或移动后区块卸载导致怪物消失并卡住袭击的情况。挑战袭击在 `challenge.cleanup_mobs_on_end` 为 true 时，会在结束时清理追踪到的挑战怪物。

活跃袭击快照由 `ActiveRaidStateStore` 保存到 `active_raids.yml`。玩家 omen 状态由 `OmenStateStore` 保存到 `omen_state.yml`。

## 配置建议

- 根据区域大小和服务器承载能力设置 `challenge.max_alive_mobs` 和 `spawn_count_per_round`。
- 除非明确需要更宽松的生成规则，建议保持 `safe_check: true`。
- 排行和计分板条目数量保持适中。
- 如果挑战刷怪选点过于频繁，适当提高 `spawn_period_seconds`。
