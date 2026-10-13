# CentOS Stream 9 Package Management

## Overview

This project documents practical package management and troubleshooting using DNF and RPM on CentOS Stream 9 running in VMware Workstation.

The lab covers package discovery, package information, installed-version verification, upgrades, downgrades, version locking, package removal, repository investigation, and evidence-based troubleshooting.


## Objectives

- Identify the Linux distribution and configured repositories.
- Search for packages and investigate unavailable results.
- Inspect package information and available releases.
- Verify installed package versions with RPM.
- Upgrade and downgrade packages where supported.
- Install and test the DNF version-lock plugin.
- Install and safely remove a test package.
- Capture terminal screenshots as evidence.
- Document command purposes, results, errors, and lessons learned.

## Environment

- Operating system: CentOS Stream 9
- Virtualization: VMware Workstation
- Package manager: DNF
- Package database/query tool: RPM
- Shell: Bash
- Repositories observed: AppStream, BaseOS, and Extras Common

## Repository structure

```text
package_management
├── docs
│   ├── lab-report.md
│   └── troubleshooting.md
├── README.md
└── screenshots
    ├── 01-environment.png
    ├── 02-htop-search.png
    ├── 03-package-inspection.png
    ├── 04-jq-upgrade.png
    ├── 05-jq-verification.png
    ├── 06-jq-downgrade.png
    ├── 07-version-lock.png
    └── 08-package-removal.png.png

```

## Progress checklist

Mark a task complete only after verifying its result and capturing the relevant evidence.

- [*] Identify the operating system and repositories.
- [*] Investigate the unavailable `htop` search result.
- [*] Inspect `tree` and `nano`.
- [*] Verify the `jq` upgrade.
- [*] Complete and verify the `jq` downgrade.
- [*] Install and test the version-lock plugin.
- [*] Install and remove a test package.
- [*] Capture and review the screenshots.
- [*] Complete the lab report and troubleshooting notes.
- [*] Commit and push the project to GitHub.

## Key lessons

- DNF manages packages and resolves dependencies using configured repositories.
- RPM queries the local package database.
- An unavailable search result does not, by itself, identify the cause.
- An installed package can be upgraded by an install transaction when a newer candidate is selected.
- A successful transaction must be verified.
- Version locks can restrict package versions selected by DNF.
- Package removal transactions must be reviewed before confirmation.
- Screenshots and written explanations should show real results, not assumed outcomes.

## Documentation

- `docs/lab-report.md` contains the procedure, commands, explanations, and verification results.
- `docs/troubleshooting.md` records problems, investigation, solutions, and lessons learned.
- `docs/screenshots/` contains genuine terminal evidence.

