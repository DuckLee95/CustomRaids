# 05. 公共 API

公共 API 位于 `io.github.ducklee.customraids.api.CustomRaidsApi`，实现类是 `CustomRaidsApiImpl`。

## 主要 API 方法

预设与区域提供器查询：

```java
Optional<RaidPreset> getPreset(String presetName);
List<String> getPresetNames();
Optional<RegionProvider> getProvider(String providerName);
List<String> getProviderNames();
```

区域提供器注册：

```java
boolean registerRegionProvider(RegionProvider provider);
boolean unregisterRegionProvider(String providerName);
```

区域解析：

```java
Optional<RegionRef> resolveRegionAt(Location location);
Optional<RegionRef> resolveCustomRegion(String regionName);
```

启动袭击：

```java
boolean startRaid(String presetName, RegionProvider provider, RegionRef region);
boolean startRaid(RaidPreset preset, RegionProvider provider, RegionRef region);
RaidStartResult tryStartRaid(String presetName, RegionProvider provider, RegionRef region);
RaidStartResult tryStartRaid(RaidPreset preset, RegionProvider provider, RegionRef region);
```

活跃袭击查询：

```java
Optional<CustomRaidHandle> getActiveRaid(UUID raidId);
Optional<CustomRaidHandle> getActiveRaid(RegionProvider provider, RegionRef region);
Optional<CustomRaidSnapshot> getRaidSnapshot(UUID raidId);
boolean hasActiveRaid(RegionProvider provider, RegionRef region);
Optional<RaidState> getRaidState(RegionProvider provider, RegionRef region);
```

袭击控制：

```java
boolean cancelRaid(UUID raidId);
boolean cancelRaid(RegionProvider provider, RegionRef region);
boolean forceSkipWave(RegionProvider provider, RegionRef region);
boolean forceVictory(RegionProvider provider, RegionRef region);
boolean forceDefeat(RegionProvider provider, RegionRef region);
```

`forceSkipWave` 用于波次推进。挑战袭击不使用波次。

## Handle 运行时信息

`CustomRaidHandle` 还提供活跃袭击的只读辅助方法：

```java
Optional<String> getRaidTypeName();
boolean isChallengeRaid();
int getChallengeTotalKills();
int getChallengeTargetKills();
Optional<String> getChallengePhase();
Optional<String> getChallengePhaseRaw();
int getTotalPlayerDeaths();
Optional<RaidDefeatReason> getDefeatReason();
Optional<String> getDefeatReasonKey();
```

`getChallengePhase()` 返回当前语言文件中的阶段显示文本。`getChallengePhaseRaw()` 返回稳定的代码键，当前可能是 `kills`、`boss` 或 `waves`。

## API 事件

`api/event` 包当前包含袭击生命周期和波次生命周期事件：

- `CustomRaidStartEvent`
- `CustomRaidFinishEvent`
- `CustomRaidWaveStartEvent`
- `CustomRaidWaveClearedEvent`
- `CustomRaidWaveSkippedEvent`
- `CustomRaidStateChangeEvent`

集成插件应优先监听事件，而不是直接轮询内部实现。

`CustomRaidFinishEvent` 提供 `getResult()`、`isForced()`、波次数量，以及 `getDefeatReason()`。失败原因使用 `RaidDefeatReason`：

```java
GENERIC
EMPTY_REGION
TIMEOUT
MAX_DEATHS
FORCED
```

`getDefeatReasonKey()` 返回稳定的配置友好键，例如 `empty_region`、`timeout` 或 `max_deaths`。`RaidDefeatReason` 也可以通过 `key()` 和 `getKey()` 读取该值。

## 区域提供器契约

外部插件可以注册 `RegionProvider`。提供器负责根据位置解析区域、返回在线成员、区域边界、flag，以及处理临时 flag 修改。

内置提供器：

- `CustomRegionProvider`
- `LandsProvider`
- `ResidenceProvider`
- `TownyProvider`

## Handle 与 Snapshot

`CustomRaidHandle` 用于访问活跃袭击。`CustomRaidSnapshot` 用于只读状态快照。其他插件应通过 API 方法访问，而不是直接依赖 `RaidManager` 内部实现。
