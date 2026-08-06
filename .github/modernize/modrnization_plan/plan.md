# Modernization Plan: modrnization_plan

**Project**: docraptor-java

---

## Technical Framework

- **Language**: Java 8 (source/target 1.8)
- **Framework**: Plain Java library (no Spring Boot/Spring Framework), OpenAPI-generated client
- **Build Tool**: Maven 3.x (pom.xml)
- **Database**: None
- **Key Dependencies**:
  - Jersey Client 1.19.4 (HTTP client)
  - Jackson 2.12.6 (JSON processing)
  - Swagger Annotations 1.6.3
  - Jakarta Annotation API 1.3.5
  - JUnit 4.13.2 (testing)

---

## Overview

> This migration modernizes the `docraptor-java` client library — a native Java HTTP client for the DocRaptor HTML-to-PDF/XLS service. The application currently targets Java 8 with legacy Jersey 1.x HTTP client and Jackson 2.12.6 dependencies that carry known CVEs. The new architecture will:
>
> - Remediate all known CVEs in project dependencies (Jersey, Jackson, Swagger) to ensure the library is secure and production-ready.
> - Migrate plaintext API key credentials from source code to Azure Key Vault for centralized, secure secret management.
> - Deploy the modernized library as a containerized Azure Container App for cloud-native hosting.
>
> The migration follows a security-first approach: dependency CVE remediation first, then credential externalization, followed by containerized deployment to Azure.

---

## Migration Impact Summary

```
| Application     | Original Service         | New Azure Service          | Authentication     | Comments                              |
|-----------------|--------------------------|----------------------------|--------------------|---------------------------------------|
| docraptor-java  | Hardcoded API key        | Azure Key Vault            | Managed Identity   | Migrate DocRaptor API key to KV       |
| docraptor-java  | Local/on-premise hosting | Azure Container Apps       | Managed Identity   | Containerize and deploy to ACA        |
```

---

## Open Questions & Questionnaire

- [x] Q: Should integration testing be included in this plan? → A: No — no integration testing environment was specified; skipping integration testing.
- [x] Q: Should infrastructure (IaC) be provisioned? → A: No — no explicit request for Bicep/Terraform provisioning; skipping infrastructure task.
- [x] Q: Target deployment service? → A: Azure Container Apps (default).
