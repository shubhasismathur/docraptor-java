# Modernization Plan: modnization-plan

**Project**: docraptor-java

---

## Technical Framework

- **Language**: Java 8 (source/target 1.8)
- **Framework**: None (plain Java library — no Spring Boot or application framework)
- **Build Tool**: Maven 3 (pom.xml)
- **Database**: None
- **Key Dependencies**:
  - jersey-client 1.19.4 (HTTP client)
  - jackson-core / jackson-databind 2.12.6 / 2.12.6.1 (JSON processing)
  - swagger-annotations 1.6.3
  - junit 4.13.2 (test)

---

## Overview

> This migration modernizes the **docraptor-java** client library — a native Java SDK for the DocRaptor HTML-to-PDF/XLS service. The application currently runs on Java 8, uses the legacy Jersey 1.x HTTP client, and carries older Jackson and Swagger dependency versions with known CVEs.
>
> The new architecture will:
>
> - Remediate all known CVE vulnerabilities in project dependencies to improve supply-chain security.
> - Containerize the library test/example runner for consistent, repeatable cloud execution.
> - Deploy the containerized application to Azure Container Apps for scalable, managed hosting.
>
> The migration follows a sequential approach: first secure dependencies, then containerize, then deploy.

---

## Migration Impact Summary

```
| Application    | Original Service     | New Azure Service       | Authentication   | Comments                          |
|----------------|----------------------|-------------------------|------------------|-----------------------------------|
| docraptor-java | Local/on-premises    | Azure Container Apps    | Managed Identity | Containerize and deploy to ACA    |
| docraptor-java | Direct dependencies  | (no external service)   | N/A              | CVE remediation of dependencies   |
```

---

## Open Questions & Questionnaire

- [x] Q: What is the target deployment platform? → A: Azure Container Apps (default)
- [x] Q: Should integration tests be included? → A: No — skip integration testing entirely (no Azure environment provided)
- [x] Q: Should infrastructure (IaC) be provisioned? → A: No explicit infrastructure request; skipping infra task
- [x] Q: What authentication method for Azure services? → A: Managed Identity (default)
