# CVE – Authenticated Remote Code Execution in Bayanno Hospital Management System

## Overview

This repository documents a security vulnerability identified in **Bayanno Hospital Management System**, developed by **Creativeitem**.  
The issue resides in the application's update mechanism and allows **authenticated remote code execution (RCE)** under specific conditions.

The purpose of this repository is **security research, responsible disclosure, and defensive awareness**.

---

## Affected Product

- **Product:** Bayanno Hospital Management System  
- **Vendor:** Creativeitem  
- **Component:** Update mechanism (`Updater` controller)  
- **Affected Versions:** All versions prior to a vendor-provided fix  

---

## Vulnerability Summary

An authenticated administrator can upload a crafted update package that is extracted and processed by the system without proper validation or sandboxing.  
During the update process, a PHP script contained in the uploaded package is executed directly by the application.

This behavior enables **arbitrary PHP code execution**, resulting in full compromise of the application and potentially the underlying server.

---

## Technical Details

The vulnerability is caused by the following design flaws:

- Execution of uploaded PHP files via `require` without validation
- Absence of integrity or signature verification for update packages
- No sanitization of ZIP archive contents (Zip Slip risk)
- Arbitrary file write via uncontrolled `copy()` operations
- Update directory located within the web root
- Lack of restrictive allowlists for file destinations

---

## Attack Scenario

1. An attacker gains administrative access (legitimate account or compromised credentials).
2. The attacker uploads a crafted ZIP file through the update interface.
3. The ZIP archive is extracted server-side.
4. A PHP script inside the archive is executed automatically.
5. Arbitrary PHP code execution is achieved.

No additional user interaction is required.

---

## Impact

- Arbitrary PHP code execution
- Arbitrary file creation or overwrite
- Full compromise of the application
- Potential server-level compromise
- Possible lateral movement within the hosting environment

---

## Severity

**CVSS v3.1 Base Score:** 8.8 (High)

