# 更新日志

## 1.2.0（2026-07-20）

### 新增功能

- 新增挑战试炼袭击（`CHALLENGE_RAID`，进阶版专属）：
  - 支持配置加权怪物池，袭击期间会围绕区域内玩家动态生成 MythicMobs 怪物。
  - 支持统计目标击杀数、个人击杀数和玩家死亡数。
  - 支持击杀阶段、Boss 阶段和波次兼容显示。
  - 支持挑战胜利及失败命令、奖励结算和伤害排名。
- 新增挑战袭击 GUI 编辑器（进阶版专属）：
  - 支持在游戏内创建 `CHALLENGE_RAID` 预设。
  - 支持编辑挑战参数、加权怪物池、Boss 配置及胜利、失败命令。
  - 支持切换 `WAVE_RAID` 和 `CHALLENGE_RAID` 类型。
- 新增领地指定刷怪区域功能：
  - `/raids region setrange`：为支持的区域单独设置袭击刷怪范围。
  - `/raids region viewrange`：查看当前区域的刷怪范围。
  - `/raids region clearrange`：清除当前区域的刷怪范围。
  - 支持 Custom、Lands、Residence 和 Towny 区域。
  - 刷怪范围只限制刷怪位置，不影响公告、BossBar、ActionBar、Title 和计分板的显示范围。
  - 新增运行时数据文件 `raid_ranges.yml`。

### 功能改进

- 新增失败原因 Hook：`defeat_empty_region`、`defeat_timeout`、`defeat_max_deaths`。
  - 未配置具体失败原因 Hook 时，会回退到 `defeat`。
  - 新增失败原因占位符：`%defeat_reason%`、`%defeat_reason_raw%`、`%fail_after_empty_seconds%`、`%empty_seconds%`、`%time_limit%`。
- 增强占位符系统：
  - 新增挑战阶段占位符 `%phase_raw%`。
  - 新增 PlaceholderAPI 占位符 `%customraids_raid_phase_raw%`。
  - `%phase%` 使用语言文件中的阶段文本。
- 完善公共 API：
  - 新增外部区域提供器支持。
  - 支持外部插件通过 API 触发袭击。
  - `CustomRaidHandle` 和 `CustomRaidFinishEvent` 增加挑战袭击及失败原因信息。
- 完善袭击计划：
  - 支持每周指定时间段开启袭击活动。
  - 改进袭击计划 GUI 的编辑体验。
  - 袭击计划调整为进阶版功能。
- 优化 Folia 和计分板兼容性：
  - Folia 服务端通过 ProtocolLib API 显示计分板。
  - 优化 TAB、ProtocolLib 和无兼容插件时的计分板回退逻辑。

### 问题修复

- 修复挑战袭击开始 Hook 中 `%target_kills%` 等挑战占位符不生效的问题。
- 修复玩家离开区域边界后不再显示 BossBar、ActionBar 或计分板的问题。
- 修复玩家死亡传送或区块卸载后，袭击怪物可能消失并导致袭击卡住的问题。
- 运行中的袭击怪物会被持续追踪，并保持其所在区块加载。

### 升级说明

- 更新中文和英文帮助文档。
- 更新袭击配置、袭击计划、API、PlaceholderAPI 占位符和命令说明。
- 新增或更新挑战袭击示例、完整配置及 MythicMobs 示例。

> 此版本新增了语言文件字段、挑战袭击示例和 `raid_ranges.yml`。升级前请备份旧配置，并对照新文件合并配置。

## 1.1.0（2026-05-24）

- 新增 Towny 支持。
- 新增 Folia 支持。Folia 服务端需要安装 ProtocolLib 才能显示计分板，其他服务端无需安装。
- 新增袭击计划功能（进阶版）：
  - 支持按星期、时间段、世界、区域提供器和袭击预设创建定时袭击活动。
  - 只袭击有玩家在线停留的区域。
  - 新增默认配置文件 `raid-plans.yml`。
- 新增袭击计划 GUI 编辑器（进阶版），支持创建、删除、启用、禁用和编辑袭击计划。
- 优化命令系统。
- 将 MythicMobs 设为必需依赖。
- 修复袭击结束时，部分 MythicMobs 自带血条未被清除的问题。

## 1.0.6（2026-05-16）

- 添加 bStats 匿名数据统计。
- 修复插件版本号显示错误的问题。

## 1.0.5（2026-05-09）

- 完善 Omen GUI 配置。

## 1.0.4（2026-05-09）

- Omen 配置新增 `milk_clears_custom_omen`，用于控制牛奶能否清除自定义 Omen。
- 更新帮助文档和示例配置。

## 1.0.3（2026-05-08）

- 增强 `/raids stopall`，执行后会清除对应袭击生成的怪物。
- 袭击失败时自动清除剩余怪物。
- 新增 `/raids omen bottle <player|selector> <omenId> [level]`，用于发放绑定自定义 Omen 的药水。
- 低于 1.21.5 的服务端使用普通药水替代 Omen 药水。
- Omen 详情页新增“给予征兆药水”按钮。
- 改进 TAB 计分板适配：
  - 引入 TAB API 依赖。
  - 显示袭击排行时临时关闭 TAB 计分板，袭击结束后恢复，以缓解 Sidebar 冲突和闪烁。
- 增强 Omen 药水：
  - 支持通过 `omen.bottle_item_name` 自定义显示名。
  - 支持通过 `omen.bottle_lore` 配置 Lore。
  - 饮用后按自定义 Omen 逻辑生效。

> 此版本新增了语言文件字段。升级时请删除旧语言文件并重新生成。

## 1.0.1（2026-05-03）

- 改进袭击怪物的生成逻辑并优化性能。
