# Configuration & Externalized Settings Inventory

This repository has a relatively small configuration footprint centered on build metadata, dependency versions, and runtime client settings embedded through library APIs. Configuration is primarily file-based in Maven/Gradle descriptors with no environment-specific deployment profile files in the source tree.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Maven project descriptor | Build configuration | pom.xml | Primary dependency, plugin, and profile definitions |
| Gradle build descriptor | Build configuration | build.gradle | Alternate build configuration and dependency declarations |
| Gradle settings | Build configuration | settings.gradle | Project naming and build wiring |
| OpenAPI specification | Contract configuration | docraptor.yaml | Defines endpoint surface and model schema used for code generation |
| Generator settings | Generation configuration | generator-config.json | Controls OpenAPI code generation behavior |
| Runtime API configuration object | Programmatic runtime configuration | src/main/java/com/docraptor/ApiClient.java | Base path, auth setup, serializer behavior configured in code |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| sign-artifacts (Maven) | Manual (`-Psign-artifacts`) | Signs release artifacts during verify phase | maven-gpg-plugin |
| android target (Gradle) | Property-based (`-Ptarget=android`) | Builds Android library variant and publishing artifacts | com.android.library, android-maven plugin |
| default java target (Gradle) | Automatic when no android target property | Builds Java library and publishes Maven artifact | java plugin, maven-publish plugin |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| default | Library consumer runtime | None profile-specific in repo | Uses default `ApiClient` base path `https://api.docraptor.com` and basic auth strategy |

## Properties Inventory

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| groupId | com.docraptor | all | pom.xml |
| artifactId | docraptor | all | pom.xml |
| version | 3.2.0 | all | pom.xml / build.gradle |
| maven.compiler.source | 1.8 | all | pom.xml plugin configuration |
| maven.compiler.target | 1.8 | all | pom.xml plugin configuration |
| swagger-annotations-version | 1.6.3 | all | pom.xml properties |
| jersey-version | 1.19.4 | all | pom.xml properties |
| jackson-version | 2.12.6 | all | pom.xml properties |
| jackson-databind-version | 2.12.6.1 | all | pom.xml properties |
| junit-version | 4.13.2 | test | pom.xml properties |
| ApiClient basePath | https://api.docraptor.com | runtime default | ApiClient.java |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| docraptor-java-sdk tests | surefire argLine `-Xms512m -Xmx1500m` | 512m to 1500m heap for tests | N/A (library build/test execution only) |
| docraptor-java-sdk compile | compiler memory options `meminitial 128m`, `maxmem 512m` | 128m to 512m compile memory | N/A |

## Startup Dependency Chain

1. Build tooling resolves dependencies from Maven Central before compile/package steps.
2. Consumer application initializes `DocApi` and `ApiClient` at runtime.
3. Outbound API calls require network access and valid DocRaptor credentials before successful document operations.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| DocRaptor API key (username in HTTP Basic auth) | API credential | Provided by consuming application at runtime, not stored in repository |

### Secrets Provisioning Workflow

Secrets are expected to be provided externally by the consuming application or deployment environment at runtime. The repository does not include Key Vault, Vault, or cloud secret manager integration code; credentials flow from caller configuration into `ApiClient.setUsername(...)` before outbound requests are executed.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| ApiClient debugging mode | false | Programmatic toggle `ApiClient.setDebugging(...)` |
| Document test mode | true in examples | Request field in `Doc` payload |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Java language target | 1.8 | pom.xml and build.gradle |
| OpenAPI Generator output | 3.2.0 client package version | Generated source headers and project metadata |
| Jersey client | 1.19.4 | Maven/Gradle dependencies |
| Jackson core stack | 2.12.6 (databind 2.12.6.1) | Maven/Gradle dependencies |
| Swagger annotations | 1.6.3 | Maven/Gradle dependencies |
| JUnit | 4.13.2 | Maven/Gradle test dependency |
| Maven compiler plugin | 3.8.1 | pom.xml build plugins |
