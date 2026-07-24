# 09. 定时袭击计划

定时袭击计划从 `plugins/CustomRaids/raid-plans.yml` 加载。

内置结构：

```yaml
enabled: true
check_interval_seconds: 30

plans:
  saturday_evening:
    enabled: false
    provider: "Towny"
    world: "world"
    preset: "example_raid"
    timezone: "server"
    days:
      - SATURDAY
    time:
      start: "20:00"
      end: "22:00"
    once_per_window: true
```

## 根节点

- `enabled`：是否启用计划调度器。
- `check_interval_seconds`：计划检查周期。
- `plans`：计划 id 到计划配置的映射。

## 单个计划

- `enabled`：是否启用该计划。
- `provider`：区域提供器名称，例如 `Custom`、`Towny`、`Residence`、`Lands`。
- `world`：世界名。
- `preset`：袭击预设名。
- `timezone`：`server` 或 Java 时区 id。
- `days`：允许的星期列表。
- `time.start`：窗口开始时间，格式 `HH:mm`。
- `time.end`：窗口结束时间，格式 `HH:mm`。
- `once_per_window`：为 true 时，同一窗口内避免重复启动。

## 星期格式

加载器接受完整星期名、`MON` 这类短名称，以及 `1` 到 `7` 的数字值。

## 时间窗口

计划时间窗口可以跨午夜。`RaidPlan` 会根据当前时间判断是否落入窗口。

## 执行说明

调度器会周期性检查计划窗口，遍历配置世界内的在线玩家，解析玩家当前所在的匹配 provider 区域，并在没有活跃袭击阻断时于该区域启动配置的预设。正常袭击启动阻断仍然生效，包括已有活跃袭击冲突和区域 provider flag 检查。
