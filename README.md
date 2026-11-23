# Web Application VAPT using OWASP Juice Shop

## Project Overview
This project demonstrates a beginner-friendly Vulnerability Assessment and Penetration Testing (VAPT) exercise against OWASP Juice Shop, an intentionally vulnerable web application.

Testing was performed in an isolated local lab using Kali Linux and Docker.

## Objectives
- Perform web application reconnaissance.
- Identify common web application vulnerabilities.
- Test authentication and access-control weaknesses.
- Collect and document security evidence.
- Recommend remediation measures.

## Lab Environment
- Operating System: Kali Linux
- Target Application: OWASP Juice Shop
- Deployment: Docker
- Target URL: http://127.0.0.1:3000

## Tools Used
- Nmap
- WhatWeb
- cURL
- Browser Developer Tools
- Docker
- Kali Linux

## Vulnerabilities Identified

| Finding | Preliminary Severity |
|---|---|
| Insecure Direct Object Reference (IDOR) | High |
| Hardcoded Administrator Credentials | High |
| Public Directory Listing | Low |
| KeePass Database Exposure | Medium |

Severity ratings are preliminary qualitative assessments, not formal CVSS scores.

## Key Observations

### 1. IDOR
Testing demonstrated that user records could be accessed without proper object-level authorization.

### 2. Hardcoded Credentials
Test credentials embedded in publicly accessible JavaScript allowed authentication to an administrator account.

### 3. Public Directory Listing
An accessible FTP directory exposed file listings without authentication.

### 4. KeePass Database Exposure
An encrypted KeePass database was accessible without authentication. Its password was not recovered, and its contents were not accessed.

## Methodology
1. Reconnaissance
2. Application and endpoint discovery
3. Vulnerability identification
4. Controlled security testing
5. Evidence collection
6. Risk assessment
7. Remediation recommendations
8. Report preparation

## Project Structure

```text
VAPT-Project/
├── Public-Evidence/
│   ├── Access-Control/
│   ├── Authentication/
│   ├── Information-Disclosure/
│   └── Recon/
├── Reports/
│   └── VAPT-Assessment-Report.md
├── .gitignore
└── README.md
eof
