# PersonaShift Play Console Data Safety Map

Last code review: 2026-10-11 (Android version 4)

This is an implementation-derived worksheet for the Google Play Console Data
Safety form. It is not a substitute for checking the production backend,
portrait-processing provider contracts, Firebase/AdMob settings, or the final Play Console form.

## App security and access

- Data is encrypted in transit: **Yes for app-owned traffic.** The production
  API and downloaded result URLs used by the app must remain HTTPS.
- Account creation: **No.** PersonaShift does not create user accounts.
- Local history deletion: Home > My Results > Clear All > confirmation. Removes
  Room history records, saved source-photo copies, thumbnails, result images,
  and Coil image caches. Gallery originals, gallery exports, and copies already
  shared with other apps are not removed.
- Other local data deletion: Android Settings > Apps > PersonaShift > Storage >
  Clear data, or uninstall the app. Clear All retains preferences, daily usage
  counters, and Premium state; none of these actions cancels a Play subscription.
- Local cleanup does not call a server deletion endpoint and does not erase
  feedback submissions. History is local; it is not synchronized to an account.
- Server-side deletion request: email `cukkacreatives@gmail.com` with the subject
  `PersonaShift data deletion request` and the approximate request date, time,
  and selected style or feedback submission involved.
- Android cloud backup/device transfer: **Disabled.**
- Runtime dangerous permission: **Camera only.**
- Photo access: system picker; no broad photo/media or storage permission.
- Ready-result badges and the home notice are in-app UI, not push notifications;
  the merged release manifest does not request `POST_NOTIFICATIONS`.
- Vibration, internet, and network-state access are normal permissions and do
  not show runtime permission dialogs.
- The merged release manifest also contains SDK-added access for Google
  advertising ID and Android Privacy Sandbox advertising services (Ad ID,
  Attribution, and Topics), Google Play Billing, install-referrer measurement,
  foreground/background data transport, and wake lock. These are not dangerous
  runtime permissions, but their related SDK data flows must be declared.

Exact merged release permissions observed during this review:

```text
android.permission.ACCESS_ADSERVICES_AD_ID
android.permission.ACCESS_ADSERVICES_ATTRIBUTION
android.permission.ACCESS_ADSERVICES_TOPICS
android.permission.ACCESS_NETWORK_STATE
android.permission.CAMERA
android.permission.FOREGROUND_SERVICE
android.permission.INTERNET
android.permission.VIBRATE
android.permission.WAKE_LOCK
com.android.vending.BILLING
com.google.android.finsky.permission.BIND_GET_INSTALL_REFERRER_SERVICE
com.google.android.gms.permission.AD_ID
```

The application-specific `DYNAMIC_RECEIVER_NOT_EXPORTED_PERMISSION` is an
internal signature-level protection generated during manifest merging; it is
not user-data access.

## Data types to declare

| Play data type | Collected off device | Main purpose | Required or optional |
| --- | --- | --- | --- |
| Photos | Yes, when the user starts a portrait creation | App functionality; image creation | Optional, user-initiated |
| Approximate location | Yes: country code derived from cellular-network country or device locale; AdMob may also infer it from network data | App functionality/localization; advertising | Portrait request: required; ad processing depends on consent/region |
| Purchase history | Yes through Google Play Billing: product, purchase state, entitlement, and token | App functionality and purchase management | Optional, user-initiated |
| App interactions | Yes through Firebase Analytics and AdMob, including feature, result, purchase, and ad events | Analytics; advertising | Analytics is automatic; rewarded-ad interaction is optional |
| Other user-generated content | Yes: the free-text message submitted from Settings > Contact us | App functionality (support and product feedback); fraud prevention (submission limits) | Optional, user-initiated |
| Crash logs | Yes through Firebase Crashlytics | Analytics/app stability | Automatic |
| Diagnostics | Yes through Crashlytics/Firebase Sessions and Google Mobile Ads | Analytics/app stability; fraud prevention | Automatic when the applicable SDK runs |
| Device or other IDs | Yes: app-generated installation UUID sent with portrait requests and feedback, Firebase installation identifiers (including Remote Config), and potentially advertising identifiers | App functionality, analytics, fraud prevention, advertising | App UUID is sent when creating portraits or submitting feedback; SDK/advertising use depends on the service and consent/region |

The selected language, app version, chosen feature, premium flag, job ID, and
request status are operational metadata. Map them to the closest Play category
where the Console asks for them; feature/use events belong under **App
interactions**, and app/Firebase/advertising identifiers belong under **Device
or other IDs**.

## Current app data flows

- Portrait creation sends `style_id`, the image, `language`, `country`,
  `version`, `device_id`, and `is_plus` to the PersonaShift backend. Result
  reveal reporting sends the job ID, `ad_status`, and optional `ad_error`.
- Settings > Contact us now invokes `POST /api/feedback/`. The JSON body is
  `message`, `app_name: personashift`, `app_version`, and `device_id`. The
  feedback ID is the same app installation UUID used by portrait requests;
  it is not an account ID, hardware ID, or a separate anonymous identifier.
  Feedback does not automatically attach source photos or results.
- The form has no name or email field. Review any personal information users
  may include in free text when completing the final Console categories.
- Treat feedback as persistent server-side collection, not ephemeral processing:
  the documented backend stores the message, a timestamp, and its read state.
- Free users need a successfully earned rewarded-ad reward to unlock a completed
  result. Declining an ad is possible, but leaves the result locked; optional
  ad participation must not be described as free ad-free result access.
- SDK data processing can occur at initialization or ad request/preload, before
  an ad is watched. UMP controls whether the app can request ads and whether
  Settings must show Privacy choices. Premium removes the rewarded-ad step;
  it does not disable Firebase Analytics, Crashlytics, or Remote Config.
- Remote Config supplies the daily allowances. Local defaults are 2 free and
  5 Premium portraits per day; production values can differ. Public web pages
  therefore refer to the allowance displayed in the app rather than fixed counts.

## Sharing decision that must be confirmed

PersonaShift sends photos and request metadata to its backend and image-processing
providers. In Play Console, mark these data types as **shared** unless every
recipient qualifies for a Google Play sharing exception (for example, a
contracted service provider processing data only on the developer's behalf).
Confirm the actual contracts and production data flow before submitting the
form.

Google Play Billing and user-directed sharing through Android's system share
sheet can have Play-defined sharing exceptions. Do not apply an exception
merely because an SDK is from Google; use the current Play definition and each
SDK provider's disclosure guidance.

## Not currently collected by app code

- Precise location or GPS
- Contacts, calendar, SMS/call logs, microphone/audio, or installed-app list
- Broad photo-library/media access
- Complete payment-card or bank details
- Account credentials (there is no account system)

## Before submitting the form

- Confirm the backend retention duration for submitted photos, request
  metadata, generated images, and feedback messages. The Android client and
  API documentation do not establish an exact server retention period; do not
  promise automatic server deletion when Clear All is used.
- Confirm which portrait-processing/backend recipients are processors/service providers and
  whether Play's sharing exception applies.
- Confirm AdMob Privacy & messaging configuration and the exact ad data used in
  each served region.
- Confirm Firebase Analytics retention settings and Crashlytics collection in
  the production Firebase project.
- Keep the Play Console answers, published privacy policy, and production app
  version synchronized whenever an SDK or backend data flow changes.

## Official disclosure references

- [Google Play Data safety definitions and form guidance](https://support.google.com/googleplay/android-developer/answer/10787469?hl=en)
- [Google Mobile Ads SDK data disclosure](https://developers.google.com/admob/android/privacy/play-data-disclosure)
- [Firebase privacy and service data processing](https://firebase.google.com/support/privacy)

The data-type mapping above is inferred from the current client and documented
backend. SDK guidance must be checked against the dependency versions actually
shipped, and final declarations still depend on production settings and contracts.
