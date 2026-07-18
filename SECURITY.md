# Security Policy

## Supported Versions

Easy Ads Integration Validator is currently in public beta. Security fixes are published for the latest beta version only.

| Version | Supported |
| --- | --- |
| `0.1.x-beta` | Yes |
| Older versions | No |

## Reporting A Vulnerability

Do not open a public GitHub issue for suspected vulnerabilities, leaked credentials, or private project data exposure.

Report security concerns through GitHub private vulnerability reporting:

```text
https://github.com/techdigga/easy-ads-integration-validator/security/advisories/new
```

If private vulnerability reporting is unavailable, do not include sensitive details in a public issue. Open a minimal public issue asking for a private maintainer contact path.

Include:

- The affected version or artifact name.
- The operating system and install method.
- A concise description of the issue.
- Reproduction steps or a minimal sample when possible.
- Whether any private Unity project data, credentials, ad unit IDs, SDK keys, or analytics identifiers may have been exposed.

We will acknowledge valid reports as soon as practical and publish remediation guidance with the next fixed beta artifact.

## Scope

In scope:

- Vulnerabilities in the CLI package or direct-download binaries.
- Release artifact integrity or provenance concerns.
- Accidental exposure of private project paths or internal development material in public artifacts.

Out of scope:

- Findings produced by static audit rules for a user's Unity project.
- Third-party SDK vulnerabilities in Unity projects scanned by the tool.
- Dashboard, runtime ad serving, or legal compliance issues outside this static scanner.
