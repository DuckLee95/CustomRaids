# 示例包使用指南

示例包提供一套用于本地测试的配置，覆盖以下主要功能：

- 波次袭击
- 区域挑战袭击
- 自定义 Omen
- 袭击计划
- Custom 区域
- MythicMobs 示例怪物
- 自定义 Omen 掉落
- 自定义信息显示
- 袭击奖励、失败条件和结算命令

## 目录结构

- `examples/config.yml`
  - CustomRaids 全局配置示例。
- `examples/custom_regions.example.yml`
  - 内置 Custom 区域数据文件示例。
- `examples/raid-plans.yml`
  - 定时袭击计划示例。
- `examples/raids/`
  - `example_border_patrol.yml`：使用 `near_players` 策略的波次袭击。
  - `example_siege_ring.yml`：使用 `ring` 策略的波次袭击。
  - `example_last_stand.yml`：使用 `random_in_region` 策略的高压波次袭击。
  - `example_challenge_trial.yml`：`CHALLENGE_RAID` 区域挑战袭击。
- `examples/omens/`
  - `example_omens_pack.yml`：多 Omen 路由、通知模式和等级权重。
- `examples/mythicmobs/`
  - `ExampleMobs_AllFeatures.yml`：测试用 MythicMobs 怪物。
  - `ExampleDropTables_AllFeatures.yml`：测试用 Omen 掉落表。

## 安装到服务器

1. 将 `examples/config.yml` 复制到 `plugins/CustomRaids/config.yml`。
2. 将 `examples/raids/*.yml` 复制到 `plugins/CustomRaids/raids/`。
3. 将 `examples/omens/*.yml` 复制到 `plugins/CustomRaids/omens/`。
4. 将 `examples/raid-plans.yml` 复制到 `plugins/CustomRaids/raid-plans.yml`。
5. 可选：将 `examples/custom_regions.example.yml` 复制到 `plugins/CustomRaids/custom_regions.yml`。
   - 复制前，请确认文件中的 `world` 和坐标适用于测试服务器。
6. 安装 MythicMobs 示例：
   - 将 `examples/mythicmobs/ExampleMobs_AllFeatures.yml` 复制到 `plugins/MythicMobs/Mobs/`。
   - 将 `examples/mythicmobs/ExampleDropTables_AllFeatures.yml` 复制到 `plugins/MythicMobs/DropTables/`。
7. 依次执行 `/mm reload` 和 `/raids reload`。

## 推荐测试流程

1. 创建 Custom 区域：
   - `/raids region pos1`
   - `/raids region pos2`
   - `/raids region create test_arena`
2. 启动波次袭击：
   - `/raids startpreset border_patrol test_arena`
   - `/raids startpreset siege_ring test_arena`
   - `/raids startpreset last_stand test_arena`
3. 启动挑战袭击：
   - `/raids startpreset challenge_trial test_arena`
4. 测试强制控制：
   - `/raids force skipwave test_arena`
   - `/raids force victory test_arena`
   - `/raids force defeat test_arena`
5. 测试 Omen：
   - `/raids omen give @s omen_patrol_msg 1`
   - `/raids omen give @s omen_challenge_title 2`
   - `/raids omen bottle @s omen_laststand_title 3`
   - `/raids omen info @s`
   - `/raids omen clear @s`
6. 测试 MythicMobs Omen 掉落：
   - 生成或击杀 `CR_OmenDrop_Messenger`。
   - 生成或击杀 `CR_OmenDrop_Harbinger`。
7. 测试定时计划：
   - 将 `raid-plans.yml` 中的 `world`、`days` 和 `time` 修改为当前环境及时间窗口。
   - 让玩家停留在对应的 provider 区域内，等待计划检查触发。

## 功能覆盖

- `WAVE_RAID` 和 `CHALLENGE_RAID`
- `near_players`、`ring` 和 `random_in_region` 刷怪策略
- 挑战袭击动态刷怪、怪物池权重、目标击杀和 Boss 阶段
- PvP 禁用、原版怪物阻止、死亡物品及经验保留、结束清理
- 无人区域超时和死亡次数上限等全局失败条件
- 整场袭击限时和单波限时
- `temp_flags` 和 TTL
- `safe_check` 安全刷怪检查
- `mob_glow` 怪物高亮
- Hook 聊天、音效和标题
- BossBar、ActionBar、伤害排行和计分板
- 参与奖励及 TOP 1、TOP 2、TOP 3 奖励命令
- 挑战袭击胜利和失败命令
- Omen 状态、原版 Bad Omen 触发、牛奶清除、触发后消耗、通知模式、默认音效、等级范围和权重选择
- PlaceholderAPI 的 `%customraids_...%` 占位符

## 注意事项

- Hook 的 `scope: region` 会将消息发送给区域附近的玩家；其他值按全服广播处理。因此，示例只使用 `region` 或 `server`。
- 示例奖励命令使用 `say`，便于在控制台观察结果。正式服务器应替换为经济、物品或权限命令。
- 不建议让普通袭击怪物掉落 Omen，以免连续触发袭击。示例包单独提供 `CR_OmenDrop_*` 怪物用于测试 Omen 掉落。
