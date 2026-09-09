# PersonaShift Web Pages

Public website and legal pages for the PersonaShift Android application.

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
