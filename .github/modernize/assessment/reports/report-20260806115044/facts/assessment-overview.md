# Assessment Overview

This directory contains supplementary architecture and design facts generated during the application assessment of the **DocRaptor Java** client library (v3.2.0). These documents provide detailed context to support modernization planning.

## Supplementary Documents

| Document | Description |
|---|---|
| [architecture-diagram.md](./architecture-diagram.md) | Two-layer architecture visualization: high-level application architecture (components, external services, technology stack) and detailed component relationship diagram (inter-class dependencies). |
| [dependency-map.md](./dependency-map.md) | Visual map of all external library dependencies grouped by functional category (HTTP client, JSON processing, annotations, utilities), with version risk analysis and test dependencies. |
| [api-service-contracts.md](./api-service-contracts.md) | Full inventory of outbound API operations against the DocRaptor REST API, DTOs, authentication/security posture, communication patterns, and a sequence diagram of request flows. |
| [data-architecture.md](./data-architecture.md) | Data transfer object (DTO) model documentation — entity fields, relationships, absence of local persistence, data sensitivity notes (including PII/credential handling risks). |
| [configuration-inventory.md](./configuration-inventory.md) | Inventory of all configuration sources (Maven POM, OpenAPI generator config), build profiles, framework/runtime versions, and secrets provisioning workflow. |
| [business-workflows.md](./business-workflows.md) | End-to-end business workflow documentation: synchronous PDF generation, asynchronous polling, hosted document generation, PrinceXML customization, and business rules. |
