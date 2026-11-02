# Expo & EAS Interview Questions and Answers

**For React Native developers with 3+ years of Expo experience**

A consolidated interview-preparation guide covering Expo fundamentals, configuration, navigation, native APIs, Firebase, networking, performance, testing, architecture, EAS Build/Submit/Update, credentials, environments, release operations, and CLI commands.

**How to use this guide:** Practise the answers aloud, then add examples from your own projects. For experience-based questions, explain the problem, your decision, implementation, and result.

---

## 1. Expo Fundamentals

### 1. What is Expo in React Native?

Expo is a framework and platform built around React Native. It provides a managed set of APIs, tooling, development builds, EAS services, and libraries that simplify building, testing, and shipping iOS and Android apps.

### 2. What is the difference between React Native CLI and Expo?

React Native CLI gives direct control over native projects from the start. Expo adds curated tooling and APIs, config plugins, Expo Go, development builds, and EAS. Expo projects can still contain and customize native iOS and Android code.

### 3. What is the managed workflow?

Historically, it meant developing without manually maintaining ios and android folders, with Expo generating native projects when needed. Today, Continuous Native Generation (CNG) describes this model more accurately.

### 4. What is the bare workflow?

It is a project where native iOS and Android projects are present and maintained directly. Expo modules and EAS can still be used in this workflow.

### 5. What is Continuous Native Generation (CNG)?

CNG generates native projects from app config, config plugins, and dependencies. The generated ios/android folders can be recreated consistently, reducing manual native-project drift.

### 6. What is Expo Go?

Expo Go is a prebuilt app that loads a development JavaScript bundle and includes a fixed set of native modules. It is useful for quick prototyping but cannot load arbitrary custom native modules.

### 7. What is a development build?

A development build is your own app binary containing the native modules and configuration your project needs, plus developer tooling. It supports custom native code and is the recommended development environment for production-style apps.

### 8. Expo Go vs development build?

Expo Go is a shared generic client with a fixed native runtime. A development build is generated for your app and can include custom native dependencies, config plugins, and native configuration.

### 9. What is Expo SDK?

The Expo SDK is a versioned collection of Expo libraries and tooling designed to work with a particular React Native version. Keeping SDK and package versions compatible avoids native and JavaScript mismatches.

### 10. How do you check SDK compatibility?

Use `npx expo install --check` to identify package version mismatches and `npx expo-doctor` to inspect common project configuration issues. Follow the upgrade guide for the target SDK.

### 11. What is `expo install`?

It installs a package version compatible with the project's Expo SDK. For Expo modules, prefer `npx expo install package-name` over an unconstrained package install.

### 12. What is `expo-doctor`?

It runs diagnostics for dependency compatibility and common Expo configuration problems. I use it after upgrades and before creating release builds.

### 13. What is app config?

The app config (`app.json` or `app.config.js/ts`) describes app identity, icons, splash screen, plugins, permissions, platform settings, runtime version, and other build-time configuration.

### 14. app.json vs app.config.js?

app.json is static JSON. app.config.js/ts can compute configuration from environment variables or other logic, which is useful for multiple environments and build profiles.

### 15. What is the Expo config plugin?

A config plugin modifies native project files during prebuild. It allows libraries or teams to apply native settings reproducibly rather than hand-editing generated projects.

### 16. What is `prebuild`?

`npx expo prebuild` generates native iOS and Android projects from app config and config plugins. It is useful when native projects are needed for local builds or native customization.

### 17. What is `prebuild --clean`?

It deletes and regenerates native project directories. It can resolve stale generated configuration, but manual changes inside those folders may be lost, so preserve customizations in config plugins.

### 18. What is the difference between `expo start` and `expo run`?

`npx expo start` starts the Metro development server. `npx expo run:android` or `run:ios` builds and launches the native app locally, usually using the generated native projects.

### 19. What is Metro?

Metro is the JavaScript bundler used by React Native. It resolves modules, transforms JavaScript and assets, and serves bundles to the app during development.

### 20. How do you clear Metro cache?

Run `npx expo start -c`. If the issue persists, reinstall dependencies and verify the lockfile and package versions before deleting generated folders.

---

## 2. Project Setup, Configuration & Build

### 21. How do you create an Expo project?

Use `npx create-expo-app@latest my-app`, install dependencies with the package manager, then start with `npx expo start`. I choose a template based on whether the app needs Expo Router or a minimal entry point.

### 22. What is the entry point in an Expo app?

The entry point is configured in package.json. With Expo Router it is commonly `expo-router/entry`; otherwise it may point to `node_modules/expo/AppEntry.js` or a custom entry file.

### 23. What is the purpose of `package.json`?

It defines scripts, dependencies, package metadata, and the app entry point. The lockfile records exact dependency resolutions for reproducible installs.

### 24. Why should package manager lockfiles be committed?

They make dependency resolution consistent across developer machines and CI. Mixing npm, Yarn, and pnpm lockfiles can cause inconsistent installs.

### 25. What is Hermes?

Hermes is a JavaScript engine optimized for React Native. It can improve startup time and memory use through bytecode compilation and engine-specific optimizations.

### 26. How do you configure app name and bundle identifiers?

Set `name` and `slug`, plus `ios.bundleIdentifier` and `android.package` in app config. These identifiers must remain stable for store updates to the same app.

### 27. How do you configure app icons and splash screens?

Set the icon and splash-screen assets in app config and configure the relevant plugin or SDK-supported fields. Verify dimensions, transparency, platform behavior, and test on real devices.

### 28. How do you configure permissions?

Use the relevant package config plugin and platform-specific permission descriptions or declarations. Request runtime permission only when the feature is needed and explain why it is needed.

### 29. What is `extra` in app config?

`extra` exposes non-secret app configuration to the JavaScript runtime through Expo Constants. It is suitable for public configuration, not private secrets.

### 30. How do you manage dev, staging, and production configuration?

Use EAS build profiles and EAS environment variables, with distinct API endpoints and app identifiers where appropriate. Avoid embedding secrets in the client bundle.

### 31. What is `Constants.expoConfig`?

It exposes parts of the resolved Expo app configuration at runtime. I use it for public configuration values and metadata, while checking SDK-specific API guidance.

### 32. What causes native dependency mismatch?

A JavaScript package may expect native code that is absent from the installed binary. Rebuild the development or production app after adding or changing native dependencies.

### 33. When should you rebuild a development client?

After adding or upgrading native modules, changing config plugins, permissions, native identifiers, or other native configuration. Pure JavaScript changes generally do not require a new binary.

### 34. What is a release build?

A release build is a production-optimized native binary intended for internal distribution or app-store submission. It uses production signing and release configuration.

### 35. How do you test on a physical device?

Install Expo Go for supported prototypes or install a development build for custom native modules. Test permissions, push notifications, camera, audio, and background behavior on real hardware.

---

## 3. Expo Router & Navigation

### 36. What is Expo Router?

Expo Router is file-based routing for Expo and React Native. Files and folders define routes, layouts, nested navigation, and URL paths.

### 37. How does file-based routing work?

A file such as `app/profile.tsx` defines a profile route. Folder layouts define navigation structure, and route groups organize screens without necessarily changing the URL.

### 38. What is `_layout.tsx`?

It defines a shared layout and navigator for routes in that directory. It is commonly used for Stack, Tabs, providers, authentication gates, and shared screen options.

### 39. What is a route group?

A folder wrapped in parentheses, such as `(tabs)`, groups routes without adding that folder name to the URL. It is useful for organizing authenticated and unauthenticated flows.

### 40. How do you navigate between screens?

Use `Link` for declarative navigation and `useRouter()` for programmatic navigation such as `router.push('/profile')` or `router.replace('/login')`.

### 41. push vs replace?

`push` adds a new route to the navigation stack. `replace` swaps the current route, which is useful after login or onboarding when the user should not return to the previous screen with Back.

### 42. How do you pass route parameters?

Use dynamic route segments such as `[id].tsx` and read parameters with `useLocalSearchParams()`. Validate and normalize parameters because route values are external input.

### 43. What are dynamic routes?

A file named `[id].tsx` matches a path segment such as `/users/42`. Catch-all routes use bracket syntax for multiple segments.

### 44. How do you implement protected routes?

Check the authentication state in a layout or routing layer and redirect unauthenticated users. Keep authorization enforcement on the backend; client route guards are not security boundaries.

### 45. How do deep links work?

A URL maps to an app route through the linking configuration and platform association files. Configure scheme, Android intent filters, and iOS associated domains for universal links where required.

### 46. What is a navigation stack?

A stack maintains a history of screens. Pushing adds a screen, popping returns to the previous one, and replacing swaps the current screen.

### 47. How do you handle not-found routes?

Use Expo Router's not-found route support, typically `+not-found.tsx`, to show a fallback when no route matches.

### 48. How do tabs work in Expo Router?

Define a Tabs navigator in a layout and add screen files under that route group. Configure labels, icons, and screen options centrally.

### 49. How do you handle authentication loading?

Keep an explicit loading state while restoring the session. Render a splash/loading screen until the auth state is known, then route to the correct group to avoid screen flicker.

### 50. How do you handle navigation after logout?

Clear the session and sensitive local state, then replace the current route with the login route. Also invalidate server-side refresh tokens when supported.

---

## 4. Device APIs & Expo Modules

### 51. What are Expo modules?

Expo modules are native modules exposed to JavaScript through Expo's module system. They provide access to platform capabilities while supporting Expo and non-Expo React Native projects.

### 52. How do you use the camera?

Install `expo-camera`, request permission, render the camera view, and capture images using its API. Handle denied permissions and test on a physical device.

### 53. How do you select images?

Use `expo-image-picker`, request media-library permission when needed, launch the picker, and handle cancellation and returned asset metadata.

### 54. How do you access location?

Use `expo-location`, request foreground or background permission as appropriate, and retrieve current or watched location. Explain the purpose and minimize collection.

### 55. How do you store small local values?

Use AsyncStorage for non-sensitive key-value data. For credentials or tokens, use secure storage such as `expo-secure-store` rather than plain AsyncStorage.

### 56. What is SecureStore?

Expo SecureStore stores small sensitive values using platform-provided secure storage mechanisms, such as Keychain on iOS and encrypted storage backed by Android Keystore.

### 57. AsyncStorage vs SecureStore?

AsyncStorage is general persistent storage and is not intended for secrets. SecureStore is intended for small sensitive values such as refresh tokens, though it has platform and size limitations.

### 58. How do you use the clipboard?

Use `expo-clipboard` to read or write clipboard content. Avoid copying secrets unnecessarily and provide user feedback after copy actions.

### 59. How do you use the device contacts?

Use `expo-contacts`, request permission, and fetch only the fields required by the feature. Contacts are sensitive data and should not be uploaded without clear user consent.

### 60. How do you detect network connectivity?

Use `@react-native-community/netinfo` or an appropriate Expo-compatible library to observe connection state. Network connectivity does not guarantee that an API is reachable.

### 61. How do you handle file system operations?

Use the current Expo file-system APIs to read, write, download, and manage files. Check SDK-specific API changes because the file-system package has evolved.

### 62. How do you share content?

Use `expo-sharing` to open the platform share sheet for a local file or supported URI. Check whether sharing is available on the platform.

### 63. How do you play audio?

Use the maintained audio library appropriate to the SDK, such as `expo-audio` in newer projects. Configure audio mode, handle lifecycle interruptions, and release resources when playback ends.

### 64. How do you record voice messages?

Request microphone permission, configure audio mode, start and stop recording, save the resulting file, then upload it to storage. Handle interruptions, cancellation, file size, and upload failures.

### 65. What is the difference between foreground and background location?

Foreground location is used while the app is active. Background location continues under platform-specific restrictions and requires additional permissions, configuration, and a clear user-facing purpose.

### 66. How do you handle app permissions being denied?

Explain the feature's need, provide a graceful fallback, and offer a path to system settings if permission is permanently denied. Never repeatedly prompt without context.

### 67. How do you handle app lifecycle changes?

Use AppState or the relevant module lifecycle APIs to pause work, refresh stale data, and manage media or connections when the app moves between active and background states.

### 68. What is a config plugin used for with a native module?

It applies required native settings during prebuild, such as permissions, plist entries, manifest declarations, or Gradle configuration. This keeps native setup reproducible.

---

## 5. Notifications, Firebase & Authentication

### 69. How do push notifications work in Expo?

The app obtains a push token, sends it to the backend, and the backend sends notifications through Expo Push Service or a provider such as FCM/APNs. The app handles permission, token lifecycle, and notification responses.

### 70. What is an Expo push token?

It is a token identifying an app installation for delivery through Expo's push service. Store it against the user/device on the backend and update or remove it as it changes.

### 71. Expo push token vs FCM token?

An Expo push token is used with Expo Push Service. An FCM registration token is used directly with Firebase Cloud Messaging. They are different token types and should not be interchanged.

### 72. Can Expo Go receive remote push notifications?

Support depends on the Expo SDK and platform. For current Android SDK workflows, remote push notifications require a development build; use a development or production build for reliable testing rather than assuming Expo Go support.

### 73. How do you configure Android push notifications?

Configure Firebase credentials and the required Android notification setup, including the FCM service configuration. Build an app binary containing the native configuration and test on a physical device.

### 74. How do you configure iOS push notifications?

Configure the Apple push capability and APNs credentials, enable the required app identifier capability, and ensure the correct provisioning profile and build configuration are used.

### 75. How do you handle notification taps?

Register notification response listeners, inspect the payload, validate its data, and navigate to the intended screen. Handle cold-start launches as well as taps while the app is running.

### 76. What is a notification channel on Android?

A channel groups notifications by category and controls user-visible behavior such as importance and sound. Create channels before posting notifications on Android 8 and later.

### 77. How do you avoid duplicate push notifications?

Use stable notification/event identifiers, deduplicate on the backend or client, and make token registration idempotent. Avoid sending both provider and local notifications for the same event.

### 78. How do you integrate Firebase Authentication?

Initialize Firebase with the app configuration, use the appropriate sign-in provider, obtain the ID token, and send it to the backend for verification. Persist only what is needed and handle token refresh.

### 79. How do you verify a Firebase ID token on the backend?

Use the Firebase Admin SDK to verify the ID token, then derive the authenticated user identity from the verified claims. Never trust a user ID supplied only by the client.

### 80. How do you implement Google sign-in?

Configure the OAuth client IDs and platform setup, use a compatible sign-in library, obtain the provider credential or ID token, and validate it through Firebase or the backend.

### 81. How do you implement refresh-token authentication?

Keep access tokens short-lived and refresh tokens protected. On expiry, call a refresh endpoint, rotate tokens when supported, retry the original request once, and log out if refresh fails.

### 82. How do you prevent multiple simultaneous refresh requests?

Use a shared refresh promise or request queue. Requests wait for one refresh operation, then retry with the new access token instead of triggering a refresh storm.

### 83. Where should auth tokens be stored?

Use secure platform storage for refresh tokens and avoid logging tokens. Keep access tokens in memory where practical, and define logout and token-rotation behavior.

### 84. How do you handle OAuth redirect URLs?

Configure the app scheme and provider redirect URI consistently across the identity provider and app. Validate state/PKCE where supported and test cold-start deep-link handling.

### 85. How do you handle logout?

Clear secure tokens and in-memory auth state, unregister or disassociate device tokens where appropriate, invalidate refresh tokens server-side, and reset user-specific cached data.

### 86. How do you secure API requests?

Use HTTPS, attach tokens through a centralized client/interceptor, validate authorization on the server, avoid sensitive logs, and handle expiry and retries carefully.

---

## 6. Networking, State & Data

### 87. How do you call REST APIs in Expo?

Use fetch or Axios through a centralized API layer. Centralize base URLs, headers, timeout behavior, error normalization, and authentication refresh.

### 88. What is an Axios interceptor?

An interceptor runs before a request or after a response. It is useful for attaching tokens, logging safe metadata, normalizing errors, and coordinating token refresh.

### 89. How do you handle API errors?

Distinguish network failures, timeouts, validation errors, authorization failures, and server errors. Show actionable UI feedback and preserve enough context for debugging without exposing secrets.

### 90. How do you implement pagination?

Use cursor-based pagination where possible, track the next cursor, prevent duplicate requests, and merge results by stable ID. Reset pagination when filters change.

### 91. How do you implement pull-to-refresh?

Use RefreshControl or the list's refresh props, set a refreshing state, reload the first page, and reset pagination and stale error states.

### 92. How do you support offline mode?

Cache required data locally, queue supported writes, detect connectivity changes, and synchronize when online. Define conflict resolution and make retries idempotent.

### 93. What is optimistic UI?

The UI updates before the server confirms the operation. If the request fails, revert or reconcile the optimistic state and inform the user.

### 94. Redux Toolkit vs Context API?

Context is suitable for relatively simple shared state. Redux Toolkit provides predictable global state, reducers, middleware, devtools, and scalable patterns for complex application state.

### 95. What is RTK Query?

RTK Query is Redux Toolkit's data-fetching and caching solution. It manages request lifecycle, caching, invalidation, polling, and generated hooks.

### 96. How do you persist Redux state?

Persist only necessary non-sensitive state and rehydrate it carefully. Authentication secrets should use secure storage, not generic persisted Redux state.

### 97. How do you avoid stale API responses?

Cancel obsolete requests where possible, use request IDs or query keys, and ensure only the latest relevant response updates the screen.

### 98. How do you upload an image to Firebase Storage?

Pick or capture the image, obtain a local URI, convert or stream it using a supported upload API, then retrieve the download URL. Check storage rules, MIME type, file size, and network errors.

### 99. How do you upload audio to Firebase Storage?

Record the audio file, validate its path and MIME type, upload using the storage SDK, and store the resulting URL and metadata in the database. Handle retries and cleanup of abandoned uploads.

### 100. What can cause Firebase Storage `storage/unknown`?

It is a generic error that can hide network, auth, rules, URI, MIME, or SDK issues. Inspect the full error code/message, verify Firebase initialization and Storage rules, test a known local file, and check network and app credentials.

### 101. How do you handle token expiry in API calls?

Detect 401 responses, run a single coordinated refresh operation, retry the original request once, and clear the session if refresh fails or the refresh token is invalid.

---

## 7. UI, Performance & Reliability

### 102. How do you optimize FlatList?

Use stable keys, memoized row components, `getItemLayout` for fixed-height rows, appropriate windowing settings, and avoid recreating expensive render props.

### 103. ScrollView vs FlatList?

ScrollView renders all children and is appropriate for small content. FlatList virtualizes large lists and is generally better for long or dynamic collections.

### 104. What is virtualization?

Virtualization renders only the visible items and a small buffer, reducing memory use and rendering work for large lists.

### 105. How do React.memo and useMemo help?

React.memo can skip child renders when props are unchanged. useMemo caches an expensive computed value; use it when profiling shows meaningful work, not everywhere.

### 106. What does useCallback do?

It memoizes a function reference between renders while dependencies remain unchanged. It is useful when stable callbacks prevent unnecessary child renders or effect reruns.

### 107. How do you reduce unnecessary re-renders?

Keep state close to where it is used, split components, use stable keys and props, memoize measured hotspots, and avoid putting frequently changing values in broad context providers.

### 108. How do you improve app startup time?

Reduce startup work, defer noncritical initialization, optimize assets and fonts, avoid blocking synchronous work, and measure release builds on representative devices.

### 109. How do you reduce app bundle size?

Remove unused dependencies and assets, optimize images, avoid duplicate libraries, and use platform-specific or lazy loading patterns where appropriate. Check the actual native artifact size.

### 110. How do you handle image performance?

Use appropriately sized images, caching, thumbnails, modern formats where supported, and avoid decoding very large images for small UI elements.

### 111. How do you prevent memory leaks?

Clean up subscriptions, timers, event listeners, animation loops, and media resources. Cancel in-flight work when screens unmount or become irrelevant.

### 112. How do you debug a frozen UI?

Profile JavaScript and UI thread activity, inspect expensive renders and synchronous loops, and move CPU-heavy work off the main JS path where appropriate.

### 113. What is the JS thread?

It executes JavaScript application logic and React rendering work. Heavy JS work can delay event handling and cause dropped frames.

### 114. What is the UI thread?

It handles native view operations and much of the platform's UI work. Excessive work on either the JS or UI thread can harm responsiveness.

### 115. How do you handle app crashes?

Collect crash logs and device/OS details, reproduce with a release-like build, inspect native and JavaScript stack traces, isolate recent changes, and add regression tests.

### 116. How do you handle Android back button?

Use navigation's built-in back handling where possible. For custom behavior, register a BackHandler listener and remove it during cleanup.

### 117. How do you support different screen sizes?

Use flexible layouts, dimensions, safe-area insets, responsive spacing, and test small phones, large phones, and tablets. Avoid hardcoding absolute positions unnecessarily.

### 118. What are safe area insets?

They describe areas obscured by system UI such as notches and home indicators. Use safe-area context components/hooks to keep content visible.

### 119. How do you improve accessibility?

Provide labels and roles, support screen readers, maintain sufficient contrast and touch target sizes, respect font scaling, and test with VoiceOver and TalkBack.

### 120. How do you handle app backgrounding during upload?

Use an upload strategy appropriate to platform background limits, persist upload state, resume or retry when the app returns, and clearly communicate incomplete uploads.

### 121. How do you measure performance?

Use React Native profiling tools, platform profilers, performance monitors, and production telemetry. Measure on release builds and compare before/after results.

---

## 8. Testing, Debugging & Release

### 122. How do you test an Expo app?

Use Jest and React Native Testing Library for unit/component tests, integration tests for flows, and device testing for native behavior. Test both development and release builds.

### 123. What is React Native Testing Library?

It tests components through user-observable behavior rather than implementation details. It encourages accessible queries and realistic interaction tests.

### 124. How do you mock Expo modules in Jest?

Use the package's documented Jest mock or provide a focused mock for the native API. Keep mocks representative and add device-level tests for behavior that mocks cannot validate.

### 125. What is EAS Build?

EAS Build is Expo's cloud build service for producing Android and iOS binaries. It manages build environments and can handle signing credentials.

### 126. What is EAS Submit?

EAS Submit uploads built app binaries to app-store distribution systems such as App Store Connect and Google Play. Submission does not mean the app is automatically approved or released.

### 127. What is EAS Update?

EAS Update publishes compatible JavaScript and asset updates to installed app binaries. It cannot replace native code or native configuration that requires a new binary.

### 128. What is OTA update?

An over-the-air update delivers compatible app JavaScript and assets without users installing a new store binary. It must match the installed app's runtime compatibility.

### 129. What is runtimeVersion?

It identifies the native runtime compatibility of an app binary. Updates are delivered only to binaries with a compatible runtime version.

### 130. How do you debug a production-only issue?

Reproduce with a release build and production-like configuration, inspect crash and telemetry data, verify environment variables and native settings, and compare the exact build/runtime versions.

### 131. What is source map usage?

Source maps map minified production stack traces back to original source locations. Upload them to the monitoring service securely to make crash reports actionable.

### 132. How do you manage app versioning?

Maintain user-facing version and platform build numbers. Increment build numbers for store uploads and use a consistent release strategy across environments.

### 133. How do you create an Android APK?

Configure an EAS build profile with an APK build type if direct installation is required, then run `eas build -p android --profile <profile>`. AAB is generally used for Play Store distribution.

### 134. APK vs AAB?

APK is directly installable. AAB is a publishing format from which Google Play generates optimized APKs for devices; it is the standard Play Store upload format.

### 135. How do you create an iOS build?

Configure the iOS bundle identifier and signing credentials, then run `eas build -p ios --profile <profile>`. Distribution type depends on the profile and intended audience.

### 136. How do you submit to stores?

Build a signed production artifact, validate metadata and compliance, then use EAS Submit or the store console. Review, phased release, and approval remain store-controlled processes.

---

## 9. Architecture, Security & Scenario Questions

### 137. How would you structure a large Expo app?

Separate features, shared UI, navigation, API/data access, state, hooks, and platform-specific code. Keep business logic out of screens and define clear module boundaries.

### 138. How do you handle environment secrets?

Use EAS environment variables for build/update configuration, but remember values embedded in a client app can be extracted. Keep private API keys and credentials on the backend.

### 139. How do you secure local data?

Store only necessary data, use SecureStore for small secrets, encrypt sensitive files where needed, clear data on logout, and protect server-side resources with authorization.

### 140. How do you prevent unauthorized deep-link access?

Validate route parameters and check authentication/authorization before showing protected data. The backend must enforce access control regardless of navigation guards.

### 141. How do you handle a failed deployment?

Stop promotion, identify the affected build/update and channel, inspect logs and runtime compatibility, then publish a corrected update or roll back using the supported EAS update controls.

### 142. A new native package works in Expo Go but fails in a development build. What do you check?

Check package compatibility, config plugin setup, native build logs, autolinking, and SDK versions. Rebuild the development client after native dependency changes.

### 143. Push notifications work on Android but not iOS. How do you debug?

Verify APNs capability and credentials, bundle identifier, provisioning profile, device token registration, environment, and notification permission. Test on a physical device and inspect provider responses.

### 144. An OTA update is not appearing. What do you check?

Verify channel-to-branch mapping, runtimeVersion compatibility, update URL/project ID, publication status, network access, and whether the app checks for updates.

### 145. The app works in development but crashes in release. What do you do?

Reproduce the exact release profile, inspect native crash logs, verify minification and environment configuration, check native permissions and signing, and isolate changes with a minimal build.

### 146. How would you implement offline-first chat?

Persist conversations locally, queue outgoing messages with client-generated IDs, sync when connected, deduplicate server acknowledgements, and define ordering and conflict behavior.

### 147. How would you implement voice messages?

Request microphone access, record with an audio module, upload to cloud storage, save message metadata in the database, and show upload/playback state with retry and cleanup handling.

### 148. How do you handle token refresh across multiple API calls?

Use one shared refresh operation and queue pending requests. Retry each request once with the refreshed token and reject all queued requests if refresh fails.

### 149. How do you roll out a risky feature safely?

Use remote feature flags, staged rollout, telemetry, and a rollback plan. Keep the native runtime compatible and avoid shipping an untested native dependency change.

### 150. How do you handle an app store rejection?

Read the exact review reason, reproduce the issue, fix the product or metadata, verify on a clean build, and resubmit with a concise explanation when needed.

### 151. How do you explain your Expo experience in an interview?

Describe the app, your ownership, navigation and native APIs used, backend/auth integration, build and release process, performance or reliability improvements, and one concrete production issue you solved.

---

## 10. EAS Build, Submit, Update & Operations

### 152. What does EAS stand for?

Expo Application Services. It is a set of cloud services and tooling for building, submitting, and updating Expo and React Native applications.

### 153. What services are included in EAS?

The core services include EAS Build for native binaries, EAS Submit for store uploads, and EAS Update for compatible over-the-air updates. EAS also provides workflow and metadata-related tooling.

### 154. What is eas.json?

It is the EAS CLI configuration file. It defines build profiles, distribution settings, environment selection, channels, credentials-related options, and other build behavior.

### 155. What is a build profile?

A named configuration under `build` in eas.json, such as development, preview, or production. Each profile can define distribution, development client, environment, channel, and platform-specific settings.

### 156. What is the difference between development, preview, and production profiles?

Development usually creates a development client with debugging tools. Preview creates an internal QA build. Production creates a store-ready release build with production settings.

### 157. What is `developmentClient: true`?

It configures a build to include expo-dev-client, producing a custom development app that can load the project's JavaScript bundle and native modules.

### 158. What is internal distribution?

It distributes builds directly to testers, typically through an installable link or platform-specific internal mechanism, without requiring a public store release.

### 159. When would you use APK instead of AAB?

Use APK for direct Android installation and internal testing when appropriate. Use AAB for Google Play production distribution in the normal publishing workflow.

### 160. What is EAS Submit used for?

It uploads an already-built binary to App Store Connect or Google Play. Store metadata, review, and release controls are handled separately.

### 161. Does EAS Submit publish the app to users immediately?

No. It uploads the artifact to the store. The app may still need metadata completion, review, approval, and a manual or scheduled release.

### 162. What are EAS environment variables?

They are environment-specific values available to EAS builds and updates. Classify them by visibility and never treat a value embedded in a client bundle as secret.

### 163. What is the difference between plain text and sensitive environment variables?

Plain text values can be visible to authorized project users and build logs depending on use. Sensitive values receive stronger handling in EAS, but once embedded in the app they are still extractable.

### 164. How do you select an environment for a build?

Set the `environment` field in the build profile in eas.json, then run the build with that profile. Confirm that the expected variables are available to the build.

### 165. What are EAS credentials?

They are signing materials used to create trusted platform binaries, such as Android keystores and iOS certificates/provisioning profiles. EAS can manage or use credentials you provide.

### 166. What is Android keystore?

It is the signing key material used to sign Android app artifacts. The signing identity must remain consistent for updates to an existing app, subject to Play App Signing arrangements.

### 167. What is iOS provisioning profile?

It authorizes an app to run on specified devices or be distributed through a particular channel, and ties together the app identifier, certificates, and entitlements.

### 168. What is the difference between development and distribution certificates?

Development signing is for running/debugging on registered devices. Distribution signing is for distributing builds through the intended Apple distribution channel.

### 169. What happens if you lose your Android keystore?

If Play App Signing is enabled, you may be able to reset the upload key through Play Console while Google retains the app signing key. Without Play App Signing, losing the original signing key can prevent updates under the same app identity.

### 170. What is EAS Update?

It publishes JavaScript bundles and assets to an update branch. Compatible installed binaries can download the update without a new store release.

### 171. What cannot be changed using EAS Update?

Native code, native dependencies, native permissions, app entitlements, and other changes requiring a new native binary cannot be delivered by OTA update.

### 172. What is an EAS Update channel?

A channel is configured in a build and points to an update branch. It lets binaries receive updates from the intended release stream, such as preview or production.

### 173. What is an update branch?

A branch is a stream of published updates. A channel maps installed builds to a branch, allowing the team to control which updates each build receives.

### 174. Channel vs branch?

A channel is embedded in the native build and maps to a branch. A branch contains published updates. This separation lets a channel's target branch be changed without rebuilding every app.

### 175. What is runtimeVersion in EAS Update?

It marks native compatibility. An update is eligible only for installed binaries with a matching runtime version, preventing incompatible JavaScript from being loaded into a different native runtime.

### 176. How do you configure runtime version?

Use the `runtimeVersion` app config setting and choose a policy or explicit versioning strategy that matches your native-change process. Verify the selected policy against the Expo SDK guidance.

### 177. What is the fingerprint runtime policy?

It derives runtime compatibility from the native project fingerprint. Native dependency or configuration changes can produce a different fingerprint and therefore a different update runtime.

### 178. How do you publish an update?

Use `eas update --channel <channel> --message "..."` after verifying the intended environment, channel, and runtime compatibility. Monitor adoption and errors after publishing.

### 179. How do you roll back an EAS Update?

Publish a known-good update to the same intended branch/channel or use the supported update-revert workflow for the project. Verify the rollback targets the correct runtime and audience.

### 180. What is an update group?

It is a published set of platform-specific updates associated with one update publication. It helps track the related Android and iOS update artifacts.

### 181. How do you view builds?

Use `eas build:list` to list builds and `eas build:view <build-id>` to inspect a particular build and its details.

### 182. How do you inspect EAS build logs?

Open the build details from the EAS dashboard or CLI output and inspect the failing build phase. Identify whether the issue is dependency installation, prebuild, native compilation, credentials, or submission.

### 183. How do you build only Android?

Run `eas build --platform android --profile <profile>`. Use the corresponding `ios` platform option for iOS.

### 184. How do you build both platforms?

Run `eas build --platform all --profile <profile>`, or start separate platform builds if different timing or troubleshooting is needed.

### 185. How do you use local builds?

Use `eas build --local` where supported by the platform and environment. Local builds are useful for debugging or using local infrastructure, but they still require the right native toolchain and credentials.

### 186. What is EAS Workflows?

It is Expo's workflow automation capability for automating tasks such as builds, submissions, and updates in response to repository events or configured workflow triggers.

### 187. How do you manage multiple app environments?

Use separate EAS profiles and environment variables, and consider distinct bundle identifiers/package names for parallel installs. Make the environment visible in the app to prevent tester confusion.

### 188. How do you protect production releases?

Restrict production credentials and environment access, require review and QA, use clear profile/channel naming, and monitor builds and updates after release.

### 189. What would you do if an EAS build fails?

Read the first meaningful error in the build logs, reproduce locally if possible, run expo-doctor, verify SDK/package compatibility and credentials, then retry after fixing the root cause rather than blindly rebuilding.

---

## 11. EAS CLI Commands & Quick Revision

### 190. How do you install or run EAS CLI?

Use `npm install -g eas-cli` to install globally, or run it through `npx eas-cli`. Check the installed CLI version with `eas --version`.

### 191. How do you configure a project for EAS?

Run `eas build:configure` from the project root. It creates or updates EAS build configuration and links the project as needed.

### 192. How do you log in to Expo?

Run `eas login`; use `eas whoami` to confirm the active account.

### 193. How do you create a development build?

Run `eas build --profile development --platform android` or the iOS equivalent, with `developmentClient: true` configured in eas.json.

### 194. How do you create a preview build?

Run `eas build --profile preview --platform all` or select one platform. Configure the profile for internal distribution if that is the intended testing path.

### 195. How do you create a production build?

Run `eas build --profile production --platform all`. Verify app identifiers, signing, environment, and version/build numbers first.

### 196. How do you submit a build?

Run `eas submit --platform android --profile production` or the iOS equivalent, selecting the correct build and store credentials.

### 197. How do you publish an OTA update?

Run `eas update --channel production --message "Fix login flow"` after confirming the target channel and runtime.

### 198. How do you list update branches?

Use `eas branch:list` to inspect available update branches.

### 199. How do you list channels?

Use `eas channel:list` to inspect configured channels and their branch mappings.

### 200. How do you inspect a channel?

Use `eas channel:view <channel-name>` to view the channel's configuration and mapped branch.

### 201. How do you list environment variables?

Use the EAS environment CLI commands, such as `eas env:list`, and select the appropriate environment. Exact command options can vary with EAS CLI versions.

### 202. How do you run project diagnostics before a build?

Run `npx expo-doctor` and `npx expo install --check`, then resolve relevant warnings before building.

### 203. What should you check before production EAS build?

Confirm SDK compatibility, app identifiers, runtimeVersion, production environment variables, signing credentials, icons/splash, permissions, version/build number, and release notes.

---

## EAS command cheat sheet

`eas.json` example:

```json
{
  "build": {
    "development": { "developmentClient": true, "distribution": "internal" },
    "preview": { "distribution": "internal", "channel": "preview" },
    "production": { "channel": "production" }
  }
}
```

| Purpose | Command |
|---|---|
| Login | `eas login` |
| Check account | `eas whoami` |
| Configure EAS | `eas build:configure` |
| Android build | `eas build -p android --profile production` |
| iOS build | `eas build -p ios --profile production` |
| Both platforms | `eas build --platform all --profile production` |
| Internal build | `eas build --profile preview --platform all` |
| Submit | `eas submit --platform android --profile production` |
| Publish OTA | `eas update --channel production --message "message"` |
| List builds | `eas build:list` |
| Inspect build | `eas build:view <build-id>` |
| List branches | `eas branch:list` |
| List channels | `eas channel:list` |
| View channel | `eas channel:view <channel>` |
| Diagnostics | `npx expo-doctor` |
| Check package versions | `npx expo install --check` |

Note: EAS CLI options evolve. Confirm exact syntax with `eas <command> --help` for the installed CLI version.

## Top 10 EAS concepts to remember

1. EAS Build creates native binaries; EAS Submit uploads them to app stores; EAS Update publishes compatible JS/assets.
2. `eas.json` defines named build profiles.
3. Development builds include your project's native dependencies and are the right choice for custom native modules.
4. Internal distribution is useful for QA without a public store release.
5. Android Play Store releases normally use AAB; direct installation commonly uses APK.
6. Signing credentials must be managed carefully; updates must preserve the app's signing identity.
7. EAS environment variables help separate development, preview, and production configuration, but client-side values are not secrets.
8. A channel is embedded in a build and maps to an update branch.
9. Runtime compatibility determines which installed binaries can receive an OTA update.
10. OTA updates cannot deliver native code or native configuration changes; those require a new binary.

## Interview closing tip

Prepare one end-to-end example: explain how you created an Expo app, structured navigation, integrated APIs/authentication and device features, generated a development build, tested a preview build, configured production credentials, submitted to stores, and safely published a compatible OTA update.
