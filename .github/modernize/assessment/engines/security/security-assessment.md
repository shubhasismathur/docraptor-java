# Security Assessment Report

**Generated:** 2026-08-06T11:27:38.0000000Z

## Summary

| Metric | Count |
|--------|-------|
| Total Findings | 2 |
| CVE Vulnerabilities | 0 |
| CWE Vulnerabilities | 2 |
| Total Rules Assessed | 59 |
| Rules Passed | 57 |

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 0 |
| optional | 0 |
| potential | 2 |

## CVE Findings (Dependency Vulnerabilities)

No CVE findings met the minimum severity threshold (critical).

## CWE Findings (Code-Level Vulnerabilities)

### CWE-543: Use of Singleton Pattern Without Synchronization in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/main/java/com/docraptor/Configuration.java:18, src/main/java/com/docraptor/Configuration.java:36

Configuration keeps a static singleton-like ApiClient (`defaultApiClient`) and exposes unsynchronized global getter/setter methods. Concurrent callers can replace/read this shared mutable instance without synchronization or volatile semantics.

### CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/main/java/com/docraptor/Configuration.java:18, src/main/java/com/docraptor/Configuration.java:37

`defaultApiClient` is shared static state updated through `setDefaultApiClient` and read from `getDefaultApiClient` without synchronization primitives, allowing race-prone global mutation.
