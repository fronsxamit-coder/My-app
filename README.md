# NexChat — lightweight Discord-style messenger (Flutter + Firebase)

Features: Google login, email register/login, servers + channels (invite codes),
DMs, group chats, images / video / files / voice notes, reactions, delete,
1-to-1 voice and video calls. All data is stored in your Firebase account, so
logging out or uninstalling loses nothing. Log in on any device to get it back.

## 1. Install tools (once)
- Flutter SDK (flutter.dev) and Android Studio (for the Android SDK)
- Run `flutter doctor` until it shows no errors

## 2. Create the project shell
```bash
flutter create --org com.yourname --project-name nexchat nexchat_app
```
Copy these from this folder into `nexchat_app/`, replacing the originals:
`pubspec.yaml`, `lib/` (whole folder), `firestore.rules`, `storage.rules`.

## 3. Firebase setup (free Spark plan is enough to start)
1. console.firebase.google.com → Add project
2. Add Android app. Package name must match (e.g. `com.yourname.nexchat`)
3. Add your SHA-1 key (needed for Google login):
   `cd android && ./gradlew signingReport` and copy the SHA-1
4. Download `google-services.json` → put in `android/app/`
5. Build → Authentication → enable **Google** and **Email/Password**
6. Build → Firestore Database → create → paste `firestore.rules` in Rules
7. Build → Storage → get started → paste `storage.rules` in Rules
   (Storage may need the Blaze pay-as-you-go plan on new projects. Free quota still applies)
8. Firestore will print a link when a query needs an index
   (chats list, channels list). Open it and click Create.

## 4. Android config
`android/build.gradle` (or settings.gradle.kts) — add the Google services plugin
as shown in the Firebase console instructions.

`android/app/build.gradle`: set `minSdkVersion 23` (needed by WebRTC).

`android/app/src/main/AndroidManifest.xml` — inside `<manifest>` add:
```xml
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.CAMERA"/>
<uses-permission android:name="android.permission.RECORD_AUDIO"/>
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS"/>
<uses-permission android:name="android.permission.READ_MEDIA_IMAGES"/>
<uses-permission android:name="android.permission.READ_MEDIA_VIDEO"/>
```

## 5. Build the APK
```bash
flutter pub get
flutter run                          # test on phone / emulator
flutter build apk --release --split-per-abi
```
APKs are in `build/app/outputs/flutter-apk/`.
Use `app-arm64-v8a-release.apk` for most modern phones (smallest size).

## Known limits / next steps
- Calls: 1-to-1 only, STUN only. Add a TURN server in `call_service.dart`
  for users on strict mobile networks.
- No push notifications yet. Add `firebase_messaging` plus a Cloud Function.
- User search is a case-sensitive prefix match.
- Group chats start with only the creator. Add a "add member" screen.
- Server roles/permissions, message edit, replies, and search are not built yet.
- Rules are a solid starting point, but review them before public launch.
