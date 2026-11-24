# Build Verification & Field Validation

This document records the build checks and real-device smoke tests reported for the v1.0.0 Windows x86 release. It separates build-time checks from runtime integration tests and identifies which evidence has not been independently reproduced.

## 1. Build and packaging checks

The maintainer-provided Antigravity build session reported the following results:

| Check | Reported result | What it demonstrates |
| --- | --- | --- |
| JavaScript syntax: `node --check` over files in `src/` | Passed without errors | The checked JavaScript files parsed successfully |
| `npm run package:x86` | Exit code 0 | The x86 portable package task completed |
| Portable application output | `dist/Home Assistant Companion-win32-ia32/Home Assistant Companion.exe` | A portable x86 application directory was produced |
| Native bridge test: `ha-sys-bridge.exe sensors` | Returned a JSON telemetry payload | The bridge started and returned sensor data on the build machine |
| Packaged application startup | Process remained active during a 3-second startup observation | The packaged executable launched; this is only a short startup smoke test |

The build transcript reported these PE header values:

| Executable | Architecture | Reported MinOS | Reported Subsystem |
| --- | --- | --- | --- |
| `Home Assistant Companion.exe` | x86 (32-bit) | 5.1 | 5.1 |
| `ha-sys-bridge.exe` | x86 (32-bit) | 4.0 | 4.0 |

The installer session also reported an NSIS x86 installer built with Electron Builder, including `ha-sys-bridge.exe` and `ha-icon.png` under the unpacked resources directory, with an installation-directory selector, Start Menu/Desktop shortcuts and an uninstaller.

### Interpretation and limits

- These results are transcribed from the maintainer-provided build session; they have not been independently rerun by this documentation change.
- A successful syntax check or package task does not establish runtime correctness of every feature.
- PE architecture and version fields are useful compatibility evidence, but do not prove that every Windows release, driver, hardware device or dependency combination works.
- A three-second process observation proves only that the process launched and remained present during that observation.
- The bridge sensor command demonstrates a telemetry response on the build machine; it does not prove every sensor works on every Windows version.
- The build transcript stated that OAuth2 and the Home Assistant WebSocket connection were still to be tested against a real Home Assistant server at that stage. Subsequent field validation is recorded separately below.

## 2. Installer artifact

The v1.0.0 release record identifies:

- File: `Home-Assistant-Companion-Setup-1.0.0-win-x86.exe`
- Target architecture: x86 (32-bit)
- Packaging: NSIS x86 installer
- Size: 64,973,268 bytes (approximately 61.96 MiB)
- Published SHA-256: `44655171B66C6AF01D5F9D889D26839B61DEFA28F5753E62915ABE0E3FE582EC`

Verify a downloaded installer before execution:

```powershell
Get-FileHash .\Home-Assistant-Companion-Setup-1.0.0-win-x86.exe -Algorithm SHA256
```

The locally calculated digest must match the digest published for the GitHub Release. This repository does not claim that the installer is Authenticode-signed; verify its signature independently if a signed build becomes available.

## 3. Real-device field validation

The maintainer confirmed that the released Companion was installed on both of the following devices and accessed the configured Home Assistant instance:

| Device | Operating system | OS architecture | Result |
| --- | --- | --- | --- |
| PC | Windows 11 | x64 (64-bit) | Installed; access to Home Assistant confirmed |
| Tablet | Windows 8 | x86 (32-bit) | Installed; access to Home Assistant confirmed |

These are real-device smoke tests and materially strengthen the compatibility evidence. They establish installation and successful access to the configured Home Assistant instance on those two specific environments.

They do not, by themselves, establish that every sensor, notification action, remote command, reconnection scenario, upgrade path or uninstall path was exercised on both devices. Exact OS build numbers and per-feature test records have not yet been attached to this public repository.

## 4. Compatibility matrix

| Host operating system | Architecture | Evidence status |
| --- | --- | --- |
| Windows 8 | x86 | Field smoke test: installation and Home Assistant access confirmed on a tablet |
| Windows 11 | x64 | Field smoke test: installation and Home Assistant access confirmed on a PC |
| Windows 8 | x64 | Not yet documented |
| Windows 8.1 | x86 / x64 | Not yet documented |
| Windows 10 | x86 / x64 | Not yet documented |

Windows 11 is available as a 64-bit host; the x86 application runs as a 32-bit process on that host. Do not describe Windows 11 as a 32-bit operating system.

## 5. Home Assistant integration test boundary

The original build session explicitly left OAuth2 and WebSocket validation against a real Home Assistant instance for a later test. The subsequent field tests confirmed access to Home Assistant from the Windows 8 x86 tablet and Windows 11 x64 PC.

The following detailed integration tests remain separate checklist items until individually exercised and recorded:

- OAuth2 login, refresh and logout.
- Device and webhook registration through `mobile_app`.
- Sensor update cadence and enabled/disabled behavior.
- Notification delivery while the WebSocket is connected.
- Notification action event handling.
- WebSocket reconnect after network loss or Home Assistant restart.
- Each documented remote command.
- Internal/external URL switching and SSID behavior.

## 6. Build provenance and reproducibility

The public repository intentionally contains documentation and release binaries, not the application source code. Consequently:

- The source tree and exact source revision used for v1.0.0 are not available in this repository.
- The build workspace's local filesystem path is intentionally not published.
- The build results above are maintainer-reported evidence, not a reproducible public CI build.
- No public CI workflow is claimed to rebuild the release artifact.
- Future releases should retain a private build record containing source revision, toolchain versions, dependency lockfile, build logs, artifact hash and test evidence.

This distinction preserves the source-code distribution boundary while making the available evidence transparent.
