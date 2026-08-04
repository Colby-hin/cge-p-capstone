# Lab 2.3: Building a Compliant Resource (AWS S3)

This project uses Terraform to create a secure Amazon S3 architecture with a primary data bucket and a separate bucket for access logs.

## Architecture

```text
Primary S3 bucket
-----------------
AES-256 encryption
Versioning enabled
Public access blocked
Required compliance tags
        |
        | S3 server-access logs
        v
Log S3 bucket
-------------
AES-256 encryption
Public access blocked
Log-delivery permissions
Required compliance tags
```

The primary bucket stores protected data. A separate log bucket receives access records from the primary bucket so audit evidence is kept apart from the data being monitored.

## Security Controls

- **SC-28:** AES-256 server-side encryption protects data at rest.
- **AC-3:** All four S3 Block Public Access settings are enabled.
- **CM-6:** Required tags are applied automatically, and versioning is enabled on the primary bucket.
- **AU-3:** S3 server-access logging records activity against the primary bucket.
- **AU-6:** Logs are stored in a separate bucket for later review and analysis.

## Compliance Evidence

Terraform exported machine-readable evidence of the deployment:

```text
evidence/lab-2-3/plan.json
evidence/lab-2-3/state.json
```

The live AWS configuration was also verified with the AWS CLI to confirm:

- Encryption used `AES256`
- Versioning was `Enabled`
- All public-access-block settings were `true`

## Status

The S3 resources were successfully deployed, verified, documented, committed to GitHub, and destroyed after the lab was completed.
