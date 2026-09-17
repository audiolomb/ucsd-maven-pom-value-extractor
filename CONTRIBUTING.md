# Contributing to UCSD Maven POM Value Extractor

Before submitting a pull request, please make sure your code is covered by tests.

## Building the Code

The code is built with Maven and JDK 21.

1. Clone the repo.
2. Build and run the unit tests:
   ```shell
   mvn test
   ```
3. Build the plugin jar:
   ```shell
   mvn package
   ```

After the build finishes, you'll find `maven-pom-parser-plugin-<version>.jar` under
`target/`. This jar can be uploaded into Bamboo via **Manage apps → Upload app**.

## Versioning

This project uses `{year}.{minor}.{patch}` versioning — see
[README.md](README.md#versioning). Bump the `<version>` in `pom.xml` when cutting a
release.

## Compatibility notes

The plugin key (`com.davidehringer.atlassian.bamboo.maven.maven-pom-parser-plugin`),
task type key (`maven-pom-parser-plugin`), and all class names must stay unchanged so
that existing Bamboo plans keep recognizing this as the same task after an upgrade.
If you need to change any of those, call it out explicitly in your PR description.
