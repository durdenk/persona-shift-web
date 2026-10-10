# PersonaShift Web Pages

Public website and legal pages for the PersonaShift Android application.

Content reviewed against Android version 4 on 2026-10-11. Pages remain in English.

## App behavior reflected in these pages

- The home catalog contains 20 styles across Eras, History, and Fantasy. Viking
  belongs to History; Elf belongs to Fantasy. The flow starts with a style,
  followed by a camera or system-picker photo.
- Free results require a successfully completed rewarded ad to unlock. Active
  Premium removes that step. Unlocked saved results can be reopened without
  another ad; unsuccessful ads leave free results locked.
- My Results and in-app result alerts expose device-local history. Clear All
  requires confirmation and clears the app's history and private portrait files,
  leaving gallery originals, exports, preferences, usage counters, subscriptions,
  and server-side records outside that cleanup.
- Settings > Contact us is a backend feedback form, not an email composer. It
  sends the message, app name/version, and the app installation UUID without
  automatically attaching photos. Email remains a separate public support channel.
- Weekly, monthly, and yearly Premium subscriptions are available. Both free
  and Premium use have daily allowances supplied by app configuration; public
  copy does not promise unlimited generation, fixed quotas, export resolution,
  or a guaranteed processing priority.
- `app-ads.txt` uses publisher `pub-6793071222149082`, matching the release AdMob
  configuration. The Play links use `com.cukkacreatives.personashift`.

Server retention periods, processor contracts, and production SDK consent and
retention settings are not defined by the Android source. Confirm those values
before making more specific public retention or provider-use claims.

- Website: https://durdenk.github.io/persona-shift-web/
- Privacy policy: https://durdenk.github.io/persona-shift-web/privacy-policy.html
- Terms of use: https://durdenk.github.io/persona-shift-web/terms-of-use.html
- Authorized seller declaration: https://durdenk.github.io/persona-shift-web/app-ads.txt
- Google Play package: `com.cukkacreatives.personashift`

The implementation-derived Play Console worksheet is in [`PLAY_CONSOLE_DATA_SAFETY.md`](./PLAY_CONSOLE_DATA_SAFETY.md). Review it against the production backend and SDK console settings before submitting the Data Safety form.

## Publishing

Copy these files to the root of the [`durdenk/persona-shift-web`](https://github.com/durdenk/persona-shift-web) repository and publish its `main` branch with GitHub Pages.

The `app-ads.txt` file must remain at the root of the published website. Use the GitHub Pages URL as the developer website in Google Play Console so advertising systems can discover it.

After an app permission, SDK, backend field, retention rule, or data recipient changes, update both the privacy policy and the Play Console worksheet before publishing the Android release.
