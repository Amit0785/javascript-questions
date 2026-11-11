## Section 1: React Native fundamentals (1–15)

Q1. What is React Native, and how is it different from React.js?

Answer: React Native is a framework for building native Android and iOS applications using JavaScript or TypeScript and React concepts.

- React.js renders UI in the browser using DOM elements.
- React Native renders native UI components such as `View`, `Text`, and `Image`.
- React.js uses CSS; React Native uses JavaScript-based styling.
- React Native can access native device capabilities through native modules.

Q2. How does React Native work internally?

Answer: React Native uses React's component model to describe the UI. JavaScript executes application logic, while the native platform renders native UI components.

In the New Architecture, communication is improved through JSI, Fabric and TurboModules, reducing dependence on the old asynchronous bridge.

Q3. What is the difference between React Native CLI and Expo?

Answer:

| React Native CLI                          | Expo                                                             |
| ----------------------------------------- | ---------------------------------------------------------------- |
| More direct native project control        | Managed tooling and development services                         |
| Native configuration is developer-managed | Simplified configuration                                         |
| Native modules can be integrated directly | Supports many native modules through Expo SDK and config plugins |
| More manual build setup                   | EAS simplifies builds and submissions                            |

I use React Native CLI and Expo depending on the project's native requirements and release workflow.

Q4. What are the core components of React Native?

Answer:

- `View`: Container component.
- `Text`: Displays text.
- `Image`: Displays images.
- `TextInput`: Accepts user input.
- `ScrollView`: Scrollable content.
- `FlatList`: Efficient rendering of lists.
- `Pressable`: Handles press interactions.
- `Modal`: Displays content above the current screen.

Q5. What is JSX?

Answer: JSX is a syntax extension for JavaScript that allows us to describe UI using HTML-like syntax.

```
const Welcome = () => {
  return <Text>Hello React Native</Text>;
};
```

It is transformed into JavaScript function calls during compilation.

Q6. What is the difference between functional and class components?

Answer: Class components use ES6 classes and lifecycle methods, whereas functional components use functions and React Hooks.

I prefer functional components because they provide reusable logic through custom hooks and make component code easier to maintain.

Q7. What are React Hooks?

Answer: Hooks allow functional components to use React features such as state, lifecycle-related effects, references and context.

Common hooks:

- `useState`
- `useEffect`
- `useMemo`
- `useCallback`
- `useRef`
- `useContext`
- `useReducer`

Q8. Explain useState.

Answer: `useState` manages local component state.

```
const [count, setCount] = useState(0);

const increment = () => {
  setCount(prev => prev + 1);
};
```

When state changes, React schedules a re-render.

Q9. Explain useEffect.

Answer: `useEffect` handles side effects such as API calls, subscriptions and event listeners.

```
useEffect(() => {
  fetchData();

  return () => {
    // Cleanup
  };
}, []);
```

An empty dependency array runs the effect after the initial mount. Cleanup runs when the effect is removed or before it re-runs.

Q10. What is the difference between useEffect and useLayoutEffect?

Answer:

- `useEffect` runs after the component commits and generally does not block painting.
- `useLayoutEffect` runs synchronously after DOM/native host mutations in the relevant commit phase, before the browser paints.

I use `useEffect` for normal API calls and subscriptions. I reserve layout effects for cases requiring layout measurements or updates before visual presentation.

Q11. What is the Virtual DOM?

Answer: The Virtual DOM is a lightweight JavaScript representation of the UI. React compares the previous and next render descriptions and determines which updates are needed.

In React Native, the result is committed to native host components rather than browser DOM elements.

Q12. What is reconciliation?

Answer: Reconciliation is the process React uses to compare the previous and new element trees and determine the necessary updates.

It uses element types and keys to identify which components can be reused, updated or removed.

Q13. What is the difference between props and state?

Answer:

| Props                                   | State                                   |
| --------------------------------------- | --------------------------------------- |
| Passed from parent                      | Managed within component or state owner |
| Read-only from child perspective        | Updated through state setters           |
| Used for configuration and data passing | Used for changing application data      |

Q14. What is prop drilling, and how do you avoid it?

Answer: Prop drilling means passing data through multiple intermediate components even when they don't need it.

I avoid it using:

- Context API
- Redux Toolkit
- Custom hooks
- Component composition

Q15. What is a controlled component?

Answer: A controlled component gets its value from React state.

```
const [email, setEmail] = useState('');

<TextInput
  value={email}
  onChangeText={setEmail}
/>
```

This provides centralized control over the input value and validation.

## Section 2: React Native architecture and advanced concepts (16–30)

Q16. Explain the old React Native architecture.

Answer: The old architecture primarily consisted of:

1. JavaScript thread.
2. Native/UI thread.
3. Shadow thread for layout calculations.
4. Asynchronous Bridge for communication.

JavaScript and native code communicated through serialized messages over the bridge.

Q17. What is the React Native New Architecture?

Answer: The New Architecture modernizes React Native's internal communication and rendering system.

Its major components are:

- JSI
- Fabric
- TurboModules
- Codegen
- Concurrent React support

It improves interoperability between JavaScript and native code and provides a foundation for better performance.

Q18. What is JSI?

Answer: JavaScript Interface (JSI) is a C++ interface that allows JavaScript engines to interact with native C++ objects and functionality.

It removes the need for every native interaction to pass through the old serialized asynchronous bridge.

Q19. What is Fabric?

Answer: Fabric is React Native's new rendering system.

It improves coordination between React's rendering process and native UI views, supports modern React features and provides better interoperability with native components.

Q20. What are TurboModules?

Answer: TurboModules are the New Architecture's native module system.

They provide more efficient access to native functionality, support lazy loading and use Codegen for type-safe interfaces.

Q21. What is Codegen in React Native?

Answer: Codegen generates native interface code from typed JavaScript or TypeScript specifications.

It helps maintain consistency between JavaScript and native module or component interfaces.

Q22. What is Hermes?

Answer: Hermes is a JavaScript engine optimized for React Native.

Its benefits include:

- Optimized startup performance.
- Bytecode compilation.
- Reduced memory overhead in many workloads.
- Improved execution characteristics on mobile devices.

I consider Hermes when optimizing application startup and runtime performance.

Q23. What is the difference between JSI and the Bridge?

Answer:

| Bridge                                   | JSI                                              |
| ---------------------------------------- | ------------------------------------------------ |
| Asynchronous message-based communication | Direct interface to native C++ objects           |
| Serialization overhead                   | Avoids traditional bridge serialization          |
| Message queue based                      | Enables synchronous and asynchronous interaction |
| Old architecture                         | Foundation of New Architecture                   |

Q24. What are native modules?

Answer: Native modules allow React Native applications to access platform-specific functionality written in Kotlin, Java, Swift or Objective-C.

Examples include device sensors, specialized SDKs, native payment integrations and platform-specific services.

Q25. When would you create a custom native module?

Answer: I would create one when a required native capability is unavailable through existing React Native libraries or when a specialized native SDK needs integration.

For example, a custom module could expose a platform-specific calendar or device capability to JavaScript.

Q26. What is the Shadow Tree?

Answer: The Shadow Tree is an internal representation of the UI used for layout calculations. React Native uses Yoga to calculate layouts based on Flexbox rules.

Q27. What is Yoga in React Native?

Answer: Yoga is a cross-platform layout engine that calculates layouts using Flexbox concepts. It allows consistent layout behavior across Android and iOS.

Q28. What are React Native threads?

Answer: Important execution contexts include:

- JavaScript thread: Runs application JavaScript.
- UI/main thread: Handles native UI operations.
- Background/native threads: Perform operations such as networking and other native work.
- Shadow/layout processing: Calculates layout information.

The exact threading behavior depends on the architecture and operation.

Q29. What is the difference between synchronous and asynchronous execution?

Answer: Synchronous execution blocks the current execution flow until an operation completes. Asynchronous execution allows other work to proceed while waiting for an operation.

For example, network requests should generally be asynchronous to avoid blocking application execution.

Q30. What is CodePush or OTA updating?

Answer: OTA (Over-the-Air) updates allow compatible JavaScript bundles and assets to be delivered without publishing a new store binary.

I would use OTA only for changes permitted by the platform and distribution policy. Native dependency or binary changes still require a new application build.

## Section 3: State management and Redux Toolkit (31–45)

Q31. What is Redux?

Answer: Redux is a predictable state management library that maintains shared application state in a centralized store.

Its main concepts are:

- Store
- Actions
- Reducers
- Dispatch
- Selectors

Q32. What is Redux Toolkit?

Answer: Redux Toolkit is the recommended approach for writing Redux logic.

It simplifies Redux through:

- `configureStore`
- `createSlice`
- `createAsyncThunk`
- RTK Query
- Built-in Immer support

Q33. Explain the Redux data flow.

Answer:

```
Component
    |
    | dispatch(action)
    v
Redux Store
    |
    v
Reducer
    |
    v
Updated State
    |
    v
Subscribed Components
```

The component reads the updated state through selectors.

Q34. What is createSlice?

Answer: `createSlice` generates action creators and reducers from a slice definition.

```
const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: state => {
      state.value += 1;
    }
  }
});
```

It uses Immer internally to produce immutable state updates.

Q35. What is createAsyncThunk?

Answer: It simplifies asynchronous Redux operations and automatically generates pending, fulfilled and rejected action types.

```
export const fetchUsers = createAsyncThunk(
  'users/fetchUsers',
  async () => {
    const response = await api.get('/users');
    return response.data;
  }
);
```

I use these lifecycle states to manage loading, success and error conditions.

Q36. What is the difference between Redux and Context API?

Answer:

- Context API is suitable for sharing relatively simple state across components.
- Redux Toolkit is more suitable for complex application-wide state, predictable updates, middleware and debugging.

For applications such as Examly and Best Price India, centralized Redux state is useful for managing authentication, user data and business workflows.

Q37. What is middleware in Redux?

Answer: Middleware intercepts dispatched actions before they reach reducers.

It is useful for:

- API requests.
- Logging.
- Analytics.
- Authentication handling.
- Side effects.

Q38. What is useSelector?

Answer: `useSelector` reads data from the Redux store.

```
const user = useSelector(state => state.auth.user);
```

It subscribes the component to the selected state.

Q39. What is useDispatch?

Answer: `useDispatch` returns the Redux store's dispatch function.

```
const dispatch = useDispatch();

dispatch(fetchUsers());
```

Q40. How do you avoid unnecessary Redux re-renders?

Answer:

- Use specific selectors.
- Avoid selecting the entire store.
- Use memoized selectors.
- Normalize large collections.
- Keep state updates targeted.
- Use `React.memo` where appropriate.

Q41. What is Redux Persist?

Answer: Redux Persist is a library that persists selected Redux state to storage and restores it when the application starts.

I would persist only the necessary state and avoid storing sensitive tokens in ordinary unencrypted storage.

Q42. What is the difference between Redux Persist and AsyncStorage?

Answer: AsyncStorage is a key-value storage API. Redux Persist is a state persistence solution that can use AsyncStorage as its storage engine.

Q43. What is a selector in Redux?

Answer: A selector is a function that extracts specific data from the Redux store.

```
const selectUserName = state => state.auth.user?.name;
```

Memoized selectors can prevent unnecessary recalculations.

Q44. How do you structure Redux in a large React Native application?

Answer: I prefer feature-based organization.

```
src/
  features/
    auth/
      authSlice.ts
      authApi.ts
      authSelectors.ts
    products/
      productSlice.ts
      productApi.ts
  app/
    store.ts
  components/
  navigation/
  services/
  hooks/
  utils/
```

This makes the code easier to maintain and extend.

Q45. How do you handle API errors in Redux Toolkit?

Answer: I handle errors in the rejected state of `createAsyncThunk`, extract meaningful server messages and update the UI accordingly.

I also distinguish network failures, authentication errors, validation errors and server errors.

## Section 4: Navigation and application lifecycle (46–55)

Q46. What is React Navigation?

Answer: React Navigation is a navigation library for React Native that supports stack, tab and drawer navigation.

Q47. What are the different types of navigation?

Answer:

- Stack Navigation
- Bottom Tab Navigation
- Drawer Navigation
- Material Top Tabs
- Nested Navigation

Q48. What is the difference between navigate and push?

Answer:

- `navigate()` moves to a screen and may reuse an existing route depending on navigator behavior.
- `push()` adds a new route instance to a stack.

Q49. What is the difference between goBack and pop?

Answer: `goBack()` navigates back from the current route. `pop()` removes one or more screens from the stack.

Q50. How do you pass data between screens?

Answer:

```
navigation.navigate('Details', {
  productId: 101
});
```

On the destination screen:

```
const { productId } = route.params;
```

For complex shared state, I prefer Redux Toolkit rather than passing large objects through navigation parameters.

Q51. What are deep links?

Answer: Deep links allow users to open a particular screen inside an application using a URL.

For example:

```
myapp://products/101
```

They are useful for notifications, email links, marketing campaigns and sharing application content.

Q52. What is universal linking in iOS?

Answer: Universal Links allow standard HTTPS URLs to open the corresponding screen in an iOS application when it is installed.

They require Associated Domains configuration and an Apple App Site Association file.

Q53. What are Android App Links?

Answer: Android App Links associate verified HTTPS URLs with an Android application using Digital Asset Links and intent filters.

Q54. What is AppState in React Native?

Answer: `AppState` provides the current application state, such as active, background or inactive.

```
useEffect(() => {
  const subscription = AppState.addEventListener(
    'change',
    state => {
      console.log(state);
    }
  );

  return () => subscription.remove();
}, []);
```

It is useful for refreshing data, managing timers and handling application lifecycle transitions.

Q55. How do you handle an application going into the background during an exam?

Answer: In an exam application such as Examly, I would:

1. Monitor AppState changes.
2. Maintain the exam timer using timestamps rather than relying only on JavaScript intervals.
3. Persist answers and progress locally.
4. Restore the exam state when the user returns.
5. Apply the exam's configured background or resume policy.

I would also handle process termination and device restart separately.

## Section 5: API integration, authentication and security (56–70)

Q56. How do you integrate REST APIs in React Native?

Answer: I generally use Axios or Fetch with a centralized API service.

```
const getProducts = async () => {
  try {
    const response = await api.get('/products');
    return response.data;
  } catch (error) {
    throw error;
  }
};
```

I keep API logic separate from UI components and manage loading, success and error states through Redux or local state.

Q57. What is the difference between Axios and Fetch?

Answer:

| Axios                                                     | Fetch                             |
| --------------------------------------------------------- | --------------------------------- |
| External library                                          | Built into the runtime            |
| Automatic JSON transformation in common response handling | Requires explicit JSON parsing    |
| Interceptors supported                                    | Requires custom wrappers          |
| Request cancellation supported                            | AbortController supported         |
| Convenient timeout configuration                          | Requires additional configuration |

Q58. What is an Axios interceptor?

Answer: An interceptor allows us to process requests or responses globally.

For example, I use request interceptors to attach authentication tokens and response interceptors to handle common HTTP errors.

Q59. How do you implement JWT authentication?

Answer:

1. User submits login credentials.
2. Server validates credentials.
3. Server returns access and refresh tokens.
4. Access token is attached to protected API requests.
5. Expired access tokens are refreshed using the refresh token.
6. Logout clears stored credentials and application authentication state.

Q60. What is the difference between access token and refresh token?

Answer:

- Access token: Used to access protected APIs and generally has a short expiry.
- Refresh token: Used to obtain a new access token and generally has a longer lifetime.

Refresh tokens require stronger protection and should be securely stored.

Q61. How do you securely store authentication tokens?

Answer: I prefer Keychain on iOS and Android Keystore-backed secure storage through an appropriate library.

AsyncStorage is not appropriate for storing highly sensitive authentication credentials because it is not encrypted by default.

Q62. What is OAuth 2.0?

Answer: OAuth 2.0 is an authorization framework that allows applications to obtain limited access to resources.

For mobile applications, I prefer Authorization Code Flow with PKCE when implementing supported OAuth authorization.

Q63. What is token refresh handling?

Answer: When an API returns an expired-token response, the application requests a new access token using the refresh token.

I also use a request queue or shared refresh promise so multiple simultaneous API failures do not trigger multiple refresh requests.

Q64. How do you prevent multiple refresh-token API calls?

Answer: I maintain a shared refresh operation.

If one refresh request is already running, other failed requests wait for its result rather than starting additional refresh requests.

After successful refresh, pending requests are retried with the new access token.

Q65. How do you handle a 401 Unauthorized response?

Answer:

1. Check whether the access token has expired.
2. Attempt a refresh if appropriate.
3. Retry the original request once.
4. If refresh fails, clear authentication state.
5. Redirect the user to login.

I also prevent infinite retry loops.

Q66. How do you handle network failures?

Answer: I use:

- Network connectivity detection.
- Request timeouts.
- Retry with exponential backoff for eligible requests.
- Local caching.
- Clear offline indicators.
- Safe handling of pending operations.

I avoid retrying non-idempotent operations blindly.

Q67. What is GraphQL?

Answer: GraphQL is an API query language that allows clients to request exactly the data they need.

It supports queries, mutations and subscriptions.

Q68. What is the difference between REST and GraphQL?

Answer:

| REST                                       | GraphQL                                    |
| ------------------------------------------ | ------------------------------------------ |
| Multiple resource endpoints                | Usually one GraphQL endpoint               |
| Server-defined response structure          | Client-defined query structure             |
| May over-fetch or under-fetch              | Helps request specific fields              |
| HTTP methods commonly represent operations | Queries and mutations represent operations |

I have worked with REST and GraphQL integrations in projects such as Best Price India.

Q69. What is Apollo Client?

Answer: Apollo Client is a GraphQL client that manages queries, mutations, caching and local application data.

It helps manage GraphQL server communication and can reduce repeated network requests through caching.

Q70. How do you handle file uploads in React Native?

Answer: I use `FormData` for multipart uploads.

```
const formData = new FormData();

formData.append('file', {
  uri: imageUri,
  name: 'photo.jpg',
  type: 'image/jpeg'
});

await api.post('/upload', formData, {
  headers: {
    'Content-Type': 'multipart/form-data'
  }
});
```

I also handle upload progress, file validation, permissions and network failures.

## Section 6: Performance optimization (71–85)

Q71. How do you optimize React Native application performance?

Answer: My approach includes:

- Optimizing FlatList rendering.
- Avoiding unnecessary re-renders.
- Using React.memo, useMemo and useCallback appropriately.
- Optimizing images and caching.
- Reducing unnecessary API calls.
- Using Hermes.
- Profiling JavaScript and native performance.
- Removing unused dependencies.
- Optimizing application startup.

In my previous projects, I worked on reducing application load latency by 40% and improving scrolling performance on budget Android devices.

Q72. What is FlatList, and why is it preferred over ScrollView?

Answer: FlatList is a performant list component that lazily renders items as needed.

ScrollView renders all its children at once, whereas FlatList virtualizes list items to reduce memory consumption.

For large lists, I prefer FlatList.

Q73. How do you optimize FlatList?

Answer:

- Use stable and unique keys.
- Use `getItemLayout` when item dimensions are predictable.
- Use `initialNumToRender`.
- Configure `windowSize`.
- Use `maxToRenderPerBatch`.
- Avoid unnecessary component re-renders.
- Memoize list item components.
- Optimize images.

Example:

```
<FlatList
  data={products}
  keyExtractor={item => item.id.toString()}
  initialNumToRender={10}
  maxToRenderPerBatch={10}
  windowSize={5}
  renderItem={({ item }) => (
    <ProductItem item={item} />
  )}
/>
```

Q74. What is React.memo?

Answer: React.memo is a higher-order component that prevents unnecessary re-renders when a component's props have not changed according to its comparison function.

```
const UserCard = React.memo(({ user }) => {
  return <Text>{user.name}</Text>;
});
```

Q75. What is the difference between useMemo and useCallback?

Answer:

- `useMemo` memoizes a calculated value.
- `useCallback` memoizes a function reference.

```
const total = useMemo(
  () => calculateTotal(items),
  [items]
);

const handlePress = useCallback(() => {
  submitForm();
}, []);
```

Q76. What causes unnecessary re-renders?

Answer:

- Updating state unnecessarily.
- Passing newly created objects or functions as props.
- Changing context values frequently.
- Selecting large portions of Redux state.
- Incorrect component structure.
- Updating parent state that affects many children.

Q77. How do you identify performance bottlenecks?

Answer: I use React DevTools, React Native DevTools, Android Studio Profiler, Xcode Instruments and platform-specific logs.

I inspect:

- Slow rendering.
- JavaScript thread activity.
- UI frame drops.
- Memory consumption.
- Network latency.
- Startup time.
- Expensive calculations.

Q78. What is the difference between JS thread performance and UI thread performance?

Answer: The JavaScript thread executes application logic, while the native UI thread handles native UI operations.

Heavy JavaScript execution can delay event handling and JS-driven animations. Heavy native layout or rendering work can cause UI frame drops.

I profile both separately before deciding on an optimization.

Q79. What is a memory leak in React Native?

Answer: A memory leak happens when memory is retained unnecessarily and cannot be released when it is no longer needed.

Common causes include:

- Unremoved event listeners.
- Uncleaned timers.
- Unmanaged subscriptions.
- Retained references.
- Incorrect image or resource handling.

Q80. How do you prevent memory leaks?

Answer:

```
useEffect(() => {
  const timer = setInterval(() => {
    console.log('Running');
  }, 1000);

  return () => {
    clearInterval(timer);
  };
}, []);
```

I also remove listeners, unsubscribe from streams and avoid updating state from obsolete asynchronous operations.

Q81. How did you resolve memory leaks in CamUK?

Answer: In CamUK, I worked on memory issues in image-heavy newsfeed cards.

I addressed them through image caching, preloading and better image resource handling. I also reviewed component lifecycle and rendering behavior to avoid retaining unnecessary resources.

Q82. How do you optimize images in React Native?

Answer:

- Use appropriately sized images.
- Compress images.
- Use modern image formats when supported.
- Implement caching.
- Avoid loading full-resolution images in small containers.
- Use lazy loading.
- Use placeholders.
- Release unnecessary references.

Q83. What is lazy loading?

Answer: Lazy loading means loading resources only when they are needed.

For example, in a newsfeed, I load images as items approach the visible screen rather than downloading every image immediately.

Q84. How did you achieve smooth scrolling on low-end Android devices?

Answer: In Best Price India, I worked on list virtualization and lazy rendering.

I focused on reducing unnecessary component rendering, optimizing list item components, improving image loading and controlling how many items were rendered at a time.

The objective was smooth scrolling, targeting approximately 60 FPS.

Q85. How did you reduce application build size?

Answer: In Best Price India, I worked on reducing Android and iOS production build sizes by approximately 30%.

My approach included:

- Removing unused libraries.
- Reviewing native dependencies.
- Optimizing assets.
- Applying code-splitting where applicable.
- Reviewing release build configuration.

## Section 7: Offline-first applications, storage and real-time communication (86–98)

Q86. What is offline-first architecture?

Answer: Offline-first architecture allows an application to continue working even when the network is unavailable.

The application stores important data locally, tracks pending operations and synchronizes changes with the server when connectivity returns.

Q87. How did you implement offline functionality in Examly?

Answer: I worked on an offline-first exam module.

The application buffers student answers and progress locally, maintains exam timers and multi-state answer tracking, and securely submits pending answers when network connectivity returns.

This helps students continue their exams even during temporary network interruptions.

Q88. How do you manage offline data synchronization?

Answer:

1. Store user actions locally.
2. Maintain a pending synchronization queue.
3. Monitor connectivity.
4. Send pending changes when the network returns.
5. Handle server responses.
6. Mark successfully synchronized operations.
7. Retry failed operations safely.

Q89. What is AsyncStorage?

Answer: AsyncStorage is an asynchronous, persistent, unencrypted key-value storage system for React Native.

It is useful for storing non-sensitive information such as preferences, small cached values and application settings.

Q90. What is the difference between AsyncStorage and SQLite?

Answer:

| AsyncStorage                         | SQLite                           |
| ------------------------------------ | -------------------------------- |
| Key-value storage                    | Relational database              |
| Suitable for small persistent values | Suitable for structured datasets |
| Simple API                           | SQL queries and transactions     |
| Limited querying capabilities        | Supports complex querying        |

Q91. How do you handle conflicts during offline synchronization?

Answer: I consider the type of data and business requirements.

Possible strategies include:

- Last-write-wins.
- Server-authoritative updates.
- Version-based conflict detection.
- Field-level merging.
- Manual conflict resolution.

For exam answers, I would prioritize preserving submitted answer history and using server-side validation rather than blindly overwriting data.

Q92. How do you maintain an exam timer when an application goes into the background?

Answer: I store the start time and calculate elapsed time using timestamps.

I do not depend exclusively on `setInterval`, because JavaScript timers can be delayed or suspended in the background.

I also synchronize with the server where exam rules require authoritative timing.

Q93. What are WebSockets?

Answer: WebSockets provide persistent, bidirectional communication between client and server.

Unlike traditional request-response APIs, they allow the server to push real-time updates to the application.

Q94. What is Socket.IO?

Answer: Socket.IO is a real-time communication library that provides event-based communication, reconnection handling, acknowledgements and additional transport capabilities.

It is useful for chat, live notifications, bidding and real-time updates.

Q95. How did you implement real-time notifications in Best Price India?

Answer: I worked on supplier bidding notifications using WebSockets.

The application receives real-time bidding updates and displays relevant notifications. I also worked on reconnection handling to improve reliability when connectivity is interrupted.

Q96. How do you handle WebSocket reconnection?

Answer:

- Detect disconnection.
- Retry using exponential backoff.
- Stop retries when the application is intentionally disconnected.
- Reconnect when connectivity returns.
- Re-authenticate when necessary.
- Resubscribe to required channels.
- Fetch missed updates after reconnecting.

Q97. What is the difference between polling and WebSockets?

Answer:

- Polling repeatedly requests updates from the server.
- WebSockets maintain a persistent connection and allow real-time server-to-client communication.

WebSockets are generally preferable for frequent real-time updates, while polling can be simpler for infrequent updates.

Q98. What is Firebase Cloud Messaging (FCM)?

Answer: Firebase Cloud Messaging is a cross-platform messaging service used to send notifications and data messages to Android, iOS and web clients.

I have worked with FCM-based match and score alerts in Sport Ranking FTV.

## Section 8: Firebase, payments and native integrations (99–108)

Q99. How do push notifications work in React Native?

Answer:

1. Request notification permissions.
2. Register the device with the notification service.
3. Obtain the push token.
4. Send the token to the backend.
5. Backend sends notifications through FCM or APNs.
6. Application handles foreground, background and notification-tap events.

Q100. What is the difference between foreground and background notifications?

Answer:

- Foreground: The application is currently active.
- Background: The application is running in the background.
- Quit state: The application process is not running.

Notification handling differs across these states and depends on platform configuration and payload type.

Q101. What is APNs?

Answer: Apple Push Notification service is Apple's infrastructure for delivering remote notifications to iOS devices.

For iOS push notifications, the application requires appropriate capabilities, permissions and APNs configuration.

Q102. How do you handle notification permissions?

Answer: I request permissions at an appropriate point in the user journey, explain their purpose and handle permission states.

I also account for users denying permissions and provide a route to system settings when appropriate.

Q103. How do you integrate payment gateways in React Native?

Answer: My approach is:

1. Integrate the payment SDK or supported checkout flow.
2. Request payment details or an order from the backend.
3. Launch the payment interface.
4. Handle success, failure and cancellation callbacks.
5. Verify the transaction with the backend.
6. Update the application order status.

I have worked with Razorpay and in-app purchases in my projects.

Q104. What is Razorpay integration?

Answer: Razorpay provides payment gateway services.

In React Native, the application can initiate a payment through its supported SDK or checkout integration. The backend should verify payment signatures and confirm the final transaction status.

Q105. What are in-app purchases?

Answer: In-app purchases allow users to buy digital products, subscriptions or premium features within mobile applications.

On iOS, StoreKit handles Apple's in-app purchase system. On Android, Google Play Billing handles Google Play purchases.

Q106. What is the difference between Razorpay and Apple In-App Purchases?

Answer: Razorpay is a payment gateway that supports eligible payment use cases, while Apple In-App Purchases are Apple's billing mechanism for digital goods and services distributed through the App Store.

The appropriate payment method depends on the product, transaction type and applicable store policies.

Q107. How do you handle payment success if the application closes before receiving the callback?

Answer: I never rely only on the client callback.

I would:

- Verify the transaction on the backend.
- Use server-side payment confirmation or webhooks.
- Store the order reference.
- Fetch the latest order status when the application restarts.
- Update the UI based on verified transaction status.

Q108. How do you integrate native device features such as calendars?

Answer: I use an appropriate React Native library or native module to access the platform calendar.

I handle runtime permissions, platform-specific differences, error conditions and user cancellation.

In CamUK, I worked on event booking and notification functionality integrated with native calendars.

## Section 9: Testing, debugging and production releases (109–120)

Q109. What is Jest?

Answer: Jest is a JavaScript testing framework used for unit testing, mocking, assertions and code coverage.

It helps verify application logic independently from the UI.

Q110. What is React Native Testing Library?

Answer: React Native Testing Library helps test React Native components from the user's perspective.

It allows developers to render components, interact with them and verify expected UI behavior.

Q111. What is the difference between unit testing and integration testing?

Answer:

- Unit testing checks individual functions or components.
- Integration testing checks whether multiple parts work correctly together.

For example, testing a reducer is unit testing, while testing a login form connected to Redux and mocked API services is an integration-style test.

Q112. How do you test API calls in React Native?

Answer: I mock the API layer and test:

- Successful responses.
- Failed requests.
- Loading states.
- Empty responses.
- Authentication errors.
- Network failures.

This helps ensure predictable behavior without depending on a live backend.

Q113. How do you debug Android-specific issues?

Answer: I use:

- Android Studio.
- Logcat.
- ADB.
- React Native DevTools.
- Android Profiler.
- Gradle build logs.

I also reproduce issues on different Android versions and device configurations.

Q114. How do you debug iOS-specific issues?

Answer: I use:

- Xcode.
- Xcode console.
- Xcode Instruments.
- Simulator.
- Device logs.
- CocoaPods diagnostics.

I also verify provisioning profiles, signing configuration and native dependencies.

Q115. What is CI/CD in React Native?

Answer: CI/CD automates the process of building, testing and distributing mobile applications.

A typical pipeline includes:

```
Code Push
    |
    v
Dependency Installation
    |
    v
Linting and Testing
    |
    v
Android/iOS Build
    |
    v
Artifact Generation
    |
    v
Distribution / Store Release
```

Q116. What tools have you used for mobile build and release management?

Answer: My resume includes experience with:

- Android Studio.
- Xcode.
- Gradle.
- CocoaPods.
- GitHub Actions.
- GitLab CI/CD.
- Git and GitHub.

I use these tools to manage builds, configurations, testing and application releases.

Q117. Explain the Android release process.

Answer:

1. Configure application ID and version.
2. Generate a signing key.
3. Configure release signing.
4. Generate an Android App Bundle (AAB).
5. Test the release build.
6. Upload to Google Play Console.
7. Complete release checks and rollout.

Q118. Explain the iOS release process.

Answer:

1. Configure the bundle identifier.
2. Configure certificates and provisioning profiles.
3. Update version and build number.
4. Archive the application using Xcode.
5. Validate the archive.
6. Upload to App Store Connect.
7. Complete testing and App Store submission.

Q119. What is the difference between Debug and Release builds?

Answer:

| Debug                     | Release                  |
| ------------------------- | ------------------------ |
| Development and debugging | Production distribution  |
| Debugging support enabled | Optimized for production |
| Development configuration | Production configuration |
| Usually slower            | Generally optimized      |

A release build must be tested separately because some issues appear only in production configurations.

Q120. What is the difference between CodePush/OTA and a store release?

Answer: OTA updates deliver compatible JavaScript and assets to an existing binary.

A store release is necessary for native code changes, native dependency changes, permissions or other binary-level modifications.

OTA updates must comply with Google Play and Apple App Store policies.

## Section 10: Resume-based project and scenario questions (121–135)

These are particularly important because interviewers often select questions directly from your project descriptions.

Q121. Explain the architecture of your Examly application.

Answer: Examly is an educational application designed for NEET and JEE aspirants. I worked on its React Native frontend using TypeScript, Redux and Firebase.

The application includes:

- Exam preparation and mock tests.
- Offline-first exam functionality.
- Local answer persistence.
- Exam timers and answer tracking.
- Math and science equation rendering.
- Razorpay and in-app purchase integration.
- Student performance analytics.

I focused on reliability during long examination sessions, performance and data synchronization.

Q122. What was the biggest technical challenge in Examly?

Answer: One of the major challenges was maintaining exam progress when network connectivity was interrupted.

I addressed this by implementing local answer buffering, maintaining answer states and synchronizing pending answers when connectivity returned.

Another challenge was rendering mathematical and scientific notation smoothly without excessive memory consumption or layout issues.

Q123. How would you prevent students from losing answers during an examination?

Answer: I would use a combination of local persistence and server synchronization.

- Save answers locally immediately.
- Maintain a pending synchronization queue.
- Track question status.
- Retry failed synchronization requests.
- Use timestamps and versioning.
- Verify final submission with the server.

I would also ensure that duplicate requests do not create duplicate submissions.

Q124. How would you handle a 3-hour examination without performance degradation?

Answer: I would focus on:

1. Efficient question rendering.
2. Avoiding unnecessary re-renders.
3. Optimizing mathematical notation rendering.
4. Managing memory usage.
5. Persisting answers periodically.
6. Using timestamp-based timers.
7. Handling application background and foreground transitions.
8. Testing on low-memory devices.

Q125. Explain your work on Best Price India.

Answer: Best Price India is a B2B marketplace connecting buyers and suppliers.

I worked on React Native application development, Redux state management, GraphQL and REST API integrations, Razorpay payments and real-time supplier bidding notifications.

One important focus was improving performance on low- and mid-range Android devices.

Q126. How did you improve scrolling performance in Best Price India?

Answer: I worked on list virtualization and lazy rendering.

I optimized list item rendering, reduced unnecessary component updates and improved image loading.

The objective was to achieve smooth scrolling at approximately 60 FPS, including on budget Android devices.

Q127. How did you implement real-time supplier bidding?

Answer: I used WebSocket-based communication to receive real-time bidding updates.

The application listens for bidding events, updates the relevant UI and handles connection interruptions through reconnection logic.

I would also synchronize the latest bidding state with the server after reconnecting to avoid missing updates.

Q128. Explain your work on CamUK.

Answer: CamUK is a community ecosystem application for Cameroonians living, studying and working in the UK.

It includes social feeds, newsletters, job boards, business listings, legal resources and event booking.

I worked on end-to-end frontend development using React Native, TypeScript, Redux and Clean Architecture principles.

I also worked on real-time event notifications, native calendar integration, image optimization and memory leak resolution.

Q129. How did you implement event booking and notifications in CamUK?

Answer: I worked on the event booking and notification panel, integrating it with native calendars.

The flow involves retrieving event information, displaying booking options, handling booking status and managing notification interactions.

I would also handle calendar permissions and prevent duplicate event creation.

Q130. How would you troubleshoot a memory leak in a social media newsfeed?

Answer:

1. Reproduce the issue by scrolling through the feed repeatedly.
2. Monitor memory using Android Profiler or Xcode Instruments.
3. Check image caching and image dimensions.
4. Inspect event listeners and subscriptions.
5. Check component cleanup.
6. Identify retained objects.
7. Implement fixes and repeat the profiling process.

In CamUK, I worked on image caching and preloading to address memory issues in image-heavy newsfeed cards.

Q131. Explain your work on Sport Ranking FTV.

Answer: Sport Ranking FTV is a sports tournament management platform for foot volleyball in Brazil.

It consists of a React Native application for athletes and a React.js dashboard for arena managers.

I worked on synchronizing referee score updates with the public leaderboard and implementing tournament brackets and Firebase Cloud Messaging alerts.

Q132. How would you synchronize scores between a mobile application and a web dashboard?

Answer: I would use a centralized server as the source of truth.

The referee submits a score update, the server validates it and stores the latest result. The updated score is then distributed to connected clients through real-time communication.

I would also use match IDs, event IDs and sequence numbers to prevent duplicate or out-of-order updates.

Q133. How would you design a reusable component architecture for a large React Native application?

Answer: I would separate components into three categories:

- Common UI components: Buttons, inputs, cards and loaders.
- Feature components: Product cards, exam questions and booking panels.
- Screen components: Complete application screens.

I would also maintain centralized design tokens, reusable hooks, API services and feature-based state management.

This improves maintainability, consistency and code reuse.

Q134. How do you approach a production crash reported by a user?

Answer: My approach is:

1. Collect crash logs and device information.
2. Identify the affected application version.
3. Reproduce the issue.
4. Find the root cause.
5. Implement and test the fix.
6. Validate on relevant devices.
7. Release a patch or compatible OTA update.
8. Monitor the production issue.

I prioritize issues based on user impact and severity.

Q135. Why should we hire you as a Senior React Native Developer?

Answer: I have over 4.5 years of experience developing cross-platform mobile applications using React Native, JavaScript and TypeScript.

I have worked on production applications involving offline functionality, payment integrations, real-time communication, push notifications, application performance optimization and store releases.

I also have practical experience with Redux Toolkit, REST and GraphQL integrations, Clean Architecture, CI/CD and Android/iOS debugging.

I can take ownership of features from implementation through testing and release, collaborate with backend and QA teams, and support junior developers.

## Most important questions to prepare first

For your experience level, I recommend prioritizing these 25 questions.

| Priority | Topic                                     |
| -------- | ----------------------------------------- |
| 1        | React Native architecture                 |
| 2        | New Architecture                          |
| 3        | JSI vs Bridge                             |
| 4        | Fabric and TurboModules                   |
| 5        | Hermes                                    |
| 6        | Redux Toolkit architecture                |
| 7        | Redux vs Context API                      |
| 8        | useEffect and useLayoutEffect             |
| 9        | React.memo, useMemo and useCallback       |
| 10       | FlatList optimization                     |
| 11       | Memory leak debugging                     |
| 12       | JS thread vs UI thread                    |
| 13       | Offline-first architecture                |
| 14       | API integration and interceptors          |
| 15       | JWT authentication and token refresh      |
| 16       | Secure token storage                      |
| 17       | REST vs GraphQL                           |
| 18       | WebSockets and reconnection               |
| 19       | Firebase and push notifications           |
| 20       | Razorpay and in-app purchases             |
| 21       | Android and iOS release processes         |
| 22       | CI/CD                                     |
| 23       | Examly architecture                       |
| 24       | Best Price India performance optimization |
| 25       | CamUK memory leak resolution              |

## Practical coding questions to prepare

These are also commonly asked during technical interviews:

1. Implement debounce and throttle.
2. Create a custom useDebounce hook.
3. Implement pagination using FlatList.
4. Build a reusable TextInput component.
5. Implement API calls using Axios.
6. Create an Axios request interceptor.
7. Implement refresh-token handling.
8. Implement infinite scrolling.
9. Implement pull-to-refresh.
10. Create a reusable modal.
11. Implement a countdown timer.
12. Implement offline data synchronization.
13. Create a custom network connectivity hook.
14. Implement image caching.
15. Build a searchable FlatList.
16. Implement Redux Toolkit authentication.
17. Implement protected navigation.
18. Implement push notification handling.
19. Create a WebSocket connection with reconnection.
20. Optimize a component that renders thousands of records.