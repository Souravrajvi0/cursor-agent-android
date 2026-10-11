# Publishing to Google Play

Application ID: `com.souravrajvi.agentremote` (cannot change after the first upload).

## Build the bundle

Put upload-key settings in `local.properties` (gitignored) or CI env vars:

```
RELEASE_STORE_FILE=/path/to/upload.jks
RELEASE_STORE_PASSWORD=...
RELEASE_KEY_ALIAS=...
RELEASE_KEY_PASSWORD=...
```

```bash
./gradlew :app:bundlePlayRelease
# -> app/build/outputs/bundle/playRelease/app-play-release.aab
```

Back up the keystore and passwords outside the repo. Enroll in Play App Signing so
Google holds the app signing key and this keystore is only the upload key.

Bump `versionCode` in `app/build.gradle.kts` for every upload.

## Console checklist

- [ ] Developer account (one-time fee, identity verification)
- [ ] Personal accounts: run the required closed test (tester count and duration set by Google) before production access
- [ ] Store listing: title, short and full description, 512x512 icon, feature graphic, 2+ phone screenshots
- [ ] Listing must state the app is unofficial and not affiliated with Anysphere / Cursor; do not use Cursor logos or put "Cursor" in the title
- [ ] Privacy policy URL (the app stores a user-supplied API key on-device and sends it to api.cursor.com)
- [ ] Data safety form (see below)
- [ ] Content rating questionnaire, target audience, ads declaration (no ads)
- [ ] Foreground service declaration: `dataSync` is used for live agent progress notifications
- [ ] Permissions justification: `RECORD_AUDIO` (voice input), `POST_NOTIFICATIONS` (agent progress)

## Data safety notes

- API key: stored on-device in encrypted storage, never sent anywhere except the Cursor API.
- Prompts, repository names and agent output are sent to the Cursor API at the user's request.
- Voice input uses Android speech recognition; audio is not stored by the app.
- No analytics or ad SDKs are included. Confirm this against the dependency list before submitting.
- Backups are disabled (`allowBackup=false`) so the key is not copied to cloud backups.
