# 命令

`plugin.yml` 只定义了一个根命令 `/raids`。下列所有命令均需要 `customraids.raids` 权限，该权限默认仅授予 OP。

## 通用命令

| 命令 | 用途 | 权限 |
| --- | --- | --- |
| `/raids` | 显示命令面板。 | `customraids.raids` |
| `/raids reload` | 通过插件重载流程重新加载配置和袭击数据。 | `customraids.raids` |
| `/raids stopall` | 停止所有活跃袭击。 | `customraids.raids` |
| `/raids gui` | 为玩家打开管理 GUI，可编辑袭击预设、征兆和袭击计划。 | `customraids.raids` |
| `/raids input <value...>` | 提交当前 GUI 输入提示的值。 | `customraids.raids` |

GUI 支持编辑 `WAVE_RAID` 和 `CHALLENGE_RAID` 预设，包括波次、挑战设置、加权怪物池、Boss 和指令。

## 启动与控制袭击

| 命令 | 用途 | 权限 |
| --- | --- | --- |
| `/raids startpreset <name> [customRegion]` | 使用指定预设在目标区域启动袭击。 | `customraids.raids` |
| `/raids force skipwave [customRegion]` | 跳过波次袭击的当前波。 | `customraids.raids` |
| `/raids force victory [customRegion]` | 强制当前袭击按胜利结算。 | `customraids.raids` |
| `/raids force defeat [customRegion]` | 强制当前袭击按失败结算。 | `customraids.raids` |

省略 `customRegion` 时，执行者必须是玩家且站在支持的目标区域内。提供 `customRegion` 时，会按名称解析内置 Custom 区域。

## Custom 区域

| 命令 | 用途 | 权限 |
| --- | --- | --- |
| `/raids region pos1` | 将当前位置设为 Custom 区域选区的第一个点。仅玩家可执行。 | `customraids.raids` |
| `/raids region pos2` | 将当前位置设为 Custom 区域选区的第二个点。仅玩家可执行。 | `customraids.raids` |
| `/raids region create <name>` | 根据当前选区创建指定名称的 Custom 区域。仅玩家可执行。 | `customraids.raids` |
| `/raids region delete <name>` | 删除指定名称的 Custom 区域。 | `customraids.raids` |
| `/raids region list` | 列出所有 Custom 区域。 | `customraids.raids` |
| `/raids region info <name>` | 查看指定 Custom 区域的信息。 | `customraids.raids` |

Custom 区域保存到 `custom_regions.yml`。

## 袭击刷怪范围覆盖

| 命令 | 用途 | 权限 |
| --- | --- | --- |
| `/raids region setrange` | 将当前长方体选区设为所在区域的袭击刷怪范围。仅玩家可执行。 | `customraids.raids` |
| `/raids region clearrange` | 清除所在区域的刷怪范围覆盖，恢复区域默认边界。仅玩家可执行。 | `customraids.raids` |
| `/raids region viewrange` | 查看所在区域的刷怪范围覆盖信息。仅玩家可执行。 | `customraids.raids` |

上述命令作用于玩家当前所在区域，支持 Lands、Residence、Towny 和 Custom 区域。使用 `setrange` 前，先通过 `/raids region pos1` 和 `/raids region pos2` 选择一个长方体范围。

范围覆盖**只限制刷怪点选择**；ActionBar、计分板、Title 和聊天栏公告等仍按原区域及周围范围的正常逻辑显示。覆盖数据保存到运行时文件 `raid_ranges.yml`。

## Omen

| 命令 | 用途 | 权限 |
| --- | --- | --- |
| `/raids omen give <player\|selector> <omenId> [level]` | 为目标记录指定等级的自定义 Omen 状态。 | `customraids.raids` |
| `/raids omen bottle <player\|selector> <omenId> [level]` | 向目标发放指定 Omen 的瓶装物品。等级补全建议为 1～5。 | `customraids.raids` |
| `/raids omen clear <player\|selector>` | 清除目标的自定义 Omen 状态。 | `customraids.raids` |
| `/raids omen info [player\|selector]` | 查看自己或指定目标的 Omen 信息。 | `customraids.raids` |

目标参数支持精确在线玩家名、`@a`、`@p`、`@r`、`@s`，以及逗号分隔的多个目标，例如 `Steve,Alex,@s`。

## Tab 补全

命令类会补全子命令、预设名、Custom 区域名、Omen ID、选择器、在线玩家，以及激活 GUI 输入提示时的建议值。
