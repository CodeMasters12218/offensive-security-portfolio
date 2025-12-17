# Authentication Bypass – State Machine Flaw

## Overview
This assessment demonstrates an authentication bypass caused by a flawed state machine in the application's multi‑step login process. By interrupting the expected sequence of requests, it is possible to obtain elevated privileges without valid authorization.

## Application Behavior
- Users authenticate using a standard username/password form.
- After successful credential submission, the application forces a second step: a role‑selection page (/role-selector).
- The server assumes that any client reaching this step has followed the correct sequence and does not revalidate authentication state.
- The selected role is accepted without verifying whether the user is authorized to assume it.

## Vulnerability
The authentication flow relies on the order of client requests, not on server‑side validation. By preventing the application from serving the intermediate GET /role-selector page, the state machine becomes inconsistent and accepts arbitrary role assignments submitted directly via POST /role-selector.

## Exploitation
1. Logged in with valid low‑privilege credentials.
2. Intercepted the login flow using Burp Suite.
3. Dropped the GET /role-selector request, preventing the server from enforcing the expected state transition.
4. The server defaulted to the administrator role without verification, granting access to the administrative interface.

Used the admin panel to perform privileged actions, including deleting another user account.

## Impact
- Full authentication bypass.
- Unauthorized access to administrative functionality.
- Ability to perform destructive actions such as deleting user accounts.
- Potential for broader privilege escalation in multi‑role environments.

## Mitigation
- Enforce strict server‑side validation of authentication state at every step.
- Do not rely on client‑controlled navigation order.
- Validate that the selected role is authorized for the authenticated user.
- Implement robust session‑state tracking and reject inconsistent transitions.
