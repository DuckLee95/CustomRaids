# 06. config.yml

`config.yml` 保存插件全局默认配置，会复制到 `plugins/CustomRaids/config.yml`。

当前内置结构：

```yaml
defaults:
  language: auto
  bstats:
    enabled: true
  audience:
    range: 24.0
    vertical_range: 24.0
  bossbar:
    hide_delay_ticks: 60
  ranking:
    max_entries: 5

omen:
  default_obtain_sound:
    id: "minecraft:entity.experience_orb.pickup"
    volume: 1.0
    pitch: 1.0
```

## defaults

- `defaults.language`：`auto`、`en_US` 或 `zh_CN`。
- `defaults.bstats.enabled`：是否启用 bStats。
- `defaults.audience.range`：附近受众的水平范围。
- `defaults.audience.vertical_range`：附近受众的垂直范围。
- `defaults.bossbar.hide_delay_ticks`：结算 BossBar 隐藏延迟。
- `defaults.ranking.max_entries`：默认排行最大条目数。

## omen 默认值

`omen.default_obtain_sound` 是 omen 获取提示的默认音效。将 `id` 设置为 `"none"` 可禁用默认音效。

## 相关文件

其他配置文件是独立的：

- `raids/*.yml`：袭击预设。
- `omens/*.yml`：omen 定义。
- `raid-plans.yml`：定时袭击计划。
- `custom_regions.yml`：生成的自定义区域。
