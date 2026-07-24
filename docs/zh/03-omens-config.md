# 03. Omen 配置

Omen 定义位于 `plugins/CustomRaids/omens/` 下。内置示例是 `example_bad_omen.yml`。

## 文件结构

```yaml
omens:
  example_bad_omen:
    display_name: "&2Example Bad Omen"
    use_vanilla_bad_omen: true
    milk_clears_custom_omen: true
    consume_custom_omen: true
    consume_vanilla_bad_omen: true
    obtain_notify:
      mode: msg
      msg: "&a你获得了 %omen% Lv.%level%"
      actionbar: "&a%omen% Lv.%level%"
      title: "&2Omen Obtained"
      subtitle: "&a%omen% Lv.%level%"
      title_fade_in: 5
      title_stay: 40
      title_fade_out: 10
      sound:
        id: "minecraft:entity.experience_orb.pickup"
        volume: 1.0
        pitch: 1.0
    presets:
      - preset: example_raid
        weight: 100
        min_level: 1
        max_level: 5
```

`omens` 下每个子节点都是一个 omen id，命令和占位符都会使用该 id。

## Omen 字段

- `display_name`：彩色显示名。
- `use_vanilla_bad_omen`：允许原版 `BAD_OMEN` 效果触发该 omen。
- `milk_clears_custom_omen`：启用时，牛奶可清除自定义 omen 状态。
- `consume_custom_omen`：触发后清除自定义 omen。
- `consume_vanilla_bad_omen`：触发后移除原版 Bad Omen 效果。

## 获取提示

`obtain_notify.mode` 支持：

- `msg`
- `actionbar`
- `title`

提示可包含 `msg`、`actionbar`、`title`、`subtitle`、标题时间配置和可选音效。如果 omen 没有配置音效，则使用 `config.yml` 中的 `omen.default_obtain_sound`。

## 预设选择

`presets` 是带权重列表。每个条目使用：

- `preset`：袭击预设名。
- `weight`：相对选择权重。
- `min_level`：可选最低 omen 等级。
- `max_level`：可选最高 omen 等级。

触发 omen 时，`OmenManager` 会根据等级和权重选择一个匹配预设。

## 命令

Omen 由 `/raids omen` 管理：

- `/raids omen give <player|selector> <omenId> [level]`
- `/raids omen bottle <player|selector> <omenId> [level]`
- `/raids omen clear <player|selector>`
- `/raids omen info [player|selector]`

目标支持精确玩家名、`@a`、`@p`、`@r`、`@s` 和逗号分隔列表。

## 持久化

玩家当前自定义 omen 状态保存在 `plugins/CustomRaids/omen_state.yml`。
