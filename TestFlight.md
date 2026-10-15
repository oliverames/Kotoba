# Kotoba internal TestFlight

Fork: https://github.com/oliverames/Kotoba

App Store Connect: https://appstoreconnect.apple.com/apps/6823260733/testflight

## Configuration

- Apple Developer account: oliver@ames.consulting
- Team: Oliver Ames, 84M4ZF255G
- App: Kotoba Internal, 6823260733
- Bundle: com.amesconsulting.Kotoba
- Share extension: com.amesconsulting.Kotoba.ShareExtension
- Shared App Group: group.com.amesconsulting.Kotoba
- Shared iCloud key-value store: 84M4ZF255G.com.amesconsulting.Kotoba
- Minimum iOS version: 18.0

Release builds use the existing Apple Distribution certificate and the Kotoba App Store and Kotoba Share Extension App Store provisioning profiles. Debug builds use automatic signing. Credentials stay in the Development vault and local Keychain.

## Release procedure

Increment CURRENT_PROJECT_VERSION for both targets before another upload. The main app and extension must have matching versions.

```sh
xcodebuild -project code/Kotoba.xcodeproj -scheme Kotoba \
  -configuration Release -destination 'generic/platform=iOS' \
  -archivePath build/Kotoba.xcarchive archive

xcodebuild -exportArchive -archivePath build/Kotoba.xcarchive \
  -exportPath build/upload -exportOptionsPlist ci/ExportOptions.plist \
  -authenticationKeyPath "$ASC_KEY_PATH" \
  -authenticationKeyID "$ASC_KEY_ID" \
  -authenticationKeyIssuerID "$ASC_ISSUER_ID"
```

Resolve the API key from 1Password into a protected temporary file. Remove that temporary copy after export. Do not commit private keys or provisioning profiles.

The export options enforce internal-only TestFlight distribution. After Apple processes a build, assign that specific build to Kotoba Internal Testers and verify the intended internal tester. Automatic access to future builds is disabled.

## Verified on October 15, 2026

Version 2.0 (1) passed a signed device archive, strict signature checks for both targets, and Apple upload processing. Apple reports VALID, INTERNAL_ONLY, and READY_FOR_BETA_TESTING. External testing is NOT_APPLICABLE. The build is assigned to Kotoba Internal Testers.

The Release app also built, installed, and opened to the word-entry screen in the iOS simulator. Physical-device installation, dictionary lookup, share-sheet operation, and iCloud synchronization still require device testing.

Apple accepted the upload with a warning about the upstream experimental chock.lproj localization. The existing upstream BartyCrouch and storyboard warnings also remain.
