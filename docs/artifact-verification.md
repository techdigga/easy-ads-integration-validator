# Artifact Verification

Release artifacts may include:

- `EasyAdsIntegrationValidator.<version>.nupkg`
- `easy-ads-validator-osx-arm64.zip`
- `easy-ads-validator-osx-x64.zip`
- `easy-ads-validator-win-x64.zip`
- `checksums.txt`

Verify checksums before installing direct-download artifacts:

```bash
shasum -a 256 -c checksums.txt
```

On Windows PowerShell, compare a downloaded file with the checksum entry:

```powershell
Get-FileHash .\easy-ads-validator-win-x64.zip -Algorithm SHA256
```

## Current Beta Trust Model

Public beta direct-download binaries are unsigned. The published checksum confirms that a downloaded file matches the release asset, but it does not prove who built the asset or how it was built.

Prefer the NuGet tool install path when possible. When direct downloads are needed, download artifacts only from the GitHub Releases page and verify `checksums.txt` before use.

GitHub artifact attestations are planned for a future automated release workflow. Until then, treat direct-download binaries as beta convenience artifacts.

NuGet tool install remains the preferred path for most users:

```bash
dotnet tool install --global EasyAdsIntegrationValidator --version 0.1.0-beta.8
```

Direct binary archives are intended for users who do not want a global .NET tool install.
