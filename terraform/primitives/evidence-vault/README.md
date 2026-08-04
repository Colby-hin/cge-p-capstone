# Lab 2.5: IaC as Compliance Evidence (AWS)

A reviewed, immutably stored Terraform commit provides stronger compliance evidence than a screenshot. This project builds an Amazon S3 Object Lock vault and a shell script that collects, hashes, bundles, and uploads Terraform evidence.

## Architecture

The capture script reads evidence from the Lab 2.3 Terraform workspace, generates SHA-256 hashes, packages the files into an archive, and uploads the archive to an S3 Object Lock vault.

```text
Lab 2.3 workspace
-----------------
tfplan
Terraform state
Terraform files
Git commit
        |
        v
capture-evidence.sh
-------------------
Collects plan.json
Collects state.json
Records commit.txt
Records version.txt
Creates SHA-256 manifest
Builds bundle.tar.gz
        |
        v
S3 Object Lock vault
--------------------
runs/test-001/bundle.tar.gz
One-day GOVERNANCE retention
AES-256 encryption
Versioning enabled
Public access blocked
        |
        v
JSON receipt
------------
run_id
vault
object key
VersionId
capture timestamp
```

The S3 `VersionId` provides a durable reference to the exact uploaded evidence bundle. An attempted deletion was denied while the retention period was active, confirming that the evidence was protected by Object Lock.

## Status

The evidence vault was successfully deployed, tested, committed to GitHub, and destroyed after the lab was completed.
