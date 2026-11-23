# Web Application Vulnerability Assessment and Penetration Testing (VAPT)

## 1. Executive Summary

This project demonstrates a hands-on Vulnerability Assessment and
Penetration Testing exercise conducted against OWASP Juice Shop,
an intentionally vulnerable web application.

The assessment was performed in a controlled local Kali Linux
and Docker lab environment.

The testing focused on reconnaissance, authentication,
access control, and information disclosure.

The assessment documented four security findings:

1. Insecure Direct Object Reference (IDOR)
2. Hardcoded Administrator Credentials
3. Public Directory Listing
4. Public Exposure of an Encrypted KeePass Database

The assessment also documented reconnaissance observations
to identify the application's technologies, API endpoints,
and publicly accessible resources.

## 2. Scope and Objectives

Target Application: OWASP Juice Shop

Target URL: http://127.0.0.1:3000

Testing Environment: Local Kali Linux and Docker lab

Objectives:

- Identify the target application's technologies.
- Discover publicly accessible resources and API endpoints.
- Examine authentication weaknesses.
- Test object-level access controls.
- Identify information disclosure issues.
- Document findings and recommend remediation.

Scope Restriction:

All testing was performed against the locally hosted lab
application. No external systems were targeted.

## 3. Tools Used

| Tool | Purpose |
|---|---|
| Kali Linux | Security testing environment |
| Nmap | Network and service discovery |
| WhatWeb | Web technology identification |
| cURL | HTTP request and response testing |
| Browser Developer Tools | Frontend JavaScript inspection |
| Docker | Local application environment |

## 4. Testing Methodology

The assessment followed these stages:

### Stage 1: Reconnaissance

- Identified the target application using WhatWeb.
- Checked HTTP responses using cURL.
- Inspected frontend JavaScript files.
- Identified API endpoint references.
- Reviewed robots.txt.
- Discovered the publicly accessible /ftp/ directory.

### Stage 2: Authentication Testing

- Inspected publicly accessible frontend JavaScript.
- Identified embedded test credentials.
- Validated the credentials against the application's login endpoint.
- Confirmed successful authentication with an administrator account.

### Stage 3: Access Control Testing

- Used an authenticated customer test account.
- Tested access to other user records.
- Observed successful HTTP responses for unauthorized
  object access attempts.

### Stage 4: Information Disclosure Testing

- Tested access to the /ftp/ directory.
- Examined the publicly exposed directory listing.
- Downloaded an encrypted KeePass database without authentication.
- Compared SHA-256 hashes to validate the downloaded file.

## 5. Vulnerability Findings

### Finding 1: Insecure Direct Object Reference (IDOR)

Severity: High (preliminary)

Affected Endpoint:
- GET /api/Users/3
- GET /api/Users/1

Description:

An authenticated customer account was able to access
other users' records by changing the user identifier
in the API request.

Evidence:

- Customer accessed another customer's record.
- Customer accessed an administrator's record.
- Both requests returned HTTP 200 OK.

Impact:

Unauthorized access to user information may result
in data exposure and privacy violations.

Recommendation:

- Implement server-side object-level authorization.
- Verify the requesting user's permissions for every resource.
- Prevent users from accessing records they do not own
  unless explicitly authorized.

### Finding 2: Hardcoded Administrator Credentials

Severity: High (preliminary)

Affected File:
- main.js

Location:
- Line 7482

Affected Endpoint:
- POST /rest/user/login

Description:

Testing credentials were embedded in publicly accessible
frontend JavaScript code.

The discovered credentials successfully authenticated
and provided access to an administrator account.

Evidence:

- Credentials identified through JavaScript inspection.
- Login validation returned HTTP 200 OK.
- The authenticated account had administrator privileges.

Impact:

Exposed administrator credentials may allow unauthorized
administrative access to the application.

Recommendation:

- Never store passwords or secrets in frontend JavaScript.
- Remove exposed credentials and rotate affected passwords.
- Apply secure authentication and authorization controls.
- Review account activity for suspicious access.

Note:

Actual credentials and account identifiers are excluded
from the public evidence.

### Finding 3: Public Directory Listing

Severity: Low (preliminary)

Affected Endpoint:
- /ftp/

Description:

The FTP directory was accessible through an HTTP GET request
without authentication.

The directory listing exposed information such as:

- File names
- File sizes
- Modification timestamps
- Directory contents
- Shortcut files containing external URLs

Additional Observation:

The robots.txt file contained Disallow: /ftp.

However, robots.txt is not an access-control mechanism
and does not prevent direct access to the directory.

Impact:

An attacker may discover resources and information
that could assist further security testing or attacks.

Recommendation:

- Disable unnecessary public directory listings.
- Remove sensitive files from public web directories.
- Apply proper server-side access controls.
- Do not rely on robots.txt to protect sensitive resources.

### Finding 4: Public Exposure of KeePass Database

Severity: Medium (preliminary)

Affected Endpoint:
- /ftp/incident-support.kdbx

Description:

An encrypted KeePass database was downloadable through
an unauthenticated HTTP GET request.

The server returned HTTP 200 OK.

Validation:

The SHA-256 hash of the downloaded database matched
the original database file.

Impact:

An attacker could obtain the encrypted database and
attempt offline password attacks.

Limitation:

The database password was not recovered.
Its stored contents were not accessed.

Recommendation:

- Remove sensitive database files from public directories.
- Restrict access through authentication and authorization.
- Rotate potentially exposed credentials if compromise
  is suspected.

## 6. Reconnaissance Observations

The following observations were recorded:

- OWASP Juice Shop application identification.
- HTTP 200 OK response from the homepage.
- Frontend JavaScript file discovery.
- API endpoint reference discovery.
- robots.txt review.
- Public /ftp/ directory discovery.

These are reconnaissance observations and are not
classified as separate confirmed vulnerabilities.

## 7. Evidence Documentation

Sanitized public evidence is available under Public-Evidence/.

| Evidence File | Purpose |
|---|---|
| Recon/recon-summary.txt | Reconnaissance observations |
| Access-Control/idor-test.txt | IDOR testing |
| Authentication/hardcoded-credentials.txt | Credential exposure |
| Information-Disclosure/directory-listing.txt | Directory listing |
| Information-Disclosure/keepass-exposure.txt | KeePass exposure |

Sensitive credentials and unnecessary account identifiers
have been removed from the public evidence.

## 8. Remediation Summary

| Finding | Recommended Action |
|---|---|
| IDOR | Enforce object-level authorization |
| Hardcoded Credentials | Remove secrets from frontend code and rotate passwords |
| Directory Listing | Disable public directory listing |
| KeePass Exposure | Remove sensitive files and restrict access |

## 9. Limitations

- Testing was limited to a local OWASP Juice Shop lab.
- Findings relate to the tested application and configuration.
- The KeePass database remained encrypted.
- The database password was not recovered.
- No production systems or external organizations were tested.
- Severity ratings are preliminary qualitative assessments,
  not formal CVSS scores.
- This project does not represent a full enterprise penetration test.

## 10. Conclusion

The assessment demonstrated practical web application
security testing skills using Kali Linux and common
security testing tools.

The exercise identified access control weaknesses,
credential exposure, and information disclosure issues.

The project provided hands-on experience in reconnaissance,
HTTP analysis, authentication testing, API access control
testing, evidence collection, vulnerability documentation,
and remediation recommendations.

All activities were performed in a controlled local lab
environment for educational purposes.
