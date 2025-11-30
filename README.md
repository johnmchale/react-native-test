# React Native Expo + Expo Router + NativeWind + MMKV (beta) Template

This repository is a **starter template** for building React Native apps using:

- [Expo](https://expo.dev/) (SDK 54)
- [Expo Router](https://expo.github.io/router/)
- [NativeWind](https://www.nativewind.dev/) (Tailwind CSS for React Native)
- [MMKV](https://github.com/mrousavy/react-native-mmkv) **v4 beta** (fast key/value storage)

It is intended to be cloned and used as a base for new mobile apps. It also includes notes and instructions for getting Android / Java set up on Windows and running on both emulator and real devices.

> **Critical note about Expo Go**
>
> - This template uses **`react-native-mmkv`**, which is a **native module**.
> - Because of this, the app **will not run in Expo Go**.
> - To run it on a device or emulator you **must** use a native build, e.g.:
>   - `npm run android` (for Android)
> - Do **not** scan the QR code in Expo Go expecting this project to work there.

---

## Features

- **Expo** SDK 54 (managed/bare hybrid, Android native project included)
- **Expo Router** with:
  - Stack navigation
  - Example screens: `Home` and `Details`
  - 404 handler (`+not-found.tsx`)
- **NativeWind + Tailwind v4** via `global.css` and `@tailwindcss/postcss`
- **MMKV v4 beta** preinstalled in `package.json`:
  - `"react-native-mmkv": "^4.0.0-beta.10"`
- **Android native project**:
  - Gradle 8, Kotlin, new architecture enabled, Hermes enabled
- **Tooling**:
  - TypeScript `strict` mode
  - ESLint + Prettier + `prettier-plugin-tailwindcss`
  - Metro + NativeWind config
  - Web support with `app/+html.tsx`

---

## Project Structure

- [app](app)
  - [`_layout.tsx`](app/_layout.tsx) – Expo Router stack layout, imports [`global.css`](global.css)
  - [`index.tsx`](app/index.tsx) – Home screen, uses [`components/Button.tsx`](components/Button.tsx) and [`components/ScreenContent.tsx`](components/ScreenContent.tsx), navigates to `/details`
  - [`details.tsx`](app/details.tsx) – Details screen reading route params via `useLocalSearchParams`
  - [`+not-found.tsx`](app/+not-found.tsx) – 404 screen
  - [`+html.tsx`](app/+html.tsx) – Web-only HTML shell for static web rendering
- [components](components)
  - [`Button.tsx`](components/Button.tsx) – Reusable NativeWind-styled button
  - [`ScreenContent.tsx`](components/ScreenContent.tsx) – Screen wrapper (title, separator, helper text)
  - [`EditScreenInfo.tsx`](components/EditScreenInfo.tsx) – “edit this file and hot reload” helper
- Root config
  - [`app.json`](app.json) – Expo app config (name, slug, scheme, Android package, web options)
  - [`package.json`](package.json) – Scripts and dependencies
  - [`tsconfig.json`](tsconfig.json) – TS config with `@/*` path alias
  - [`babel.config.js`](babel.config.js) – Babel preset + `react-native-worklets/plugin`
  - [`metro.config.js`](metro.config.js) – Metro + `withNativewind`
  - [`global.css`](global.css) – Tailwind + NativeWind styles
  - [`postcss.config.mjs`](postcss.config.mjs) – Tailwind v4 PostCSS plugin
  - [`eslint.config.js`](eslint.config.js), [`prettier.config.js`](prettier.config.js)
- Android native
  - [`android`](android) – Native Android project (Gradle, manifests, Kotlin `MainActivity` / `MainApplication`, resources, ProGuard, etc.)

---

## Using This Template

### 1. Create a New App from This Repo

1. Clone this repo:

   ```bash
   git clone <this-repo-url> my-new-app
   cd my-new-app
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Quick dev run (Android only):

   ```bash
   npm run start   # Metro / Expo dev server
   npm run android # Build & install native Android app
   ```

### 2. Rename the Repo / App After Cloning

When you use this repo as a template, you should **immediately rename** it so it uses **your own app name and package IDs** instead of:

- Repo / project name: `react-native-expo-nativewind-mmkv`
- Java package: `com.anonymous.reactnativeexponativewindmmkv`
- Android deep-link scheme: `react-native-expo-nativewind-mmkv`

There are three areas to update.

#### a) Expo / JS configuration

- `app.json`
  - `name` – Human‑readable app name shown on the device.
  - `slug` – Expo / URL slug, used in dev tools and (optionally) builds.
  - `android.package` – Java package, must be a valid identifier, e.g. `com.yourcompany.yourapp` (**no hyphens**).

- `package.json`
  - `name` – NPM/package name, e.g. `"yourapp"` or `"@your-scope/yourapp"`.

- `cesconfig.jsonc`
  - `projectName` – currently `"react-native-expo-nativewind-mmkv"`; change to your new project name if you use this config.

#### b) Android native identifiers

- `android/app/build.gradle`
  - `namespace` – should match your Java package, e.g. `com.yourcompany.yourapp`.
  - `defaultConfig.applicationId` – same value as above.

- `android/app/src/main/AndroidManifest.xml`
  - `package` attribute – should match your Java package.
  - `<data android:scheme="react-native-expo-nativewind-mmkv"/>` – change the `scheme` to a value that matches your app (used for deep links), e.g. `"yourapp"`.

- `android/app/src/main/java/com/anonymous/reactnativeexponativewindmmkv/MainActivity.kt`
- `android/app/src/main/java/com/anonymous/reactnativeexponativewindmmkv/MainApplication.kt`
  - `package com.anonymous.reactnativeexponativewindmmkv` – update to your new package (e.g. `package com.yourcompany.yourapp`).
  - Optionally move these files into a matching folder structure under `java/` (e.g. `com/yourcompany/yourapp`).

- `android/settings.gradle`
  - `rootProject.name = 'react-native-expo-nativewind-mmkv'` – change this to your new project name to adjust how it appears in Android Studio.

#### c) Lockfile (optional but recommended)

- `package-lock.json`
  - Top-level `"name"` and inner package `"name"` were set to `"react-native-expo-nativewind-mmkv"`.
  - You can either update them manually or simply delete the lockfile and regenerate:

    ```bash
    del package-lock.json   # Windows cmd
    npm install
    ```

#### d) Verify the renamed app builds

After renaming everything above, run a clean Android build:

```bash
npm run android
```

If it installs and launches on the emulator / device, your rename is consistent and you’re good to continue development under the new app identity.

---

## MMKV (v4 beta) Setup

This template already includes:

```jsonc
// package.json
"react-native-mmkv": "^4.0.0-beta.10"
```

The v4 beta API is:

```ts
import { MMKV } from 'react-native-mmkv';

const storage = new MMKV();

storage.set('user', 'John Doe');
const user = storage.getString('user');
```

You can create your own `storage` helper, for example:

```ts
// utils/storage.ts
import { MMKV } from 'react-native-mmkv';

export const storage = new MMKV();
```

MMKV is autolinked by React Native on Android via the existing Gradle / autolinking setup in [`android`](android).

---

## Styling with NativeWind + Tailwind v4

Tailwind v4 is wired via CSS and PostCSS:

- [`global.css`](global.css):

  ```css
  @import 'tailwindcss/theme.css' layer(theme);
  @import 'tailwindcss/preflight.css' layer(base);
  @import 'tailwindcss/utilities.css';

  @import 'nativewind/theme';
  ```

- [`postcss.config.mjs`](postcss.config.mjs):

  ```js
  export default {
    plugins: {
      '@tailwindcss/postcss': {},
    },
  };
  ```

Example component: [`components/Button.tsx`](components/Button.tsx)

```tsx
import { forwardRef } from 'react';
import { Text, TouchableOpacity, TouchableOpacityProps, View } from 'react-native';

type ButtonProps = {
  title: string;
} & TouchableOpacityProps;

export const Button = forwardRef<View, ButtonProps>(({ title, ...touchableProps }, ref) => {
  return (
    <TouchableOpacity
      ref={ref}
      {...touchableProps}
      className={`${styles.button} ${touchableProps.className}`}>
      <Text className={styles.buttonText}>{title}</Text>
    </TouchableOpacity>
  );
});

Button.displayName = 'Button';

const styles = {
  button: 'items-center bg-indigo-500 rounded-[28px] shadow-md p-4 w-full',
  buttonText: 'text-white text-lg font-semibold text-center',
};
```

You style components using Tailwind utility classes in `className`.

---

## Navigation with Expo Router

Routes are file-based under [app](app):

- [`app/index.tsx`](app/index.tsx) → `/` (Home)
- [`app/details.tsx`](app/details.tsx) → `/details`
- [`app/+not-found.tsx`](app/+not-found.tsx) → 404

Home navigates to Details:

```tsx
import { router } from 'expo-router';

<Button
  title="Show Details"
  onPress={() => router.push({ pathname: '/details', params: { name: 'John' } })}
/>;
```

Details reads the param:

```tsx
import { Stack, useLocalSearchParams } from 'expo-router';

export default function Details() {
  const { name } = useLocalSearchParams();

  return (
    <>
      <Stack.Screen options={{ title: 'Details' }} />
      <ScreenContent path="screens/details.tsx" title={`Showing details for user ${name}`} />
    </>
  );
}
```

---

## Running the App (Dev)

From the project root:

```bash
npm run start      # Expo dev server
npm run web        # Web
npm run android    # Android native (requires Android setup)
npm run ios        # iOS native (on macOS, with Xcode)
```

### Run on Your Phone via Expo Go

1. Install **Expo Go** on your phone.
2. Run `npm run start` on your machine.
3. Make sure phone and PC are on the **same Wi‑Fi network**.
4. Scan the QR code from the terminal or browser dev tools using Expo Go.

Note: Expo Go is great for quick UI/dev, but MMKV and other native modules are fully supported when you run a **native / dev build** (`npm run android` or `npm run ios`).

---

## Your Setup Notes (Windows + Android / Java)

These are your detailed environment notes to help users get Android and Java configured correctly on Windows.

### 1 — Create the project (rn-new command)

For a fresh project (if you ever recreate from scratch) you used:

```bash
npx rn-new@latest expo-nativewind-template MyApp --expo-router --nativewind
cd MyApp
npm install
```

This repo is already created and configured, so you **don’t need to run this again** when using this template, but it’s here for reference.

---

### 2 — MMKV (original note)

Previously you used:

```bash
npm install react-native-mmkv@2.12.2-beta.4
```

In this template, we’ve standardized on:

```jsonc
"react-native-mmkv": "^4.0.0-beta.10"
```

Use the v4 API (`new MMKV()` as shown above). The earlier version line is kept here for history/reference.

---

### 3 — Android Native Build Notes for MMKV

MMKV can be sensitive to ABI configuration. If you hit missing native library errors on some devices, you can customize NDK filters.

Current template config in [`android/app/build.gradle`](android/app/build.gradle):

```gradle
defaultConfig {
  applicationId 'com.anonymous.reactnativeexponativewindmmkv'
  ...
  ndk {
    abiFilters "armeabi-v7a", "arm64-v8a", "x86_64"
  }
}
```

If a particular MMKV build requires excluding `armeabi-v7a`, you can adjust that list accordingly, for example:

```gradle
ndk {
    abiFilters "arm64-v8a", "x86_64"
}
```

Only change this if you actually see ABI-related build/run errors.

---

### 4 — Required Android + Java Prerequisites (Windows)

#### ✔ Install Java

- Use **Java 17** (not Java 21, not Java 8).
- Recommended: **Eclipse Temurin JDK 17**.
- Example install path:

  ```text
  C:\Program Files\Eclipse Adoptium\jdk-17.0.17.10-hotspot
  ```

- Set as a **System environment variable**:

  ```text
  JAVA_HOME = C:\Program Files\Eclipse Adoptium\jdk-17.0.17.10-hotspot
  ```

---

#### ✔ Install Android Studio

During setup make sure you install:

- Android SDK Platform
- SDK Platform 34 or 35
- Android Virtual Device (AVD)
- NDK (any version compatible with Expo SDK 54 is fine)

---

#### ✔ Create `ANDROID_HOME`

As a **User variable**:

```text
ANDROID_HOME = C:\Users\<YOUR_USERNAME>\AppData\Local\Android\Sdk
```

For you this was:

```text
ANDROID_HOME = C:\Users\johnm\AppData\Local\Android\Sdk
```

---

#### ✔ Add Required Entries to PATH

Edit your **User PATH** and add:

```text
%ANDROID_HOME%\platform-tools
%ANDROID_HOME%\cmdline-tools\latest\bin
%ANDROID_HOME%\tools
%ANDROID_HOME%\tools\bin
%ANDROID_HOME%\emulator
```

Restart your terminal after applying these changes.

---

#### 🔍 Verify Installation

Run:

```bash
java --version
adb --version
sdkmanager --version
```

If all three respond correctly, your environment is set up.

---

### 5 — Run on Android Emulator

1. Start an emulator from **Android Studio**.
2. From the project root:

   ```bash
   npm run android
   ```

- First native build: **15–25 minutes** (downloads & compiles).
- Subsequent builds: **2–5 minutes**.
- Hot reload / Fast Refresh works; you **do not** need to rebuild for every JS change.

---

### 6 — Run on a Real Android Phone

1. On your phone:
   - Go to **Settings → About phone**.
   - Tap **Build number** 7 times to enable **Developer options**.
2. In **Developer options**, enable:
   - `USB debugging`
   - `Install via USB`
3. Connect phone to PC via USB (use “File Transfer / MTP” mode).
4. Check connection:

   ```bash
   adb devices
   ```

   - If the phone shows as `unauthorized`, unlock it and tap **Allow USB debugging**.

5. Install and run the app:

   ```bash
   npm run android
   ```

The app is installed directly on the device and supports Fast Refresh / live reload.

---

### Build Times Summary

| Platform   | First Native Build | Subsequent Builds |
| ---------- | ------------------ | ----------------- |
| Emulator   | 15–25 min          | 2–5 min           |
| Real phone | 15–25 min          | 2–5 min           |

You **do not** need to rebuild after every code change — hot reload works on both emulator and real devices.

---

## Linting & Formatting

- Lint:

  ```bash
  npm run lint
  ```

- Format (ESLint fix + Prettier):

  ```bash
  npm run format
  ```

ESLint config: [`eslint.config.js`](eslint.config.js)  
Prettier config: [`prettier.config.js`](prettier.config.js)

---

## License / Usage

Use this template freely as a starting point for your own applications. Add your own license file if you intend to distribute your app or this template.
