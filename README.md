# Resolving SSL Certificate Chain Trust Issues in Nessus Scan: Self-Signed Certificates

## Overview:
In this lab, I performed a vulnerability scan using Tenable Nessus Essentials, which flagged an issue related to SSL certificate trust. The scan identified that the X.509 certificate for a remote service could not be trusted due to a broken certificate chain. Specifically, the certificate was issued by an unknown certificate authority (CA) because the certificate was self-signed.

## Process:

### Initial Scan:
During the scan, Nessus flagged a "Medium SSL Certificate Cannot Be Trusted" vulnerability.

The certificate chain was broken because the remote host sent a certificate signed by an unknown CA. The certificate in question was issued by the self-signed certificate used by the FileZilla server.

### Detailed Investigation:
The Nessus scan revealed the following certificate details:
- **Subject**: CN=filezilla-server self-signed certificate
- **Issuer**: CN=filezilla-server self-signed certificate

Both the subject and issuer were identical, indicating that the certificate was self-signed and not recognized by the system’s trusted certificate authorities. This is common when services, like FileZilla server, use self-signed certificates instead of obtaining one from a trusted Certificate Authority (CA).

### Implications:
In a controlled lab environment, this issue did not represent a critical vulnerability, but in a production environment, it could lead to trust issues with clients connecting to the service. The lack of a trusted certificate authority chain makes it easier for attackers to impersonate the server and perform man-in-the-middle (MITM) attacks, potentially compromising sensitive data.

### Resolution:
As this issue stemmed from the use of a self-signed certificate by the FileZilla server, I recognized that the resolution in this controlled environment would be to either replace the self-signed certificate with a certificate from a trusted CA or add the self-signed certificate to the local trust store for internal use.

In a production environment, replacing the self-signed certificate with one issued by a well-known Certificate Authority is recommended to avoid trust-related issues and improve security.

### Troubleshooting:
The Nessus scanner flagged the certificate because it was unable to verify the certificate chain without a valid CA at the top. Once I realized the root cause was the self-signed certificate used by FileZilla, I understood this was not an unusual finding in such a lab setup.

### Outcome:
This vulnerability was not a critical security risk in my lab setup but could be a major issue in a production environment.

To resolve the trust issue and prevent potential MITM attacks, the recommended solution is to replace the self-signed certificate with one issued by a trusted CA.

## Tools Used:
- Tenable Nessus Essentials
- Command-line tools (e.g., `ipconfig` for networking)

## Issue Found in Nessus Scan:

![SSL Certificate Issue](https://imgur.com/lojrysH.png)
