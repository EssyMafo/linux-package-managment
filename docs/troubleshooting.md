# Troubleshooting Log: CentOS Stream 9 Package Management

This document records package-management issues encountered during the lab. Entries distinguish observed facts from possible explanations.

## Issue 1: `htop` search returned no matches

### Problem
The package search did not find `htop`.

### Environment
CentOS Stream 9 using DNF with configured AppStream, BaseOS, and Extras Common repositories.

### Symptoms
`dnf search htop` returned `No matches found.`

### Investigation
Commands used:

```bash
dnf search htop
dnf info htop
dnf repolist
dnf repolist --all
```

### Findings
The configured package search did not return a match. The output alone did not establish the underlying cause.

### Resolution
Document the result and investigate repository configuration and metadata before making changes. Do not enable unrelated repositories or install untrusted RPMs just to force a result.

### Lesson
A failed search is a symptom, not a complete diagnosis.


## Issue 2: Slow mirror during the `jq` upgrade

### Problem
A package download encountered a timeout warning.

### Symptoms
DNF reported Curl error 28 and that transfer speed had remained below its threshold.

### Investigation
The download later completed and DNF reported that the transaction was complete.

### Resolution
The transaction succeeded. The resulting package was verified using:

```bash
rpm -q jq
jq --version
```

The RPM release was `jq-1.6-21.el9.x86_64`, while the application reported `jq-1.6`.

### Lesson
A warning during a transaction does not necessarily mean the entire operation failed. Check the final transaction status and verify the installed package.

## Issue 3: `dnf versionlock` command was unavailable

### Problem
The version-lock command was not initially recognized.

### Symptoms
DNF reported that `versionlock` was not a command and suggested installing a plugin.

### Investigation
The correct package was identified as:

```text
python3-dnf-plugin-versionlock
```

The similarly named package query without the separating hyphen did not return a match.

### Resolution
Install the correct plugin if needed:

```bash
dnf install python3-dnf-plugin-versionlock
dnf versionlock --help
```

Then test a lock and remove it afterward.

### Lesson
Optional DNF subcommands can be provided by plugins. Confirm the exact plugin package name before installing it.


## Issue 4: GitHub HTTPS authentication failure

### Problem
Git rejected an HTTPS push with an authentication error.

### Symptoms
The message indicated that password authentication was not supported for Git operations.

### Cause
GitHub HTTPS Git operations require an accepted authentication method, such as a personal access token used in place of a password or a configured credential manager.

### Resolution
Use GitHub's supported authentication process. For a personal access token, grant only the required access to the intended repository. Do not paste a token into a Git command, commit it to a file, or include it in a screenshot.

### Verification
After authentication, verify the push by checking the command's success and the remote repository's commit history.

## Troubleshooting method used in this lab

1. Read the entire error message.
2. Identify the command and its purpose.
3. Check the operating system and current configuration.
4. Query package availability and installed state.
5. Make one justified change at a time.
6. Read the proposed transaction before confirming.
7. Verify the final state with an independent command.
8. Record the actual result and any remaining uncertainty.
9. Remove temporary restrictions or test packages when appropriate.
