# SQL Injection – Retrieving Hidden Data

## Overview
This lab demonstrates a basic SQL injection vulnerability in a WHERE clause that allows an attacker to bypass filtering conditions and retrieve hidden data.

## Application Behavior
- Product listings are filtered by category.
- Hidden products are excluded using a boolean condition.
- User input is directly embedded into an SQL query.

## Vulnerability
The application fails to properly sanitize user-supplied input, allowing manipulation of the SQL query logic.

## Exploitation
1. Intercepted the request using Burp Suite.
2. Identified the vulnerable parameter.
3. Injected a crafted SQL payload to alter query logic.

### Payload Used

' OR 1=1--


## Impact
- Access to hidden or unauthorized data.
- Potential exposure of sensitive information.
- Foundation for more severe attacks if combined with other flaws.

## Mitigation
- Use parameterized queries (prepared statements).
- Implement proper input validation.
- Apply least-privilege principles on database access.