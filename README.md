# FlowAudio — Legal Documents

**App:** FlowAudio (AutoSpeedAudio) &nbsp;|&nbsp; **Version:** 1.0.0 &nbsp;|&nbsp; **Package:** `com.cstnl.autospeedaudio`  
**Platform:** Android (API 31+) &nbsp;|&nbsp; **Effective Date:** June 10, 2026

---

- [Privacy Policy](#privacy-policy)
- [Terms of Service](#terms-of-service)

---

# Privacy Policy

This Privacy Policy describes how FlowAudio ("the App," "we," "us," or "our") handles information when you use the FlowAudio Android application (`com.cstnl.autospeedaudio`). We are committed to being fully transparent about what data is processed and why.

## 1. Information We Collect

### 1.1 GPS Speed Data (on-device only)

The App uses your device's GPS chip to measure your current speed via `FusedLocationProviderClient` with `PRIORITY_HIGH_ACCURACY`. Specifically:

- GPS fixes are requested approximately every 1 second (minimum interval 500 ms) while the service is active
- The raw `location.speed` value (m/s) is converted to km/h on-device and passed through an Exponential Moving Average filter
- **Raw coordinates are never read, stored, or transmitted** — the App calls `location.speed`, not `location.latitude` or `location.longitude`
- Smoothed speed readings are used only to select a volume tier and update the on-screen display; they are discarded immediately and never written to disk

### 1.2 Firebase Analytics

The App integrates **Firebase Analytics** (`com.google.firebase:firebase-analytics` v23.2.0) to collect anonymised usage events. The following events are logged:

| Event | When fired |
|---|---|
| `service_started` | User activates the GPS volume service |
| `service_stopped` | User or watch stops the service |
| `terms_accepted` | User accepts the Terms & Privacy screen on first launch |
| `permission_denied` | User denies a required permission (location or notification) |

Firebase Analytics **does not** collect on our behalf:

- Your GPS coordinates, speed readings, or travel history
- Your configured volume levels or speed thresholds
- Your name, email address, or any personally identifiable information
- Your device's IMEI or advertising ID (Firebase uses a pseudonymous app instance ID only)

Analytics data is transmitted to Google's servers and is subject to [Google's Privacy Policy](https://policies.google.com/privacy) and the [Firebase Data Processing Terms](https://firebase.google.com/terms/data-processing-terms).

### 1.3 Firebase Crashlytics

The App integrates **Firebase Crashlytics** (`com.google.firebase:firebase-crashlytics`) to automatically detect and report app crashes and non-fatal errors. When the App crashes or encounters a handled exception, Crashlytics collects:

- A stack trace identifying which part of the App code failed
- Device information: device model, manufacturer, OS version, available memory and storage, and screen resolution
- App information: app version, version code, and time since last restart
- A Crashlytics-assigned installation identifier (a pseudonymous identifier scoped to this app install, distinct from the Firebase Analytics instance ID)

Crashlytics **does not** collect:

- Your GPS coordinates, speed readings, or location history
- Your configured volume levels or speed thresholds
- Your name, email address, or any personally identifiable information

Crash reports are uploaded to Google's servers the next time the App has an internet connection, which may be after the crash occurred. This data is subject to [Google's Privacy Policy](https://policies.google.com/privacy) and the [Firebase Data Processing Terms](https://firebase.google.com/terms/data-processing-terms).

### 1.4 User-Configured Settings

The App stores your preferences locally in Android `SharedPreferences` (file: `flowaudio_settings`). Stored keys are:

| Key | Type | Content |
|---|---|---|
| `speed_t1` – `speed_t4` | Int | km/h thresholds for tiers 1–4 |
| `vol_pct_0` – `vol_pct_4` | Int | Volume percentage per tier (0–100) |
| `tier_en_1` – `tier_en_4` | Boolean | Whether each tier is enabled |
| `sensitivity` | Float | Speed sensitivity (0–1) |
| `terms_accepted` | Boolean | Whether the first-launch legal screen has been accepted |

These values never leave your device.

### 1.5 System Audio State

The App reads `STREAM_MUSIC` volume from `AudioManager` to display the live volume bar and to calculate fade targets. This value is not collected or transmitted.

### 1.6 Wear OS Data Layer

If you use the companion Wear OS app, your five volume percentages, four speed thresholds, per-tier enable flags, sensitivity value, and service active state are written to a Wearable Data Layer item at `/flowaudio/settings`. This data travels over the local Bluetooth/Wi-Fi Wearable Data Layer between your phone and your paired watch only — it is not transmitted to any external server.

### 1.7 What We Do Not Collect

The App does **not** collect:

- GPS coordinates, routes, or location history
- Speed history or trip logs
- Audio content or media metadata
- Names, email addresses, or account credentials
- IMEI, Android advertising ID, or persistent device identifiers

---

## 2. How We Use Information

| Data | Purpose | Leaves device? |
|---|---|---|
| GPS speed (on-device) | Select volume tier; update UI display | No |
| Firebase Analytics events | Understand feature usage; improve the App | Yes — to Google (anonymised) |
| Crashlytics crash/error reports | Diagnose and fix app stability issues | Yes — to Google (pseudonymous) |
| `SharedPreferences` settings | Persist user configuration between sessions | No |
| `STREAM_MUSIC` level | Display live volume bar; calculate fade targets | No |
| Wearable Data Layer item | Sync settings and service state to paired Wear OS watch | No (local only) |

We do not sell, rent, or share data with third parties for advertising or marketing purposes.

---

## 3. Permissions

| Permission | Required on | Purpose |
|---|---|---|
| `ACCESS_FINE_LOCATION` | All versions | `PRIORITY_HIGH_ACCURACY` GPS fixes via `FusedLocationProviderClient` |
| `ACCESS_COARSE_LOCATION` | All versions | Required alongside fine location by the Android platform |
| `FOREGROUND_SERVICE` | All versions | Required to call `startForeground()` |
| `FOREGROUND_SERVICE_LOCATION` | Android 14+ (API 34) | Allows the foreground service to receive GPS callbacks when the screen is off |
| `POST_NOTIFICATIONS` | Android 13+ (API 33) | Required to display the persistent foreground service notification |
| `INTERNET` | All versions | Used by Firebase Analytics and Crashlytics to transmit anonymised usage events and crash reports; the App's GPS and volume features function without internet access |

`ACCESS_FINE_LOCATION`, `ACCESS_COARSE_LOCATION`, and `POST_NOTIFICATIONS` require a runtime grant dialog. The service will not start if any location permission is denied.

---

## 4. Data Storage and Retention

- GPS speed readings are processed in memory and never written to disk
- `SharedPreferences` data persists until you uninstall the App or clear its storage via **Android Settings → Apps → FlowAudio → Storage → Clear Data**
- Firebase Analytics data is retained by Google for 14 months by default, in accordance with [Google's data retention policy](https://support.google.com/analytics/answer/7667196)
- Crashlytics crash and error reports are retained by Google for 90 days by default
- The Wearable Data Layer item is overwritten on every settings change and deleted when the App is uninstalled

---

## 5. Third-Party Services

### Google Play Services — FusedLocationProviderClient

`com.google.android.gms:play-services-location` v21.3.0 provides the GPS speed API. No location data is transmitted to Google through this integration; the App reads only the speed value from the local API response.

### Firebase Analytics

`com.google.firebase:firebase-analytics` v23.2.0 collects the four anonymised events listed in section 1.2. Please refer to:

- [Google's Privacy Policy](https://policies.google.com/privacy)
- [Firebase Data Processing Terms](https://firebase.google.com/terms/data-processing-terms)
- [How Google uses information from apps that use its services](https://policies.google.com/technologies/partner-sites)

### Firebase Crashlytics

`com.google.firebase:firebase-crashlytics` collects crash and error reports as described in section 1.3, including stack traces and device diagnostic information. Crashlytics is a separate Firebase product from Analytics and uses its own installation identifier. Please refer to:

- [Google's Privacy Policy](https://policies.google.com/privacy)
- [Firebase Crashlytics Data Privacy and Security](https://firebase.google.com/support/privacy)
- [Firebase Data Processing Terms](https://firebase.google.com/terms/data-processing-terms)

### Google Wearable Data Layer

`com.google.android.gms:play-services-wearable` v20.0.1 is used to sync settings between the phone app and the Wear OS companion app via `DataClient` and `MessageClient`. Data travels only between your paired devices over the local Wearable Data Layer.

---

## 6. Your Choices

- **Opt out of interest-based advertising:** Go to **Settings → Google → Ads → Delete advertising ID** (Android 12+) or **Opt out of Ads Personalisation** (earlier versions). This limits Firebase Analytics' ability to associate events with an advertising profile.
- **Reset the Firebase instance ID:** Uninstalling and reinstalling the App generates a new Firebase app instance ID, effectively unlinking any prior analytics history.
- **Crashlytics data:** Crash and error reports are tied to a separate Crashlytics installation identifier; uninstalling and reinstalling the App also resets this identifier.
- **Delete all App data:** **Android Settings → Apps → FlowAudio → Storage → Clear Data** removes all locally stored settings, including the `terms_accepted` flag.

---

## 7. Children's Privacy

The App is not directed at children under the age of 13. We do not knowingly collect personal information from children. Firebase Analytics is configured without advertising features. If you have concerns about a child's use of the App, please contact us.

---

## 8. Changes to This Privacy Policy

We may update this Privacy Policy at any time. Changes will be reflected in a new App version with a revised effective date at the top of this document. Continued use of the App after an update constitutes acceptance of the revised policy.

---

## 9. Contact

For questions or concerns about this Privacy Policy, please contact us through the App's listing on the [Google Play Store](https://play.google.com/store).

> **Summary:** GPS coordinates never leave your device. The App sends four anonymised usage events to Firebase Analytics and automatic crash/error reports to Firebase Crashlytics — neither includes location data, speed data, or personal identifiers.

---

# Terms of Service

Please read these Terms of Service ("Terms") carefully before using FlowAudio. By installing or using the App, you agree to be bound by these Terms. If you do not agree, do not use the App.

## 1. Description of the App

FlowAudio (package `com.cstnl.autospeedaudio`, displayed as FlowAudio) is an Android application that automatically adjusts your device's media volume based on real-time GPS speed. It runs as a foreground service (`SpeedVolumeService`) and reads GPS speed via `FusedLocationProviderClient` with `PRIORITY_HIGH_ACCURACY` to apply one of five configurable volume tiers to `STREAM_MUSIC`. An optional Wear OS companion app allows tier and sensitivity adjustments from a paired watch. The App uses Firebase Analytics to collect anonymised usage events and Firebase Crashlytics to collect automatic crash and error reports, both to help improve the product.

---

## 2. Eligibility and Device Requirements

To use the App you must:

- Be at least 13 years of age, or the minimum age of digital consent in your jurisdiction
- Own or have authorised use of an Android device running **Android 11 (API 31) or higher**, targeting API 36
- Have a device with GPS hardware (the App will not install on devices without GPS, enforced by `<uses-feature android:name="android.hardware.location.gps" android:required="true" />`)
- Grant the required location permissions; notification permission is additionally required on Android 13+

---

## 3. Licence

Subject to these Terms, we grant you a limited, non-exclusive, non-transferable, revocable licence to install and use the App on your personal Android device for personal, non-commercial purposes. You may not:

- Copy, modify, distribute, sell, or sublicence the App or any part of it
- Reverse engineer, decompile, or disassemble the App
- Use the App for any unlawful purpose
- Remove or alter any proprietary notices in the App

---

## 4. Acceptable Use

By using the App you agree that:

- You will comply with all applicable laws and regulations, including road traffic and noise laws in your jurisdiction
- You will **not** interact with the App's UI while operating a moving vehicle — configure all settings before driving; stop requires a deliberate long-press on the speed ring specifically to prevent accidental deactivation
- You are solely responsible for ensuring your configured volume tiers allow you to hear safety-critical sounds (emergency sirens, horns, vehicle warnings) at all speeds and in all conditions
- You will not use the App in any way that could endanger yourself or others

---

## 5. Analytics and Data Collection

By using the App you acknowledge and agree that:

- The App collects four anonymised usage events via Firebase Analytics (`service_started`, `service_stopped`, `terms_accepted`, `permission_denied`) as described in the Privacy Policy
- The App automatically collects crash and error reports via Firebase Crashlytics, including stack traces and device diagnostic information (device model, OS version, memory/storage state, app version), as described in the Privacy Policy
- No GPS coordinates, speed readings, or personal data are included in analytics events or crash reports
- Firebase Analytics and Crashlytics require an internet connection to transmit events and reports; the App's core GPS and volume features function fully without internet access
- You may limit analytics and crash data collection using the opt-out options described in the Privacy Policy

---

## 6. GPS and Location Permissions

By granting location permissions you acknowledge that:

- The App accesses GPS solely to calculate instantaneous speed using `location.speed`; it does not read, store, or transmit your coordinates
- GPS accuracy may vary by environment — tunnels, underground car parks, and dense urban canyons can cause signal loss
- Cold GPS start (first launch or after a long gap) may take 30–60 seconds; no volume changes are applied during this time
- You may revoke location permissions at any time via Android Settings; doing so will prevent the service from starting

---

## 7. Foreground Service, Notification, and Battery Use

The App runs `SpeedVolumeService` with `foregroundServiceType="location"` to maintain GPS callbacks when the screen is off. You acknowledge that:

- A persistent notification displaying live speed and volume is shown at all times while the service is active, as mandated by Android
- Continuous `PRIORITY_HIGH_ACCURACY` GPS consumes approximately 5–10% battery per hour on a typical device
- You can stop the service at any time via a long-press on the speed ring in the App, via the notification, or via the STOP button on a paired Wear OS watch

---

## 8. Volume Control and Audio Safety

The App controls your device's `STREAM_MUSIC` volume automatically. You expressly acknowledge and agree that:

- You are solely responsible for configuring volume tiers to levels that are safe for your environment and allow you to hear safety-critical audio at all speeds
- Tier 0 (default 12%) is the permanent floor and cannot be disabled; volume will not drop below it while the service is active
- When the service is stopped, volume **remains at the last level set by the App** and is not restored to the level that existed before activation
- When GPS signal is lost (e.g. in a tunnel), volume remains at the last applied level until the next valid GPS fix; the App does not attempt to lower volume during signal loss
- The speed displayed is a smoothed GPS-derived value (Exponential Moving Average, α = 0.35, ≈ 3 s window); actual speed may differ slightly from the displayed value near thresholds
- Volume tier changes are applied via a 1.2-second linear fade to avoid abrupt changes

---

## 9. Wear OS Companion App

The Wear OS companion app (`:wear` module) is a remote control for the phone service. You acknowledge that:

- The companion app can adjust volume tiers, speed thresholds, per-tier enable flags, and sensitivity from your wrist, and can stop the running service
- The companion app **cannot start** the GPS service; activation is phone-only
- The companion app requires the phone app to be installed and signed with the same key (`com.cstnl.autospeedaudio`)

---

## 10. Disclaimer of Warranties

THE APP IS PROVIDED "AS IS" AND "AS AVAILABLE" WITHOUT WARRANTIES OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE, AND NON-INFRINGEMENT. WE DO NOT WARRANT THAT:

- The App will operate without interruption or error on all devices
- GPS speed readings will be accurate in all environments
- Volume tier transitions will occur at precisely the configured thresholds in all conditions
- Firebase Analytics events and Crashlytics reports will be delivered under all network conditions

---

## 11. Limitation of Liability

TO THE FULLEST EXTENT PERMITTED BY APPLICABLE LAW, WE SHALL NOT BE LIABLE FOR ANY INDIRECT, INCIDENTAL, SPECIAL, CONSEQUENTIAL, OR PUNITIVE DAMAGES ARISING FROM YOUR USE OF THE APP, INCLUDING BUT NOT LIMITED TO:

- Hearing damage caused by elevated media volume
- Failure to hear safety-critical audio (sirens, horns, warnings) due to volume levels set by the App
- Vehicle accidents or personal injury
- Loss of data or device malfunction

IN NO EVENT SHALL OUR TOTAL LIABILITY EXCEED THE AMOUNT YOU PAID FOR THE APP (IF ANY).

---

## 12. Indemnification

You agree to indemnify and hold harmless FlowAudio and its developers from any claims, liabilities, losses, and expenses (including legal fees) arising from your use of the App, your violation of these Terms, or your violation of any applicable law.

---

## 13. Updates and Modifications

We may update the App and these Terms at any time. Updates are delivered via the Google Play Store. Continued use of the App after an update to these Terms constitutes acceptance of the revised Terms.

---

## 14. Termination

We may suspend or terminate access to the App for conduct that violates these Terms. You may stop using the App at any time by uninstalling it.

---

## 15. Governing Law

These Terms are governed by applicable law. Disputes shall be subject to the exclusive jurisdiction of the courts in the developer's country of residence, unless mandatory consumer protection law in your jurisdiction provides otherwise.

---

## 16. Severability

If any provision of these Terms is found unenforceable, it shall be limited to the minimum extent necessary and the remaining provisions shall continue in full force.

---

## 17. Entire Agreement

These Terms, together with the Privacy Policy above, constitute the entire agreement between you and FlowAudio regarding the App.

---

## 18. Contact

For questions about these Terms, please contact us through the App's listing on the [Google Play Store](https://play.google.com/store).

> **By installing and using FlowAudio, you confirm that you have read, understood, and agreed to these Terms of Service.**

---

*FlowAudio v2.9.1 (versionCode 7) · `com.cstnl.autospeedaudio` · Android Kotlin · Jetpack Compose + Wear OS · GPS Edition*
