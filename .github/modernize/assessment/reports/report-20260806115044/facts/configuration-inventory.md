# Configuration & Externalized Settings Inventory

This project has a minimal configuration footprint with no runtime application properties files — configuration is limited to Maven `pom.xml` build properties, an OpenAPI Generator config, and the OpenAPI spec that drives code generation. Secrets (the DocRaptor API key) are passed programmatically by calling applications.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Maven POM | Build configuration | `pom.xml` | Declares all dependency versions and build plugin configuration via `<properties>` |
| OpenAPI Specification | API contract | `docraptor.yaml` | OpenAPI 3.x spec; authoritative source for generated client code |
| OpenAPI Specification (alternate) | API contract | `api/openapi.yaml` | Alternate copy of the spec used by the generator |
| OpenAPI Generator Config | Code generation | `generator-config.json` | Controls library type (`jersey1`), package naming, artifact metadata, and date library |
| Gradle Properties | Build tool config | `gradle.properties` | Auto-generated; minimal content (no active properties set) |
| Gradle Wrapper Properties | Build tool version | `gradle/wrapper/gradle-wrapper.properties` | Pins Gradle 7.2 distribution |
| Travis CI | CI pipeline | `.travis.yml` | CI build definition |

No `application.properties`, `application.yml`, Spring Cloud Config, environment `.env` files, Vault, or KeyVault references are present.

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Default (no profile) | Always active | Compile, test, and package the library JAR | All production dependencies, maven-compiler-plugin (Java 8), maven-surefire-plugin, maven-jar-plugin |
| `sign-artifacts` | Manual: `-Psign-artifacts` | Sign artifacts for Maven Central (Sonatype OSSRH) release | `maven-gpg-plugin:1.5` — signs JARs and POMs before staging |

No dev/prod/staging runtime build profiles exist. There is a single Sonatype staging configuration (`nexus-staging-maven-plugin:1.6.8`) targeting `https://oss.sonatype.org/` for Maven Central release.

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| (none) | N/A — library has no runtime server | N/A | N/A |

This is a library, not a standalone service. There are no Spring profiles, `@Profile` annotations, runtime environment-specific config files, or `SPRING_PROFILES_ACTIVE` usage.

## Properties Inventory

### Maven POM Properties (`pom.xml`)

| Property Key | Default Value | Profiles | Source |
|---|---|---|---|
| `project.build.sourceEncoding` | `UTF-8` | All | pom.xml |
| `swagger-annotations-version` | `1.6.3` | All | pom.xml |
| `jersey-version` | `1.19.4` | All | pom.xml |
| `jackson-version` | `2.12.6` | All | pom.xml |
| `jackson-databind-version` | `2.12.6.1` | All | pom.xml |
| `jakarta-annotation-version` | `1.3.5` | All | pom.xml |
| `maven-plugin-version` | `1.0.0` | All | pom.xml (declared but unused) |
| `junit-version` | `4.13.2` | All | pom.xml |

### OpenAPI Generator Configuration (`generator-config.json`)

| Property Key | Value | Purpose |
|---|---|---|
| `library` | `jersey1` | Selects Jersey 1.x as HTTP client implementation |
| `dateLibrary` | `joda` | Uses Joda-Time for date/time fields |
| `modelPackage` | `com.docraptor` | Target Java package for generated model classes |
| `invokerPackage` | `com.docraptor` | Target Java package for ApiClient and infrastructure classes |
| `apiPackage` | `com.docraptor` | Target Java package for API operation classes |
| `artifactId` | `docraptor` | Maven artifact ID |
| `artifactVersion` | `3.2.0` | Library version |
| `supportJava6` | `false` | Does not target Java 6 |
| `hideGenerationTimestamp` | `true` | Suppresses timestamp in generated files |

### Maven Compiler Settings (`pom.xml`)

| Setting | Value | Notes |
|---|---|---|
| Java source compatibility | `1.8` | Minimum Java 8 required |
| Java target compatibility | `1.8` | Compiled bytecode targets JVM 8 |
| Compiler fork | `true` | Runs javac in a forked process |
| Memory (initial) | `128m` | `-meminitial 128m` |
| Memory (max) | `512m` | `-maxmem 512m` |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| Test execution (maven-surefire) | `-Xms512m -Xmx1500m`, `parallel=methods`, `forkMode=pertest` | 512 MB – 1.5 GB | One JVM per test method (forked) |
| Library consumer (caller's JVM) | Not specified — caller's responsibility | Not specified | Not applicable |

The library itself has no startup or container configuration. Surefire test JVMs are configured with a 512 MB initial heap and 1.5 GB maximum.

## Startup Dependency Chain

No startup dependency chain applies. This is a library; it has no embedded server, no service startup order, no Docker Compose `depends_on`, and no Kubernetes readiness probes.

The only startup-relevant behavior is that `ApiClient` defaults `connectionTimeout = 0` (infinite), meaning if the DocRaptor API is unreachable, calls will block indefinitely unless the caller overrides the timeout.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage |
|---|---|---|
| DocRaptor API Key (username for HTTP Basic Auth) | API key / credential | **Not stored in this library** — must be supplied by the calling application |
| PDF user/owner passwords (`PrinceOptions.userPassword`, `ownerPassword`) | PDF encryption credentials | In-memory only; passed in `Doc.princeOptions`; not persisted locally |

No secrets are stored in this repository. No HashiCorp Vault, AWS Secrets Manager, Azure KeyVault, Jasypt encryption, or sealed secrets are configured.

### Secrets Provisioning Workflow

The DocRaptor API key is provisioned entirely outside this library:

1. **Source**: The calling application obtains the API key from its own secret store (environment variable, CI/CD secret, vault, etc.).
2. **Binding**: The caller constructs an `ApiClient`, then calls `apiClient.setUsername("YOUR_API_KEY")` to set HTTP Basic Auth credentials. No credential is embedded in this library.
3. **Transmission**: The key is encoded as a Base64 HTTP Basic Auth header (`Authorization: Basic <base64(key:)>`) and sent over HTTPS to `api.docraptor.com` on each request.
4. **Risk**: If the calling application logs the `ApiClient` or `Doc` object, the API key and PDF passwords may appear in log output. No masking is implemented in `toString()` methods.

## Feature Flags

No feature flags, `@ConditionalOnProperty`, `@ConditionalOnExpression`, LaunchDarkly, Unleash, or any feature toggle mechanism is present in this library.

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Java (source/target) | 1.8 (Java 8) | `pom.xml` maven-compiler-plugin |
| Maven | ≥ 2.2.0 (enforced minimum) | `pom.xml` maven-enforcer-plugin |
| Gradle (wrapper) | 7.2 | `gradle/wrapper/gradle-wrapper.properties` |
| Jersey Client (HTTP) | 1.19.4 | `pom.xml` `${jersey-version}` |
| Jersey Multipart | 1.19.4 | `pom.xml` `${jersey-version}` |
| Jackson Core / Annotations / Databind | 2.12.6 / 2.12.6.1 | `pom.xml` `${jackson-version}` / `${jackson-databind-version}` |
| Jackson JAX-RS JSON Provider | 2.12.6 | `pom.xml` |
| Jackson Datatype Joda | 2.12.6 | `pom.xml` |
| Jackson Datatype JSR310 | 2.12.6 | `pom.xml` |
| Swagger Annotations | 1.6.3 | `pom.xml` `${swagger-annotations-version}` |
| Jakarta Annotation API | 1.3.5 | `pom.xml` `${jakarta-annotation-version}` |
| JSR-305 (FindBugs) | 3.0.2 | `pom.xml` |
| JUnit | 4.13.2 | `pom.xml` `${junit-version}` |
| OpenAPI Generator | Not pinned | Used externally to regenerate sources from `docraptor.yaml` |
| maven-compiler-plugin | 3.8.1 | `pom.xml` |
| maven-surefire-plugin | 2.12 | `pom.xml` |
| maven-jar-plugin | 2.2 | `pom.xml` |
| maven-source-plugin | 2.2.1 | `pom.xml` |
| maven-javadoc-plugin | 3.3.2 | `pom.xml` |
| nexus-staging-maven-plugin | 1.6.8 | `pom.xml` |
| build-helper-maven-plugin | 1.10 | `pom.xml` |
