# Content‑Type Restriction Bypass Leading to Remote Code Execution

## Overview
This assessment identified a weakness in the application's image upload feature. Although the system attempts to restrict uploads to specific file types, it relies on user‑controlled metadata for validation. This allows an attacker to bypass the restriction and upload executable content.

## Application Behavior
- Users can upload an avatar image through their account.
- The server checks the file type based on the Content‑Type header supplied by the client.
- Uploaded files are stored in a publicly accessible directory and can be retrieved directly via a predictable URL.

## Vulnerability
The application performs insufficient server‑side validation of uploaded files. Because it trusts the Content‑Type header instead of inspecting the actual file content, an attacker can disguise executable code as an allowed image type.

## Exploitation
1. The avatar upload functionality and its public storage location were identified.
2. Initial attempts to upload non‑image files were rejected based on the declared MIME type.
3. By modifying the upload request to falsely declare an allowed image Content‑Type, it was possible to upload executable content.
4. The uploaded file was then accessible through the public file path and executed by the server.
5. Execution of the uploaded file enabled access to sensitive data stored on the system.

## Impact
- Remote code execution on the server.
- Unauthorized access to confidential information.
- Potential full compromise of the application environment.
- Risk of lateral movement to other systems.
- Significant operational, security, and reputational consequences.

## Mitigation
- Enforce strict server‑side validation of uploaded files, including inspection of actual file content.
Use a robust allowlist of permitted file types.
- Store uploaded files outside the web‑accessible directory.
- Ensure uploaded files cannot be executed as code.
- Apply additional safeguards such as secure renaming, path sanitization, and disabling script execution in upload directories.
