# Public API

The public API is located at `io.github.ducklee.customraids.api.CustomRaidsApi`. Its implementation class is `CustomRaidsApiImpl`.

## Main API Methods

Preset and region provider lookup:

```java
Optional<RaidPreset> getPreset(String presetName);
List<String> getPresetNames();
Optional<RegionProvider> getProvider(String providerName);
List<String> getProviderNames();
```

Region provider registration:

```java
boolean registerRegionProvider(RegionProvider provider);
boolean unregisterRegionProvider(String providerName);
```

Region resolution:

```java
Optional<RegionRef> resolveRegionAt(Location location);
Optional<RegionRef> resolveCustomRegion(String regionName);
```

Starting raids:

```java
boolean startRaid(String presetName, RegionProvider provider, RegionRef region);
boolean startRaid(RaidPreset preset, RegionProvider provider, RegionRef region);
RaidStartResult tryStartRaid(String presetName, RegionProvider provider, RegionRef region);
RaidStartResult tryStartRaid(RaidPreset preset, RegionProvider provider, RegionRef region);
```

Active raid lookup:

```java
Optional<CustomRaidHandle> getActiveRaid(UUID raidId);
Optional<CustomRaidHandle> getActiveRaid(RegionProvider provider, RegionRef region);
Optional<CustomRaidSnapshot> getRaidSnapshot(UUID raidId);
boolean hasActiveRaid(RegionProvider provider, RegionRef region);
Optional<RaidState> getRaidState(RegionProvider provider, RegionRef region);
```

Raid control:

```java
boolean cancelRaid(UUID raidId);
boolean cancelRaid(RegionProvider provider, RegionRef region);
boolean forceSkipWave(RegionProvider provider, RegionRef region);
boolean forceVictory(RegionProvider provider, RegionRef region);
boolean forceDefeat(RegionProvider provider, RegionRef region);
```

`forceSkipWave` advances wave raids. Challenge raids do not use waves.

## Handle Runtime Data

`CustomRaidHandle` also provides read-only helpers for active raids:

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

`getChallengePhase()` returns the localized phase display text from the current language file. `getChallengePhaseRaw()` returns a stable code key, currently `kills`, `boss`, or `waves`.

## API Events

The `api/event` package contains raid and wave lifecycle events:

- `CustomRaidStartEvent`
- `CustomRaidFinishEvent`
- `CustomRaidWaveStartEvent`
- `CustomRaidWaveClearedEvent`
- `CustomRaidWaveSkippedEvent`
- `CustomRaidStateChangeEvent`

Integrations should listen for events instead of polling internal implementations directly.

`CustomRaidFinishEvent` exposes `getResult()`, `isForced()`, the wave count, and `getDefeatReason()`. Failure reasons use `RaidDefeatReason`:

```java
GENERIC
EMPTY_REGION
TIMEOUT
MAX_DEATHS
FORCED
```

`getDefeatReasonKey()` returns a stable configuration-friendly key such as `empty_region`, `timeout`, or `max_deaths`. The same value is available through `RaidDefeatReason.key()` and `RaidDefeatReason.getKey()`.

## Region Provider Contract

External plugins can register a `RegionProvider`. A provider resolves regions from locations, returns online members and boundaries, handles flags, and applies temporary flag changes.

Built-in providers:

- `CustomRegionProvider`
- `LandsProvider`
- `ResidenceProvider`
- `TownyProvider`

## Handle and Snapshot

Use `CustomRaidHandle` to access an active raid and `CustomRaidSnapshot` for a read-only state snapshot. Integrations should use the public API instead of depending directly on the internal `RaidManager` implementation.
