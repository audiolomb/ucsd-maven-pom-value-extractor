# UCSD Maven POM Value Extractor

**Maintained by:** Claudio, Claude and Summer Lombardo — University of California, San Diego

A compatibility fork of the [Bamboo Maven POM Value Extractor](https://github.com/DevoKun/bamboo-maven-pom-extractor-plugin)
(originally by David Ehringer), fully updated for **Bamboo Data Center 12.1 (LTS) on Java 21**.

The original plugin last targeted Bamboo 5.15.3 and is incompatible with Bamboo 12.1.
This fork was created to allow UCSD to upgrade from older Bamboo versions to
Bamboo 12.1 without requiring any changes to existing plans or task configurations.

---

## What it does

* Provides a Bamboo build task that extracts values from a Maven POM and sets Bamboo
  build variables using those values.
* Keeps your Bamboo variables in sync with your **Maven POM** automatically.
* Extract your artifact's **GAV** (groupId/artifactId/version) directly, or "query"
  the POM for any arbitrary element using a bean-path expression.

## Compatibility

| Component | Version |
|-----------|---------|
| Bamboo Data Center | 12.1.x (LTS) |
| Java | 21 (OpenJDK / Temurin) |
| Original upstream | `1.7.1` (David Ehringer / DevoKun fork) |

## What works

The task type, plugin key, and all configuration fields are identical to the upstream
plugin. Existing plans continue working after the Bamboo upgrade with no reconfiguration:

- GAV extraction (groupId / artifactId / version) as Job, Result, or Plan variables
- Custom element extraction via bean-path expressions (e.g. `properties.myProperty`)
- Optional `-SNAPSHOT` stripping from the extracted version
- Custom variable name prefixes

---

## Configuration

### Add the Maven POM Value Extractor to the Build Tasks

* Specify the element key to extract from the POM.
* Specify the variable to load the POM value into.

![pom_extractor_example_config.png](pom_extractor_example_config.png)

### Use the variable name as bamboo.VARIABLE_NAME

* The variable extracted from the POM can now be used in other parts of the **Build Tasks**.
* Any variable name can be referenced using: **$bamboo.VARIABLE_NAME**

![pom_extractor_example_variable_usage.png](pom_extractor_example_variable_usage.png)

---

## To Compile

* Use the maven package target

```shell
mvn package
```

* The resulting build artifact will be at: **target/maven-pom-parser-plugin-*.jar**

---

## Versioning

`{year}.{minor}.{patch}` — e.g. `2026.1.0`

- `year` — the calendar year the release was cut
- `minor` — new features or significant fixes within the year
- `patch` — bug fixes

## Key changes from upstream

Summary of what was required to make the plugin work on Bamboo 12.1 / Java 21:

- Bumped `bamboo.version` / `bamboo.data.version` to `12.1.11` and AMPS to `9.13.2`
- AMPS Maven plugin artifact rename: `maven-bamboo-plugin` → `bamboo-maven-plugin`
- Compiler target raised from Java 1.6 to `release 21`
- Removed a dead import of `com.atlassian.bamboo.agent.AgentType` (class removed in
  modern Bamboo; the import was unused)
- Replaced the removed `com.atlassian.spring.container.ContainerManager` lookup with
  `com.atlassian.bamboo.spring.ComponentAccessor.newLazyComponentReference(...)`
- Replaced `com.opensymphony.xwork.TextProvider` (WebWork-era API, fully removed in
  Bamboo 12.1) with the built-in `AbstractTaskConfigurator.getI18nBean()`
- Pinned `jaxb-api` / `istack-commons-runtime` / `FastInfoset` to real released versions
  in `dependencyManagement`, to break AMPS's own transitive dependency on dead
  `java.net` snapshot coordinates (Oracle's java.net Maven repo was shut down in 2017)
- Bumped `maven-model`, `commons-beanutils`, JUnit, Hamcrest, and Mockito to current
  releases

---

## License

Licensed under the [Apache License 2.0](LICENSE.TXT), the same license as the upstream plugin.

Original plugin copyright © David Ehringer.
Fork maintained by Claudio, Claude and Summer Lombardo — UCSD.

> This project is not affiliated with or endorsed by David Ehringer or Atlassian.
