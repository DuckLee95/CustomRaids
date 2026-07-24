# 全局配置

`config.yml` 保存插件全局默认配置，会复制到 `plugins/CustomRaids/config.yml`。

## 配置结构

```yaml
defaults:
  # 语言：auto、en_US 或 zh_CN
  language: auto

  bstats:
    # 是否启用 bStats 匿名数据统计
    enabled: true

  audience:
    # 附近受众的水平和垂直范围
    range: 24.0
    vertical_range: 24.0

  bossbar:
    # 结算 BossBar 的隐藏延迟，单位为 tick
    hide_delay_ticks: 60

  ranking:
    # 默认排行的最大条目数
    max_entries: 5

omen:
  # Omen 获取提示的默认音效
  # 将 id 设为 "none" 可禁用默认音效
  default_obtain_sound:
    id: "minecraft:entity.experience_orb.pickup"
    volume: 1.0
    pitch: 1.0
```

## 相关文件

其他配置文件是独立的：

- `raids/*.yml`：袭击预设。
- `omens/*.yml`：Omen 定义。
- `raid-plans.yml`：定时袭击计划。
- `custom_regions.yml`：生成的自定义区域。
