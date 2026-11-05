# Release checksums

GitHub reports the following SHA-256 digest for the uploaded Windows distribution asset in release [v1.0.0](https://github.com/JosephBoctor/XenoScanHGT/releases/tag/v1.0.0).

| File | Size (bytes) | SHA-256 |
|---|---:|---|
| `XenoScan1.OneCLick.V1.00.zip` | 37370285 | `9a61779e1b02f0d85d26e7630e05ad997a7d60d1ebdd96d6bd6543e3f5c38417` |

The digest above is the checksum exposed by GitHub's release-asset metadata. Users may independently verify a downloaded file on Windows with:

```powershell
Get-FileHash .\XenoScan1.OneCLick.V1.00.zip -Algorithm SHA256
```

or on macOS/Linux with:

```bash
sha256sum XenoScan1.OneCLick.V1.00.zip
```

The computed digest should match the value in this file before the archive is used.
