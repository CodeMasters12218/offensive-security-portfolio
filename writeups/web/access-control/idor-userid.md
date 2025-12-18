# Insecure Direct Object Reference (IDOR)

## Overview
A horizontal privilege escalation vulnerability was identified in the application. By manipulating a user‑controlled identifier in a request, an authenticated user can access resources belonging to other users.

## Application Behavior
- The application exposes a user account page accessible after authentication.
- The page retrieves user‑specific data based on a client‑supplied ```id``` parameter.
- The server does not validate whether the authenticated session is authorized to access the requested identifier.

## Vulnerability
The server trusts the ```id``` parameter provided by the client without verifying ownership. This allows an attacker to replace their own identifier with another user’s identifier and retrieve unauthorized data.

## Exploitation
1. Logged in with a low‑privileged account.
2. Accessed the account page, which issued a request such as:

``` GET /my-account?id=wiener ```

3. Modified the id parameter to reference another user:

``` GET /my-account?id=carlos ```

4. The server returned sensitive information belonging to the targeted user, confirming the lack of proper authorization checks.

## Impact
- Exposure of sensitive user information.
- Horizontal privilege escalation.
- Potential access to API keys, tokens, or other confidential data.
- Legal and compliance implications (e.g., GDPR).

## Mitigation
- Enforce strict server‑side authorization checks based on the authenticated session, not user‑supplied identifiers.
- Avoid exposing direct object identifiers in client‑controlled parameters.
- Use indirect references or internal mapping mechanisms.
- Apply consistent access control validation across all endpoints.