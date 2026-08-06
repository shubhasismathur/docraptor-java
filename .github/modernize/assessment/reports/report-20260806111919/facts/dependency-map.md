# Dependency Map

This Java SDK project declares a compact set of runtime dependencies for HTTP transport, JSON handling, date support, and annotations, plus one test framework dependency. The runtime dependency surface is approximately 11 libraries (excluding plugins and test scope).

## Dependencies

```mermaid
flowchart LR
    App["docraptor Java SDK"]

    subgraph Web["Web Frameworks"]
        JerseyClient["jersey-client 1.19.4"]
        JerseyMultipart["jersey-multipart 1.19.4"]
    end

    subgraph Data["Database and ORM"]
        NoDb["No database dependencies declared"]
    end

    subgraph Security["Security"]
        AnnotationApi["jakarta.annotation-api 1.3.5"]
    end

    subgraph Logging["Logging"]
        NoLogging["No logging framework dependency declared"]
    end

    subgraph Utility["Utilities"]
        SwaggerAnn["swagger-annotations 1.6.3"]
        Jsr305["jsr305 3.0.2"]
        JacksonCore["jackson-core 2.12.6"]
        JacksonAnn["jackson-annotations 2.12.6"]
        JacksonDatabind["jackson-databind 2.12.6.1"]
        JacksonJaxrs["jackson-jaxrs-json-provider 2.12.6"]
        JacksonNullable["jackson-databind-nullable 0.2.3"]
        JacksonJoda["jackson-datatype-joda 2.12.6"]
        JacksonJsr310["jackson-datatype-jsr310 2.12.6"]
        JodaTime["joda-time 2.9.9"]
    end

    App -->|"http"| Web
    App -->|"persistence"| Data
    App -->|"security annotations"| Security
    App -->|"logging"| Logging
    App -->|"serialization and utilities"| Utility
    JerseyMultipart -.->|"built on"| JerseyClient
    JacksonJaxrs -.->|"integrates with"| JacksonDatabind
```

### Dependency Summary

| Category | Count | Key Libraries | Notes |
|---|---:|---|---|
| Web Frameworks | 2 | jersey-client, jersey-multipart | Legacy Jersey 1.x client stack for REST calls |
| Database / ORM | 0 | None | SDK does not persist local data |
| Messaging | 0 | None | No messaging client dependencies |
| Caching | 0 | None | No caching library declared |
| Logging | 0 | None | Uses optional Jersey logging filter instead of a logging framework dependency |
| Security | 1 | jakarta.annotation-api | Provides annotation compatibility support |
| Observability | 0 | None | No metrics/tracing dependencies |
| Utilities | 10 | Jackson modules, Swagger annotations, Joda-Time | Serialization, typing, and compatibility support |

### Version & Compatibility Risks

The project targets Java 8 and uses legacy Jersey 1.19.4 plus Joda-Time 2.9.9, both of which are older-generation dependencies that may require modernization for newer Jakarta or Java runtime ecosystems. Jackson 2.12.x is stable but not current, so compatibility and vulnerability posture should be tracked as upstream updates are released.

### Notable Observations

- Dependency set is intentionally lean and focused on API transport plus serialization.
- Multiple Jackson modules are used to support date/time variants and nullable schema behavior.
- No ORM, database, cache, or messaging dependencies are present, consistent with an API client library role.
- Test dependencies are minimal, indicating lightweight validation coverage from dependency perspective.

## Test Dependencies

| Framework | Version | Notes |
|---|---|---|
| JUnit | 4.13.2 | Primary test framework declared in Maven and Gradle test scopes |

Total test-scope dependencies: 1

The project has a minimal test dependency footprint centered on JUnit 4, with no dedicated mocking or integration test dependency declared in the main build files.
