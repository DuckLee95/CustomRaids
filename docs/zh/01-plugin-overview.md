# 01. 插件概览

CustomRaids 用于在指定 Minecraft 区域内运行可配置袭击。袭击怪物由 MythicMobs 生成，区域来源可以是内置 Custom 区域，也可以是可选的 Lands、Residence、Towny 集成。

## 运行要求

- Java 21。
- Paper 兼容服务端，目标为 Minecraft 1.20.x。
- 必需插件：MythicMobs。
- 可选插件：Lands、Residence、Towny、PlaceholderAPI、TAB、ProtocolLib。
- `plugin.yml` 声明支持 Folia，运行时调度通过 `FoliaScheduler` 处理。

## 襲击类型

`WAVE_RAID` 是默认袭击类型。它按照配置的波次推进。每一波可以配置标题、准备时间、时间限制、音效，以及 MythicMobs 怪物 id 和数量。所有波次清理完成后胜利，触发配置的失败条件时失败。

`CHALLENGE_RAID` 是当前代码中已经存在的区域挑战型袭击 **（需要 CustomRaids Pro）**。它不按波次推进。区域内玩家会成为参与者，插件会围绕当前仍在区域内的玩家动态寻找刷怪点，从带权重的怪物池中选择普通怪生成，记录总击杀、个人击杀和死亡次数。普通怪击杀达到目标后，如果配置了 Boss 则进入 Boss 阶段；没有 Boss 时直接胜利。挑战袭击的显示信息使用与波次袭击一致的区域附近受众范围，但刷怪仍只围绕当前区域内玩家。

## 区域提供器

插件会在可用时注册以下区域提供器：

- `Custom`：内置长方体区域，通过 `/raids region` 管理。
- `Lands`：可选。
- `Residence`：可选。
- `Towny`：可选。

同一个 provider/region 组合同时只能运行一个袭击。

## 启动方式

当前代码支持以下启动方式：

- `/raids startpreset <name> [customRegion]`
- omen 触发：带有配置 omen 的玩家在支持的区域内触发袭击
- `raid-plans.yml` 定时计划
- 公共 API

## 管理 GUI

`/raids gui` 打开基于背包界面的管理员编辑器。支持创建、复制、删除袭击预设（`WAVE_RAID` 和 `CHALLENGE_RAID`），编辑波次、挑战设置、加权怪物池、Boss 配置、胜利/失败指令、奖励、显示模板、征兆和定时袭击计划。

## 显示与奖励

当前显示能力包括：

- 聊天、标题、音效 hook
- 空区域、超时、死亡次数上限对应的失败原因 hook
- BossBar
- ActionBar
- 伤害排行文本
- 启用时的右侧计分板

奖励以命令列表形式配置：

- `participation_commands`
- `top1_commands`
- `top2_commands`
- `top3_commands`

前三名奖励基于伤害排行。

## 运行期数据

插件会在 `plugins/CustomRaids/` 下保存运行期数据：

- `active_raids.yml`：活跃袭击快照
- `omen_state.yml`：玩家 omen 状态
- `custom_regions.yml`：内置 Custom 区域
