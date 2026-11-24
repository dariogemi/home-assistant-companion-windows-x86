# Release Quality Checklist

Use this checklist before publishing a new installer. Mark an item complete only when evidence is available for the exact build being released.

## 1. Build identity and provenance

- [ ] Record the source revision and build environment used to produce the installer.
- [ ] Record the installer version, architecture, file size and SHA-256.
- [ ] Confirm the executable and bundled components match the intended x86 target.
- [ ] Scan the final installer with current endpoint protection tools.
- [ ] If code signing is available, sign the installer and verify the signature on a clean machine.
- [ ] Publish release notes describing user-visible changes and known limitations.

## 2. Operating-system compatibility

Run tests on clean machines or snapshots. Do not infer support from successful compilation.

| Platform | Smoke-test evidence currently recorded |
| --- | --- |
| Windows 8 x86 | Installed on a tablet; access to the configured Home Assistant instance confirmed |
| Windows 11 x64 | Installed on a PC; access to the configured Home Assistant instance confirmed |
| Windows 8 x64 | Not yet documented |
| Windows 8.1 x86 / x64 | Not yet documented |
| Windows 10 x86 / x64 | Not yet documented |

The two recorded results are maintainer-environment smoke tests, not a complete compatibility certification. They establish installation and Home Assistant connectivity only. They do not imply that every sensor, notification path, remote command, upgrade/uninstall path, or hardware-specific feature has been tested on those machines.

For each additional tested platform, record OS build, architecture, installation result, startup result, onboarding result and known limitations. Windows 11 has no 32-bit x86 edition; the x86 application must run on a supported x64 host.

## 3. Installation and lifecycle

- [ ] Install as a standard user where supported.
- [ ] Verify install path selection and shortcuts.
- [ ] Verify first launch and onboarding.
- [ ] Verify upgrade over the previous release.
- [ ] Verify uninstall and cleanup behavior.
- [ ] Verify startup behavior after reboot.
- [ ] Verify behavior when Home Assistant is unavailable during startup.

## 4. Home Assistant integration

- [ ] Verify discovery via mDNS and manual URL entry.
- [ ] Verify OAuth2 login, token refresh and logout.
- [ ] Verify device registration through `mobile_app`.
- [ ] Verify webhook registration and sensor updates.
- [ ] Verify enabled/disabled sensor state synchronization.
- [ ] Verify internal/external URL selection and SSID switching.
- [ ] Verify multiple configured Home Assistant instances.

## 5. Notifications and remote commands

- [ ] Verify WebSocket reconnect after network loss and Home Assistant restart.
- [ ] Verify notification receipt while connected.
- [ ] Verify notification confirmation behavior when supported by the channel.
- [ ] Verify action buttons and the `mobile_app_notification_action` event.
- [ ] Verify documented remote commands individually.
- [ ] Verify remote commands remain disabled unless the user opts in.
- [ ] Verify the UI clearly communicates that local WebSocket delivery requires an active connection.

## 6. Security and privacy

- [ ] Inspect how OAuth access and refresh tokens are stored.
- [ ] Inspect how the webhook ID is stored and protected; treat it as a credential.
- [ ] Verify NTFS permissions on the configuration directory.
- [ ] Verify secrets are redacted from logs, crash reports and diagnostics.
- [ ] Verify TLS certificate validation is strict by default.
- [ ] Verify certificate exceptions are scoped to the intended server.
- [ ] Verify remote system commands require explicit opt-in.
- [ ] Verify sensors that expose user activity can be disabled individually.
- [ ] Document whether local secrets are encrypted at rest; do not claim encryption without evidence.

## 7. Release publication

- [ ] Recompute SHA-256 from the final uploaded release asset.
- [ ] Confirm the published digest matches the downloaded asset.
- [ ] Confirm release asset name, version and architecture are correct.
- [ ] Include test matrix results and known limitations in the release notes.
- [ ] Confirm README download links resolve to the intended release.
- [ ] Keep a copy of the exact installer and test evidence for rollback and support.

## Evidence record

For the v1.0.0 build and field-test evidence, see [Build Verification & Field Validation](BUILD_AND_VALIDATION.md). Keep build-time checks, short startup observations and real-device integration smoke tests as separate evidence categories.

For each release, retain:

- Version and release tag.
- Source/build revision (or an explicit statement that source is not published).
- Build date and environment.
- SHA-256 and file size.
- Operating-system test matrix and results.
- Security checks performed and any exceptions.
- Known issues and rollback instructions.

### Current field validation — 2026-11-23

- Windows 11 x64 PC: Companion installed and access to the configured Home Assistant instance confirmed.
- Windows 8 x86 tablet: Companion installed and access to the configured Home Assistant instance confirmed.
- Evidence scope: maintainer-reported real-device smoke tests. Detailed OS build numbers, installer hash per device, feature-by-feature results and reproducible test artifacts have not yet been attached to this repository.

### Build verification — v1.0.0

- [x] JavaScript syntax check reported successful for files in `src/` (`node --check`).
- [x] `npm run package:x86` reported exit code 0.
- [x] PE headers reported x86 architecture for the application and native bridge.
- [x] `ha-sys-bridge.exe sensors` reported a JSON telemetry response on the build machine.
- [x] Packaged application reported to remain running during a 3-second startup observation.
- [ ] Independently reproduce the build and attach raw logs, exact toolchain versions and source revision to a private provenance record.
- [ ] Run feature-by-feature integration tests against Home Assistant and retain redacted evidence.

The completed items above are maintainer-reported build-session results, not independently rerun by this documentation update. See [Build Verification & Field Validation](BUILD_AND_VALIDATION.md) for interpretation and limitations.
