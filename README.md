# For Shavana

The letter, packaged as a private app for two phones: **Shavana's Android** and **Hammad's iPhone**.

- It only opens within 5 km of three chosen places. The location is checked on the phone and never sent anywhere.
- On Android, screenshots and screen recordings come out black.
- Answers arrive by email, marked with whose phone sent them.

`index.html` is still the single source for the letter. The app wraps it with [Capacitor](https://capacitorjs.com).

## Settings
At the top of the `<script>` in `index.html`:

```js
const CONFIG = {
  HER_NAME: "Shavana",
  MY_NAME: "Hammad",
  EMAILJS_PUBLIC_KEY: "…",
  EMAILJS_SERVICE_ID: "…",
  EMAILJS_TEMPLATE_ID: "…",
  PLACES: [
    { name: "TCS",  lat: …, lng: … },
    { name: "Home", lat: …, lng: …, radius: 20000 },  // its own radius, in metres
    …
  ],
  RADIUS_METERS: 5000,  // used by any place without its own radius
};
```

After changing anything, rebuild the app and install it again (see below).

## EmailJS
In the EmailJS dashboard → Email Templates → your template:

- **Subject:** `{{answered_by}} answered: {{answer}}`
- **Body:**
  ```
  Answer: {{answer}}
  From: {{answered_by}}'s phone
  Time: {{time}}
  ```

`answered_by` is "Shavana" on her Android and "Hammad" on your iPhone.

If you set allowed origins under Account → Security, add both of these: `https://localhost` (Android app) and `capacitor://localhost` (iPhone app). Otherwise emails from the app will be refused.

## Build
Needs Node, Java 21, the Android SDK and Xcode, all already on this Mac.

```sh
npm install        # first time only
npm run sync       # copies index.html into the apps
```

## Install on Shavana's Android
Build the signed APK:

```sh
npm run apk
# → android/app/build/outputs/apk/release/app-release.apk
```

Then install it on her phone over USB, in one of two ways:

- **File copy:**
  1. Connect the cable and choose "File transfer" on her phone.
  2. Copy `app-release.apk` into Downloads.
  3. Open it from her Files app and allow "Install unknown apps" for Files when asked.
  4. After installing, turn that permission off again and **delete the APK** from Downloads.
- **adb:**
  1. On her phone: Settings → About phone → tap Build number 7 times → Developer options → turn on USB debugging.
  2. On the Mac: `adb install android/app/build/outputs/apk/release/app-release.apk`
  3. Turn USB debugging off afterwards.

The first time she opens the app, Android asks for location. She needs to allow it.

### ⚠️ Keep the signing key safe
`android/for-shavana.keystore` and `android/keystore.properties` (which holds its password) are **not in git**. Back both up somewhere private. Any future update has to be signed with this same key, or Android will refuse to install it over the old version.

## Install on Hammad's iPhone
1. `npm run ios` opens the project in Xcode.
2. Select the **App** target → Signing & Capabilities → Team: add and choose your Apple ID (a free account works).
3. Connect the iPhone, select it at the top of Xcode, and press **Run** (▶).
4. On the iPhone: Settings → General → VPN & Device Management → trust your developer profile.

With a free Apple ID the app stops opening after **7 days**. Connect the phone and press Run again to renew it. A paid Apple Developer account ($99 a year) lasts a year.

## What it can't stop
- A photo of the screen taken with another phone.
- Screenshots on the iPhone. iOS doesn't allow apps to block them, but it's your own phone.
- A GPS-faking app getting past the location lock.
- Anyone reading the text inside `index.html` itself. Keep this folder private, and don't host the page on the web.
