# Artifact Verification

Release artifacts may include:

- `EasyAdsIntegrationValidator.<version>.nupkg`
- `EasyAdsIntegrationValidator.Mcp.<version>.nupkg`
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

## Current trust model

Current public direct-download binaries are unsigned. The published checksum confirms that a downloaded file matches the release asset, but it does not prove who built the asset or how it was built.

Prefer the NuGet tool install path when possible. When direct downloads are needed, download artifacts only from the GitHub Releases page and verify `checksums.txt` before use.

GitHub artifact attestations are not currently published. Treat direct-download archives as convenience artifacts, verify their checksums, and prefer the NuGet tool install when it fits your environment.

NuGet tool install remains the preferred path for most users:

```bash
dotnet tool install --global EasyAdsIntegrationValidator --version 1.0.0
```

The optional MCP tool is distributed as a separate NuGet artifact:

```bash
dotnet tool install --global EasyAdsIntegrationValidator.Mcp --version 1.0.0
```

Verify the CLI and MCP package files with the same `checksums.txt` manifest before installing a locally downloaded package. Both packages carry assets for .NET 8, .NET 9, and .NET 10; the release workflow installs and smoke-tests each package on every supported framework.

Direct binary archives are intended for users who do not want a global .NET tool install.

Each archive also contains a `VERSION` sidecar at its root. Its exact single-line value must be `easy-ads-validator <version>` for the package version and is static artifact evidence; it is not a substitute for executing the binary. The release workflow executes every archive on its matching GitHub runner, while local checks execute only archives compatible with the current host and validate the sidecar for the others.
