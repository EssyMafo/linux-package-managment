# Lab Report: CentOS Stream 9 Package Management

## 1. Lab information

**Platform:** CentOS Stream 9
**Virtualization:** VMware Workstation
**Tools:** DNF, RPM, Bash, Git
**Topic:** Linux package management and troubleshooting

## 2. Objectives

The objective was to learn how to search for, inspect, install, upgrade, downgrade, version-lock, and remove RPM packages. The lab also involved checking repositories, investigating package availability, verifying changes, capturing evidence, and documenting errors.

## 3. Environment verification

### Commands

```bash
whoami
cat /etc/os-release
dnf repolist
dnf repolist --all
```

### Explanation

- `whoami` identifies the current user.
- `cat /etc/os-release` displays distribution identification and version information.
- `dnf repolist` lists enabled repositories.
- `dnf repolist --all` includes disabled repositories.

### Observed result

The VM was identified as CentOS Stream 9. The enabled repositories observed during the lab were AppStream, BaseOS, and Extras Common.

**Evidence:** `screenshots/01-environment.png`

## 4. Investigating the `htop` search

### Commands

```bash
dnf search htop
dnf info htop
dnf repolist
```

### Explanation

`dnf search` searches configured package metadata. `dnf info` attempts to retrieve details for a package. `dnf repolist` helps establish which repositories are enabled.

### Observed result

The search returned `No matches found.` This confirms that the search did not find a match in the configured metadata at that time. It does not prove whether the cause was repository configuration, metadata, or package availability.

### Lesson

Investigate repository and metadata state before concluding why a package cannot be found.

**Evidence:** `screenshots/02-htop-search.png`

## 5. Inspecting `tree` and `nano`

### Commands

```bash
dnf search tree
dnf info tree
dnf --showduplicates list tree
rpm -q tree

dnf info nano
dnf --showduplicates list nano
rpm -q nano
```

### Explanation

- `dnf search` searches for matching package names and descriptions.
- `dnf info` displays package details.
- `dnf --showduplicates list` lists available releases.
- `rpm -q` checks whether a package is installed and displays its installed version.

### Observed results

The `tree` package was reported as version `1.8.0-10.el9`. The available listing showed that release in BaseOS.

The `nano` package was reported as version `5.6.1-7.el9` before the planned downgrade. The repository listing included releases `5.6.1-5.el9`, `5.6.1-6.el9`, and `5.6.1-7.el9`.

**Evidence:** `screenshots/03-package-inspection.png`

## 6. Upgrading and verifying `jq`

### Commands

```bash
dnf install jq
rpm -q jq
jq --version
dnf info jq
```

### Explanation

`dnf install jq` requests the package. If it is already installed but a newer candidate is available, DNF may propose an upgrade. The proposed transaction must be reviewed before confirmation.

`rpm -q jq` verifies the installed RPM release. `jq --version` reports the application version.

### Observed result

The recorded transaction upgraded the RPM from `jq-1.6-17.el9.x86_64` to `jq-1.6-21.el9.x86_64`. Verification returned the release `21.el9`, while `jq --version` returned `jq-1.6`.

A download initially encountered a slow-mirror timeout, but the transaction later completed successfully.

**Evidence:** `screenshots/04-jq-upgrade.png` for the original transaction, if captured, and `screenshots/05-jq-verification.png` for the verification.

## 7. Downgrading jq

### Commands

```bash
rpm -q jq
dnf --showduplicates list jq
dnf downgrade jq
rpm -q jq
dnf info jq
```

### Explanation

A downgrade requests an earlier package version. DNF resolves the transaction using configured repositories and dependencies. Always read the proposed transaction before confirming.
### Result

Record the exact installed version before and after the downgrade, whether the transaction succeeded, and any error returned. Do not mark this task complete until the installed version has been verified.

**Evidence:** `screenshots/06-jq-downgrade.png`

## 8. DNF version locking

### Commands

```bash
rpm -q python3-dnf-plugin-versionlock
dnf install python3-dnf-plugin-versionlock
dnf versionlock --help
dnf versionlock add nano
dnf versionlock list
dnf versionlock delete nano
dnf versionlock list
```

### Explanation

The version-lock plugin adds the `dnf versionlock` command. A lock restricts which package versions DNF can select for normal transactions. The exact lock expression depends on the plugin version.

The final commands remove the test lock and verify the list. Do not leave the test lock active after the lab.

### Result

Record whether the plugin was already installed or had to be installed, the lock entry displayed, and the result after deleting the lock.

**Evidence:** `screenshots/07-version-lock.png`

## 9. Package installation and removal

### Commands

```bash
dnf info jq
rpm -q j
dnf install jq
rpm -q jq
command -v jq
dnf remove jq
rpm -q jq
command -v jq
```
### Result

Record the package actually used, its installed version, and the results of the checks before and after removal. A package is not considered removed merely because the removal command was entered; verify the resulting state.

**Evidence:** `screenshots/08-package-removal.png`

## 10. Verification and lessons learned

For each operation, the lab used a follow-up query to establish the resulting state. The main distinction is between requesting an operation and verifying that it succeeded.

Important commands:

- `dnf search PACKAGE` — search package metadata.
- `dnf info PACKAGE` — inspect package details.
- `dnf --showduplicates list PACKAGE` — inspect available releases.
- `rpm -q PACKAGE` — verify installed package state.
- `dnf install PACKAGE` — install or potentially upgrade a package.
- `dnf downgrade PACKAGE` — request an earlier version.
- `dnf remove PACKAGE` — request package removal.
- `dnf versionlock list` — inspect configured version locks.

## 11. Final status

Complete this section after running and verifying every task. List any unfinished operation, unexpected output, or issue that still needs investigation.

## 12. Evidence

Screenshots are stored in `screenshots/`. Each image should show genuine terminal output corresponding to the task described above.
