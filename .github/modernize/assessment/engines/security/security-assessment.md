# Security Assessment Report

**Generated:** 2026-08-06T12:01:28.0000000Z

## Summary

| Metric | Count |
|--------|-------|
| Total Findings | 12 |
| CVE Vulnerabilities | 6 |
| CWE Vulnerabilities | 6 |
| Total Rules Assessed | 59 |
| Rules Passed | 53 |

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 6 |
| optional | 1 |
| potential | 5 |

## CVE Findings (Dependency Vulnerabilities)

### GHSA-r7wm-3cxj-wff9: jackson-core: Async parser maxNumberLength bypass via chunked digit accumulation (incomplete fix for GHSA-72hv-8253-57qq)

- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:172

[GHSA-r7wm-3cxj-wff9](https://github.com/advisories/GHSA-r7wm-3cxj-wff9): jackson-core: Async parser maxNumberLength bypass via chunked digit accumulation (incomplete fix for GHSA-72hv-8253-57qq)

**Severity**: HIGH

**Affected dependencies**:
  - `com.fasterxml.jackson.core:jackson-core` < 2.18.8 → fix: **2.18.8**
  - `com.fasterxml.jackson.core:jackson-core` >= 2.19.0, < 2.21.4 → fix: **2.21.4**

**Recommended fix**: Upgrade all `com.fasterxml.jackson.core` artifacts to 2.18.9 or later.

### CVE-2026-54513: jackson-databind has an array subtype allowlist bypass in BasicPolymorphicTypeValidator (allowIfSubTypeIsArray)

- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:183

[CVE-2026-54513](https://github.com/advisories/GHSA-rmj7-2vxq-3g9f): jackson-databind has an array subtype allowlist bypass in BasicPolymorphicTypeValidator (allowIfSubTypeIsArray)

**Severity**: HIGH

**Affected dependencies**:
  - `com.fasterxml.jackson.core:jackson-databind` >= 2.10.0, < 2.18.8 → fix: **2.18.8**
  - `com.fasterxml.jackson.core:jackson-databind` >= 2.19.0, < 2.21.4 → fix: **2.21.4**
  - `com.fasterxml.jackson.core:jackson-databind` >= 3.0.0, < 3.1.4 → fix: **3.1.4**

**Recommended fix**: Upgrade all `com.fasterxml.jackson.core` artifacts to 2.18.9 or later.

### CVE-2026-54512: jackson-databind has a PolymorphicTypeValidator bypass via generic type parameters that allows arbitrary class instantiation

- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:183

[CVE-2026-54512](https://github.com/advisories/GHSA-j3rv-43j4-c7qm): jackson-databind has a PolymorphicTypeValidator bypass via generic type parameters that allows arbitrary class instantiation

**Severity**: HIGH

**Affected dependencies**:
  - `com.fasterxml.jackson.core:jackson-databind` >= 2.10.0, <= 2.18.7 → fix: **2.18.8**
  - `com.fasterxml.jackson.core:jackson-databind` >= 2.19.0, <= 2.21.3 → fix: **2.21.4**
  - `com.fasterxml.jackson.core:jackson-databind` >= 3.0.0, <= 3.1.3 → fix: **3.1.4**

**Recommended fix**: Upgrade all `com.fasterxml.jackson.core` artifacts to 2.18.9 or later.

### CVE-2025-52999: jackson-core can throw a StackoverflowError when processing deeply nested data

- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:172

[CVE-2025-52999](https://github.com/advisories/GHSA-h46c-h94j-95f3): jackson-core can throw a StackoverflowError when processing deeply nested data

**Severity**: HIGH

**Affected dependencies**:
  - `com.fasterxml.jackson.core:jackson-core` < 2.15.0 → fix: **2.15.0**

**Recommended fix**: Upgrade all `com.fasterxml.jackson.core` artifacts to 2.18.9 or later.

### CVE-2022-42003: Uncontrolled Resource Consumption in Jackson-databind

- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:183

[CVE-2022-42003](https://github.com/advisories/GHSA-jjjh-jjxp-wpff): Uncontrolled Resource Consumption in Jackson-databind

**Severity**: HIGH

**Affected dependencies**:
  - `com.fasterxml.jackson.core:jackson-databind` >= 2.13.0, < 2.13.4.2 → fix: **2.13.4.2**
  - `com.fasterxml.jackson.core:jackson-databind` >= 2.4.0-rc1, < 2.12.7.1 → fix: **2.12.7.1**

**Recommended fix**: Upgrade all `com.fasterxml.jackson.core` artifacts to 2.18.9 or later.

### CVE-2022-42004: Uncontrolled Resource Consumption in FasterXML jackson-databind

- **Severity:** mandatory
- **Story Points:** 1
- **Files:** pom.xml:183

[CVE-2022-42004](https://github.com/advisories/GHSA-rgv9-q543-rqg4): Uncontrolled Resource Consumption in FasterXML jackson-databind

**Severity**: HIGH

**Affected dependencies**:
  - `com.fasterxml.jackson.core:jackson-databind` >= 2.13.0, < 2.13.4 → fix: **2.13.4**
  - `com.fasterxml.jackson.core:jackson-databind` >= 2.4.0-rc1, < 2.12.7.1 → fix: **2.12.7.1**

**Recommended fix**: Upgrade all `com.fasterxml.jackson.core` artifacts to 2.18.9 or later.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-477: Use of Obsolete Function

- **Category:** Code Quality
- **Severity:** optional
- **Story Points:** 1
- **Files:** src/main/java/com/docraptor/PrinceOptions.java

PrinceOptions.getVersion() / setVersion() (around line 893-905) is explicitly marked as deprecated in its Javadoc: 'Deprecated, use the appropriate pipeline version. Specify a specific version of PrinceXML to use.' The method and its backing field remain in the public API without removal, constituting use of an obsolete API element.

### CWE-772: Missing Release of Resource after Effective Lifetime

- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/main/java/com/docraptor/ApiClient.java

In ApiClient.invokeAPI() (around line 720-748), a ClientResponse object is obtained from getAPIResponse() but is never explicitly closed in a finally block. Jersey 1.x ClientResponse holds an underlying HTTP connection; if not closed, HTTP connections may not be returned to the connection pool, leading to connection leaks under error paths where getEntity() is not called.

### CWE-543: Use of Singleton Pattern Without Synchronization in a Multithreaded Context

- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/main/java/com/docraptor/Configuration.java

Configuration.defaultApiClient is a static field initialized at class load time and exposed via getDefaultApiClient() / setDefaultApiClient(ApiClient). Neither method is synchronized, and the field is not declared volatile. In a multithreaded environment, two threads calling setDefaultApiClient() and getDefaultApiClient() concurrently may observe a stale or partially constructed ApiClient reference due to absence of a memory barrier.

### CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context

- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/main/java/com/docraptor/Configuration.java

The static field Configuration.defaultApiClient is accessed and mutated by getDefaultApiClient() and setDefaultApiClient() without any synchronization (no synchronized keyword, no volatile modifier, no AtomicReference). Concurrent reads and writes to this shared reference are a data race under the Java Memory Model.

### CWE-820: Missing Synchronization

- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/main/java/com/docraptor/Configuration.java

Configuration.defaultApiClient is a mutable static shared resource with no synchronization at all — no synchronized block, no volatile, no lock, no atomic wrapper. Multiple threads sharing a single ApiClient via Configuration.getDefaultApiClient() with concurrent calls to setDefaultApiClient() have no synchronization mechanism whatsoever.

### CWE-778: Insufficient Logging

- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/main/java/com/docraptor/ApiClient.java

ApiClient contains no security event logging. Authentication failures (HTTP 401/403 from the DocRaptor API), connection errors, and API exceptions are caught and wrapped into ApiException but never logged. The optional Jersey LoggingFilter (line 27-130) logs full request/response bodies but is disabled by default and, when enabled, would log the API key in plaintext. There is no structured security audit trail for authentication attempts, failures, or sensitive API calls.
