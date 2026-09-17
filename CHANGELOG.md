# Changelog

All notable changes to this fork are documented here. Versioning follows
`{year}.{minor}.{patch}` (see [README.md](README.md#versioning)).

## 2026.1.1

### Fixed

- Fixed an "Internal server error" (`NoClassDefFoundError: com/google/common/collect/ImmutableList`)
  when opening an existing "Maven POM Value Extractor" task for editing. Bamboo 12.1's
  OSGi plugin framework no longer exposes Guava (`com.google.common.*`) to plugins by
  default the way older Bamboo versions did, and this plugin never declared Guava as
  its own dependency — it only worked previously because it "leaked through" from the
  platform. `MavenVariableTaskConfigurator` used `ImmutableList.of(...)` and
  `Maps.newHashMap()` in a static field initializer, so the class failed to even load.
  Replaced both with plain JDK collections (`Collections.unmodifiableList(Arrays.asList(...))`
  and `new HashMap<>()`) — no functional change, no more dependency on Guava at all.

## 2026.1.0

Initial UCSD compatibility fork, forked from
[DevoKun/bamboo-maven-pom-extractor-plugin](https://github.com/DevoKun/bamboo-maven-pom-extractor-plugin)
(originally by David Ehringer), targeting **Bamboo Data Center 12.1 (LTS) / Java 21**.

### Changed

- `bamboo.version` / `bamboo.data.version` bumped `5.15.3` → `12.1.11`
- AMPS bumped `6.2.6` → `9.13.2`; the AMPS Bamboo build plugin artifact was renamed
  upstream from `maven-bamboo-plugin` to `bamboo-maven-plugin`
- Compiler target raised from Java 1.6 to `release 21`
- `maven-model`, `commons-beanutils`, JUnit, Hamcrest, and Mockito bumped to current
  releases (`mockito-all` → `mockito-core`)
- Plugin display name changed to "UCSD Maven POM Value Extractor" and description/vendor
  updated to credit the fork maintainers — plugin key, task type key, and all class
  names are unchanged so existing Bamboo plans keep working after upgrade
- Versioning scheme switched to `{year}.{minor}.{patch}`

### Fixed

- Removed a dead, unused import of `com.atlassian.bamboo.agent.AgentType` (class no
  longer exists in modern Bamboo)
- Replaced the removed `com.atlassian.spring.container.ContainerManager` component
  lookup (used by the remote-agent variable message path) with
  `com.atlassian.bamboo.spring.ComponentAccessor.newLazyComponentReference(...)`
- Replaced `com.opensymphony.xwork.TextProvider` (WebWork-era API, fully removed in
  Bamboo 12.1) with `AbstractTaskConfigurator.getI18nBean()`
- Pinned `jaxb-api`, `istack-commons-runtime`, and `FastInfoset` to real released
  versions in `dependencyManagement`, to break AMPS 9.13.2's own transitive dependency
  on unresolvable `java.net` snapshot coordinates (Oracle's java.net Maven repo was
  shut down in 2017)

### Verified

- `mvn compile`, `mvn test` (16/16 tests pass), and `mvn package` all succeed against
  the real `atlassian-bamboo-web:12.1.11` / `atlassian-bamboo-core:12.1.11` artifacts
- Filtered `atlassian-plugin.xml` inside the packaged jar resolves all Maven
  placeholders correctly and preserves the original plugin key and task type key
