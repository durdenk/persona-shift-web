# PersonaShift Play Console Data Safety Map

Last code review: 2026-09-09

This is an implementation-derived worksheet for the Google Play Console Data
Safety form. It is not a substitute for checking the production backend,
portrait-processing provider contracts, Firebase/AdMob settings, or the final Play Console form.

## App security and access

- Data is encrypted in transit: **Yes for app-owned traffic.** The production
  API and downloaded result URLs used by the app must remain HTTPS.
- Account creation: **No.** PersonaShift does not create user accounts.
- Local data deletion: Android Settings > Apps > PersonaShift > Storage > Clear data,
  or uninstall the app.
- Server-side deletion request: email `cukkacreatives@gmail.com` with the subject
  `PersonaShift data deletion request` and the approximate request date, time, and selected style.
- Android cloud backup/device transfer: **Disabled.**
- Runtime dangerous permission: **Camera only.**
- Photo access: system picker; no broad photo/media or storage permission.
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
| Photos | Yes, when the user starts a transformation | App functionality; image transformation | Optional, user-initiated |
| Approximate location | Yes: country code derived from cellular-network country or device locale; AdMob may also infer it from network data | App functionality/localization; advertising | Transformation request: required; ad processing depends on consent/region |
| Purchase history | Yes through Google Play Billing: product, purchase state, entitlement, and token | App functionality and purchase management | Optional, user-initiated |
| App interactions | Yes through Firebase Analytics and AdMob, including feature, result, purchase, and ad events | Analytics; advertising | Analytics is automatic; rewarded-ad interaction is optional |
| Crash logs | Yes through Firebase Crashlytics | Analytics/app stability | Automatic |
| Diagnostics | Yes through Crashlytics/Firebase Sessions and Google Mobile Ads | Analytics/app stability; fraud prevention | Automatic when the applicable SDK runs |
| Device or other IDs | Yes: app-generated installation UUID, Firebase installation identifiers, and potentially advertising identifiers | App functionality, analytics, fraud prevention, advertising | App UUID is required for transformation; advertising use depends on consent/region |

The selected language, app version, chosen feature, premium flag, job ID, and
request status are operational metadata. Map them to the closest Play category
where the Console asks for them; feature/use events belong under **App
interactions**, and app/Firebase/advertising identifiers belong under **Device
or other IDs**.

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
- User-entered feedback (the API capability exists, but no current UI invokes it)

## Before submitting the form

- Confirm the backend retention duration for submitted photos, request
  metadata, and generated images.
- Confirm which portrait-processing/backend recipients are processors/service providers and
  whether Play's sharing exception applies.
- Confirm AdMob Privacy & messaging configuration and the exact ad data used in
  each served region.
- Confirm Firebase Analytics retention settings and Crashlytics collection in
  the production Firebase project.
- Keep the Play Console answers, published privacy policy, and production app
  version synchronized whenever an SDK or backend data flow changes.
