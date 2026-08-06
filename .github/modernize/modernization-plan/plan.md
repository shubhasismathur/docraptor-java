# Modernization Plan: DocRaptor Java Client Modernization

**Project**: docraptor-java

---

## Technical Framework

- **Language**: Java 1.8 (Java 8)
- **Framework**: None (standalone client library)
- **Build Tool**: Maven 3.x (pom.xml)
- **Database**: None
- **Key Dependencies**: Jersey 1.19.4 (HTTP client), Jackson 2.12.6 (JSON processing), swagger-annotations 1.6.3, JUnit 4.13.2

---

## Overview

> This migration modernizes the docraptor-java client library, a native Java client for the DocRaptor HTML to PDF/XLS service. The application currently runs on Java 8 with aging dependencies (Jersey 1.x HTTP client, older Jackson 2.12.x, and JUnit 4). The new architecture will:
>
> - Upgrade the runtime to Java 21 (LTS) for improved performance, language features, and long-term support
> - Remediate all known CVEs and vulnerable dependencies to ensure a secure client library
> - Containerize the application for cloud-native deployment on Azure Container Apps
> - Deploy to Azure Container Apps for scalable, managed hosting
>
> The migration follows a sequential approach: runtime upgrade first, then security hardening, then containerization/deployment.

---

## Migration Impact Summary

| Application     | Original Service          | New Azure Service        | Authentication   | Comments                                  |
|-----------------|---------------------------|--------------------------|------------------|-------------------------------------------|
| docraptor-java  | Java 8 + Jersey 1.x       | Java 21 runtime          | N/A              | Java upgrade with dependency updates      |
| docraptor-java  | Plaintext/file logging    | Console logging          | N/A              | Cloud-native logging for Azure            |
| docraptor-java  | Local deployment          | Azure Container Apps     | Managed Identity | Containerize and deploy to ACA            |

---

## Open Questions & Questionnaire

- [x] Q: Should the plan include environment/infrastructure provisioning? → A: No — no infrastructure provisioning; focus on code migration with deployment to Azure Container Apps using default settings
- [x] Q: Should the plan include integration testing? → A: No — skip integration testing entirely (no infra provided)
- [x] Q: Should the plan include security/CVE remediation? → A: Yes — include security/CVE remediation (default)
- [x] Q: Which Azure deployment target? → A: Azure Container Apps (default)
