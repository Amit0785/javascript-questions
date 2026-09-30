# React Native Developer / Architect — Interview Questions & Answers

**Target Role:** React Native Developer / Architect  
**Experience in JD:** 5–12 years  
**Resume Basis:** Amit Kumar Singh — 4.5+ years React Native experience

This interview guide is tailored to the JD and resume, with easy-to-understand answers.

---

## 1. JavaScript / ES6+ — Questions 1–20

### 1. What is the difference between `var`, `let`, and `const`?

**Answer:**

- `var` is function-scoped.
- `let` is block-scoped.
- `const` is block-scoped and cannot be reassigned.

```js
var a = 10;
let b = 20;
const c = 30;
```

In modern JavaScript, I prefer `const` by default and `let` when reassignment is required.

### 2. What is hoisting?

**Answer:**

Hoisting means JavaScript processes declarations before executing the code.

```js
console.log(x);
var x = 10;
```

Output:

```text
undefined
```

The variable declaration is hoisted, but its assignment happens later.

Function declarations are also hoisted.

### 3. What is a closure?

**Answer:**

A closure occurs when an inner function remembers variables from its outer function even after the outer function has finished.

```js
function counter() {
  let count = 0;

  return function () {
    count++;
    return count;
  };
}

const increment = counter();

increment(); // 1
increment(); // 2
```

The inner function remembers `count`.

### 4. What is the difference between `==` and `===`?

**Answer:**

`==` performs type conversion.

```js
5 == "5" // true
```

`===` checks both type and value.

```js
5 === "5" // false
```

I generally prefer `===` because it avoids unexpected type conversion.

### 5. What is a pure function?

**Answer:**

A pure function:

1. Gives the same output for the same input.
2. Does not modify external state.

```js
function add(a, b) {
  return a + b;
}
```

Pure functions are easier to test and maintain.

### 6. What is the difference between `map()` and `forEach()`?

**Answer:**

`map()` returns a new array.

```js
const result = numbers.map(n => n * 2);
```

`forEach()` is generally used when we simply want to perform an operation.

```js
numbers.forEach(n => console.log(n));
```

### 7. What is `reduce()`?

**Answer:**

`reduce()` converts an array into a single result.

```js
const total = [10, 20, 30].reduce(
  (sum, value) => sum + value,
  0
);
```

Result:

```text
60
```

It can be used for totals, grouping, counting, and other aggregation operations.

### 8. What is destructuring?

**Answer:**

Destructuring allows us to extract values from objects or arrays easily.

```js
const user = {
  name: "Amit",
  age: 30
};

const { name, age } = user;
```

### 9. What is the spread operator?

**Answer:**

The spread operator `...` expands values.

```js
const first = [1, 2];
const second = [...first, 3, 4];
```

Result:

```text
[1, 2, 3, 4]
```

It is commonly used in React and Redux for immutable updates.

### 10. What is the rest operator?

**Answer:**

Rest collects multiple values into an array.

```js
function sum(...numbers) {
  return numbers.reduce((a, b) => a + b, 0);
}
```

### 11. What is the event loop?

**Answer:**

JavaScript is single-threaded, but it can handle asynchronous operations through the event loop.

Basic flow:

```text
Call Stack
   ↓
Web/Native APIs
   ↓
Callback/Task Queue
   ↓
Event Loop
   ↓
Call Stack
```

This allows JavaScript to handle timers, network requests, promises, etc.

### 12. What is a Promise?

**Answer:**

A Promise represents the result of an asynchronous operation.

It can be:

- Pending
- Fulfilled
- Rejected

```js
fetchData()
  .then(data => console.log(data))
  .catch(error => console.log(error));
```

### 13. What is async/await?

**Answer:**

`async/await` is a cleaner way of working with Promises.

```js
async function getUser() {
  try {
    const response = await fetch(url);
    const data = await response.json();
    return data;
  } catch (error) {
    console.log(error);
  }
}
```

It makes asynchronous code easier to read.

### 14. What is debounce?

**Answer:**

Debouncing delays execution until the user stops triggering an event for a specific period.

A common example is a search box.

```text
User types:
R
Re
Rea
React

API request → only after user stops typing
```

It reduces unnecessary API calls.

### 15. What is throttling?

**Answer:**

Throttling limits how frequently a function can execute.

For example, during scrolling:

```text
100 scroll events
↓
Function executes only once every 200ms
```

This is useful for scroll listeners and continuous events.

### 16. What is shallow copy vs deep copy?

**Answer:**

A shallow copy copies only the first level.

```js
const copy = {...user};
```

Nested objects are still referenced.

A deep copy creates independent nested objects as well.

### 17. What is immutability?

**Answer:**

Immutability means we don't directly modify existing data.

Instead of:

```js
user.name = "John";
```

we create a new object:

```js
const updatedUser = {
  ...user,
  name: "John"
};
```

This is very important in Redux and React.

### 18. What is optional chaining?

**Answer:**

Optional chaining prevents errors when accessing nested properties.

```js
user?.profile?.address?.city
```

If any intermediate value is `null` or `undefined`, it returns `undefined`.

### 19. What is nullish coalescing?

**Answer:**

The `??` operator provides a fallback only when the value is `null` or `undefined`.

```js
const name = user.name ?? "Guest";
```

### 20. What is the difference between synchronous and asynchronous code?

**Answer:**

Synchronous code executes sequentially.

Asynchronous code can start an operation and continue executing other code while waiting.

Examples:

```text
Synchronous → calculations
Asynchronous → API calls, timers, file operations
```

---

# 2. React — Questions 21–35

### 21. What is React?

**Answer:**

React is a JavaScript library for building user interfaces using reusable components.

React Native uses React concepts to build native mobile applications.

### 22. What is a component?

**Answer:**

A component is a reusable UI building block.

```jsx
const UserCard = ({name}) => {
  return <Text>{name}</Text>;
};
```

### 23. Functional component vs class component?

**Answer:**

Modern React primarily uses functional components.

Functional components use Hooks such as:

```text
useState()
useEffect()
useMemo()
useCallback()
```

They are simpler and are the preferred approach for modern React Native applications.

### 24. What is JSX?

**Answer:**

JSX allows us to write UI-like syntax inside JavaScript.

```jsx
<View>
  <Text>Hello</Text>
</View>
```

It is transformed into JavaScript internally.

### 25. What is `useState()`?

**Answer:**

`useState` manages local component state.

```js
const [count, setCount] = useState(0);
```

Calling `setCount` updates the state and causes the component to render again.

### 26. What is `useEffect()`?

**Answer:**

`useEffect` is used for side effects.

Examples:

- API calls
- subscriptions
- event listeners
- timers

```js
useEffect(() => {
  fetchUser();
}, []);
```

### 27. What does the dependency array do?

**Answer:**

```js
useEffect(() => {
  console.log("Run");
}, []);
```

An empty array means the effect runs after the initial render.

```js
useEffect(() => {
  console.log(userId);
}, [userId]);
```

This runs when `userId` changes.

### 28. What is `useMemo()`?

**Answer:**

`useMemo` caches a calculated value.

```js
const total = useMemo(() => {
  return calculateTotal(items);
}, [items]);
```

It can prevent expensive calculations from running unnecessarily.

### 29. What is `useCallback()`?

**Answer:**

`useCallback` memoizes a function.

```js
const handlePress = useCallback(() => {
  console.log("Pressed");
}, []);
```

It can help when passing callbacks to memoized child components.

### 30. `useMemo` vs `useCallback`?

**Answer:**

Simple rule:

```text
useMemo → memoizes a value
useCallback → memoizes a function
```

### 31. What is React.memo?

**Answer:**

`React.memo` prevents unnecessary rendering when props haven't changed.

```js
export default React.memo(UserCard);
```

It is useful for frequently rendered components.

### 32. What causes a React component to re-render?

**Answer:**

Common causes:

- State changes
- Props change
- Parent re-renders
- Context value changes

### 33. What is reconciliation?

**Answer:**

Reconciliation is React's process of comparing the previous UI tree with the new one and determining what needs to change.

### 34. What is the Virtual DOM?

**Answer:**

It is an in-memory representation of UI.

React compares changes and updates the actual UI efficiently.

In React Native, this eventually results in updates to native views.

### 35. How do you optimize React components?

**Answer:**

I commonly use:

- `React.memo`
- `useMemo`
- `useCallback`
- FlatList optimization
- Avoiding unnecessary state
- Splitting large components
- Stable keys
- Lazy loading

---

# 3. React Native — Questions 36–65

### 36. What is React Native?

**Answer:**

React Native is a framework that allows developers to build iOS and Android applications using JavaScript/TypeScript and React while rendering native platform UI.

### 37. React Native vs React?

**Answer:**

React is primarily used for web applications.

React Native is used for mobile applications.

For example:

```jsx
React:
<div>

React Native:
<View>
```

### 38. How does React Native communicate with native code?

**Answer:**

React Native uses native interfaces to communicate between JavaScript and native iOS/Android code.

Modern React Native uses the New Architecture with technologies such as:

- JSI
- TurboModules
- Fabric

### 39. What is Hermes?

**Answer:**

Hermes is a JavaScript engine optimized for React Native.

It can improve:

- Startup time
- Memory usage
- Application performance

### 40. What is React Native New Architecture?

**Answer:**

The New Architecture modernizes communication between JavaScript and native code.

Major components include:

```text
JSI
TurboModules
Fabric
Codegen
```

The goal is better performance and more direct communication between JS and native layers.

### 41. What is Fabric?

**Answer:**

Fabric is React Native's modern rendering system.

It improves communication between React and native UI and supports the New Architecture.

### 42. What are TurboModules?

**Answer:**

TurboModules are the modern architecture for native modules.

They allow JavaScript to communicate with native functionality more efficiently.

### 43. What are Native Modules?

**Answer:**

Native Modules allow React Native applications to use functionality implemented in:

- Swift/Objective-C on iOS
- Kotlin/Java on Android

Examples include device-specific APIs or custom native SDK integrations.

### 44. What is React Navigation?

**Answer:**

React Navigation provides navigation functionality in React Native applications.

Common navigators include:

- Stack
- Tab
- Drawer
- Native Stack

### 45. Stack navigation vs tab navigation?

**Answer:**

Stack navigation represents screens in a hierarchy:

```text
Home
 ↓
Product
 ↓
Details
```

Tab navigation is usually used for major application sections:

```text
Home | Search | Profile
```

### 46. How do you pass parameters between screens?

**Answer:**

```js
navigation.navigate("Details", {
  productId: 123
});
```

Then retrieve the parameter on the destination screen.

### 47. How do you handle deep linking?

**Answer:**

I configure React Navigation linking.

For example:

```text
myapp://product/123
```

The URL can navigate directly to the product screen.

Deep linking must also be configured at the Android and iOS levels.

### 48. What is FlatList?

**Answer:**

FlatList efficiently renders large lists by rendering only the items needed around the visible area.

```jsx
<FlatList
  data={users}
  renderItem={renderUser}
  keyExtractor={item => item.id}
/>
```

### 49. FlatList vs ScrollView?

**Answer:**

`ScrollView` renders all children.

`FlatList` virtualizes the list.

For large datasets:

```text
FlatList → preferred
ScrollView → small/static content
```

### 50. How do you optimize FlatList?

**Answer:**

I use:

- `keyExtractor`
- `getItemLayout` when possible
- `initialNumToRender`
- `windowSize`
- `maxToRenderPerBatch`
- `removeClippedSubviews` where appropriate
- memoized row components
- stable `renderItem`

### 51. How do you achieve 60 FPS?

**Answer:**

I look for:

- Expensive JS operations
- Excessive component renders
- Large images
- Heavy list rendering
- Unnecessary animations
- Memory leaks

Then I optimize rendering, list virtualization, image loading, animations, and expensive calculations.

### 52. How do you debug a React Native crash?

**Answer:**

My approach is:

1. Reproduce the crash.
2. Check logs.
3. Identify whether it is JS or native.
4. Check stack trace.
5. Check recent changes.
6. Test on affected device/OS.
7. Fix and regression-test.

### 53. How do you troubleshoot memory leaks?

**Answer:**

I check:

- Event listeners
- Timers
- WebSocket connections
- Subscriptions
- Large image objects
- Navigation listeners
- Components that don't clean up

Example:

```js
useEffect(() => {
  const subscription = subscribe();

  return () => {
    subscription.remove();
  };
}, []);
```

### 54. How do you optimize images?

**Answer:**

I use:

- Appropriate image sizes
- Caching
- Lazy loading
- Compression
- Avoiding unnecessarily large images
- Preloading only when useful

### 55. What is CodePush/OTA update?

**Answer:**

OTA means Over-The-Air update.

It can update JavaScript bundle changes without requiring a complete store release, subject to platform/store policies and what changed.

### 56. What should not be delivered through OTA?

**Answer:**

Generally, native binary changes such as:

- Native modules
- Native dependencies
- Android/iOS configuration changes

require a new application binary.

### 57. Expo vs React Native CLI?

**Answer:**

Expo provides a managed development ecosystem and many built-in services.

React Native CLI provides more direct control over native projects.

For applications requiring extensive native customization, native project access can be important.

### 58. What is Metro?

**Answer:**

Metro is the JavaScript bundler used by React Native.

It:

- Resolves modules
- Transforms JavaScript
- Builds the bundle
- Supports development features

### 59. What is the difference between Android and iOS development in React Native?

**Answer:**

The JavaScript/TypeScript layer can be shared, but platform-specific differences remain.

Examples:

```text
Android → Gradle, AndroidManifest
iOS → Xcode, Info.plist, CocoaPods
```

Native permissions and SDK behavior may also differ.

### 60. How do you handle platform-specific code?

**Answer:**

Using:

```js
Platform.OS
```

or:

```text
Component.android.js
Component.ios.js
```

Example:

```js
if (Platform.OS === "ios") {
  // iOS logic
}
```

### 61. What is `Platform.select()`?

**Answer:**

It allows platform-specific values.

```js
const padding = Platform.select({
  ios: 20,
  android: 16
});
```

### 62. How do you handle permissions?

**Answer:**

I handle permissions separately for Android and iOS because their permission systems differ.

Examples:

- Camera
- Microphone
- Location
- Notifications
- Storage

I also explain why permission is required and handle denied states gracefully.

### 63. How do you handle push notifications?

**Answer:**

Typical flow:

```text
App
 ↓
Firebase/APNs
 ↓
Device token
 ↓
Backend
 ↓
Notification service
 ↓
Device
```

The token is registered with the backend and notifications are sent through the appropriate provider.

### 64. What is a native dependency?

**Answer:**

A dependency that contains native Android/iOS code in addition to JavaScript.

Examples include:

- Payment SDKs
- Camera SDKs
- Maps SDKs
- Analytics SDKs

### 65. How do you manage third-party dependencies?

**Answer:**

I:

1. Check compatibility.
2. Check native requirements.
3. Lock versions.
4. Test Android/iOS.
5. Review release notes.
6. Remove unused packages.
7. Regularly update dependencies.

---

# 4. Redux / State Management — Questions 66–80

### 66. What is Redux?

**Answer:**

Redux is a predictable state management library.

Basic flow:

```text
Component
   ↓
Dispatch Action
   ↓
Reducer
   ↓
Store Updated
   ↓
Component Re-renders
```

### 67. Why use Redux?

**Answer:**

Redux is useful when multiple parts of an application need access to shared state.

Examples:

- Authentication
- User profile
- Cart
- Application settings
- Server data
- Global UI state

### 68. What is Redux Toolkit?

**Answer:**

Redux Toolkit is the recommended way to write Redux.

It reduces boilerplate and provides utilities such as:

```text
configureStore
createSlice
createAsyncThunk
createEntityAdapter
```

### 69. What is a Redux slice?

**Answer:**

A slice contains:

- Initial state
- Reducers
- Actions

Example:

```js
const userSlice = createSlice({
  name: "user",
  initialState,
  reducers: {
    logout: state => {
      state.user = null;
    }
  }
});
```

### 70. What is a reducer?

**Answer:**

A reducer describes how state changes in response to an action.

```text
Current State + Action
        ↓
     Reducer
        ↓
   New State
```

### 71. What is an action?

**Answer:**

An action describes what happened.

```js
{
  type: "user/logout"
}
```

With Redux Toolkit, actions are often automatically generated by slices.

### 72. What is middleware?

**Answer:**

Middleware sits between dispatching an action and the reducer.

It can be used for:

- Logging
- API calls
- Analytics
- Authentication handling

### 73. Redux vs Context API?

**Answer:**

Context is useful for simpler global values.

Redux is more suitable when:

- State is complex.
- Many components need it.
- Debugging is important.
- State transitions are frequent.
- Application architecture requires centralized state management.

### 74. Should every API response go into Redux?

**Answer:**

No.

I keep data globally only when multiple parts of the application need it.

Local component data should remain local.

For server-state-heavy applications, I would also consider a server-state solution rather than putting everything into Redux.

### 75. How do you structure Redux in a large application?

**Answer:**

I prefer feature-based organization:

```text
src/
  features/
    auth/
      authSlice.ts
      authApi.ts
    users/
      userSlice.ts
      userApi.ts
```

This makes the code easier to maintain.

### 76. How do you persist Redux state?

**Answer:**

Options include persistence middleware/libraries or explicitly storing selected state.

Sensitive information should not be blindly persisted.

### 77. Should JWT tokens be stored in Redux?

**Answer:**

Redux can hold the current authentication state, but sensitive tokens should preferably be stored in secure storage.

For example:

```text
Redux → authentication state
Secure Storage → sensitive token
```

### 78. What is a selector in Redux?

**Answer:**

A selector reads required data from the Redux store.

```js
const user = useSelector(state => state.auth.user);
```

Selectors can also derive computed data.

### 79. What is normalized state?

**Answer:**

Instead of deeply nested duplicate objects, related entities are stored separately.

Example:

```js
users: {
  byId: {},
  allIds: []
}
```

This makes updates easier.

### 80. How do you avoid unnecessary Redux re-renders?

**Answer:**

I use:

- Focused selectors
- Memoized selectors where appropriate
- Normalized state
- Smaller slices
- Avoiding unnecessary global state
- Stable references

---

# 5. GraphQL / REST APIs — Questions 81–95

### 81. What is REST?

**Answer:**

REST is an architectural style commonly used for APIs.

Typical operations:

```text
GET     → Read
POST    → Create
PUT     → Replace
PATCH   → Update
DELETE  → Delete
```

### 82. What is GraphQL?

**Answer:**

GraphQL allows clients to request exactly the data they need.

Example:

```graphql
query {
  user {
    id
    name
    email
  }
}
```

### 83. REST vs GraphQL?

**Answer:**

REST generally uses multiple endpoints.

GraphQL commonly uses a single endpoint and allows the client to specify the required fields.

```text
REST:
GET /users/123

GraphQL:
query {
  user(id: 123) {
    name
    email
  }
}
```

### 84. What are GraphQL queries and mutations?

**Answer:**

`Query` is generally used to retrieve data.

`Mutation` is generally used to modify data.

```text
Query → Read
Mutation → Create/Update/Delete
```

### 85. What is a GraphQL subscription?

**Answer:**

Subscriptions are used for real-time updates.

For example:

```text
New message
 ↓
GraphQL subscription
 ↓
Mobile app receives update
```

### 86. What is over-fetching?

**Answer:**

Over-fetching means receiving more data than the application needs.

GraphQL can help reduce this because the client specifies requested fields.

### 87. What is under-fetching?

**Answer:**

Under-fetching means one API request doesn't provide enough data, requiring additional requests.

### 88. How do you handle API errors?

**Answer:**

I categorize them:

```text
400 → Client request problem
401 → Authentication problem
403 → Permission problem
404 → Resource not found
500 → Server problem
```

Then I provide appropriate UI and logging.

### 89. How do you handle token expiration?

**Answer:**

A common approach:

```text
API request
 ↓
401
 ↓
Refresh token
 ↓
Get new access token
 ↓
Retry request
```

If refresh fails, the user is logged out.

### 90. What is an interceptor?

**Answer:**

An interceptor allows us to modify requests or responses globally.

For example:

```text
Request
 ↓
Add Authorization header
 ↓
API
```

Response interceptors can handle common errors such as 401.

### 91. How do you secure API calls?

**Answer:**

I use:

- HTTPS
- Authentication
- Authorization
- Secure token storage
- Input validation
- Proper session handling
- Avoiding sensitive data in logs

### 92. What is pagination?

**Answer:**

Pagination means retrieving data in smaller chunks.

Example:

```text
Page 1 → 20 items
Page 2 → 20 items
Page 3 → 20 items
```

This reduces memory and network usage.

### 93. Offset pagination vs cursor pagination?

**Answer:**

Offset:

```text
page=2&limit=20
```

Cursor:

```text
after=abc123
```

Cursor pagination is often more reliable for changing datasets.

### 94. How would you integrate GraphQL in React Native?

**Answer:**

Typical architecture:

```text
React Native
      ↓
GraphQL Client
      ↓
GraphQL API
      ↓
Backend
      ↓
Database
```

The client handles queries, mutations, caching, loading and error states.

### 95. How would you prevent duplicate API requests?

**Answer:**

I can use:

- Request deduplication
- Caching
- Debouncing
- Request state tracking
- Query libraries/cache policies
- Proper component lifecycle handling

---

# 6. Architecture / HLD / LLD — Questions 96–110

### 96. What architecture would you use for a large React Native application?

**Answer:**

I would use a modular architecture based on features and separation of responsibilities.

For example:

```text
Presentation
     ↓
Domain / Business Logic
     ↓
Data Layer
     ↓
API / Local Storage
```

### 97. What is Clean Architecture?

**Answer:**

Clean Architecture separates the application into layers so business logic isn't tightly coupled to UI or infrastructure.

Example:

```text
UI
 ↓
Use Cases
 ↓
Repository
 ↓
API / Database
```

### 98. What is MVVM?

**Answer:**

MVVM means:

```text
Model
View
ViewModel
```

The View displays UI.

The ViewModel handles presentation logic.

The Model represents application data.

### 99. Clean Architecture vs MVVM?

**Answer:**

They solve slightly different problems.

MVVM focuses on separating UI from presentation logic.

Clean Architecture focuses on separating business rules from frameworks and infrastructure.

They can be combined.

### 100. What is SOLID?

**Answer:**

SOLID consists of five principles:

```text
S → Single Responsibility
O → Open/Closed
L → Liskov Substitution
I → Interface Segregation
D → Dependency Inversion
```

They help make code easier to maintain and extend.

### 101. What is HLD?

**Answer:**

HLD means High-Level Design.

It describes the overall system architecture.

For a mobile app, it may contain:

```text
Mobile App
 ↓
API Layer
 ↓
Authentication
 ↓
Backend
 ↓
Database
```

### 102. What is LLD?

**Answer:**

LLD means Low-Level Design.

It explains implementation details such as:

- Classes
- Interfaces
- Data models
- Functions
- Component interactions
- API contracts

### 103. What would you include in an HLD document?

**Answer:**

I would include:

- Requirements
- Architecture diagram
- Technology choices
- API communication
- Authentication
- State management
- Data flow
- Security
- Deployment
- Scalability
- Error handling

### 104. What would you include in an LLD document?

**Answer:**

I would include:

- Component structure
- Interfaces
- Models
- Function responsibilities
- Navigation flow
- API request/response models
- State management
- Error handling

### 105. How would you design a scalable React Native application?

**Answer:**

I would focus on:

1. Feature-based modules.
2. Clear separation of concerns.
3. Reusable components.
4. Centralized API handling.
5. Predictable state management.
6. Secure authentication.
7. Testing.
8. CI/CD.
9. Performance monitoring.

### 106. How would you design offline-first functionality?

**Answer:**

Example:

```text
User action
 ↓
Save locally
 ↓
Update UI immediately
 ↓
Check network
 ↓
Sync with server
```

If offline, operations are queued.

When connectivity returns, the queue is synchronized.

### 107. How do you handle synchronization conflicts?

**Answer:**

I define a conflict strategy based on the business requirement.

Possible strategies:

- Last-write-wins
- Server-wins
- Client-wins
- Version-based conflict detection
- Manual conflict resolution

### 108. How would you design a real-time mobile application?

**Answer:**

I would use:

```text
React Native
     ↓
WebSocket / Socket.IO
     ↓
Backend
     ↓
Database
```

I would also handle:

- Reconnection
- Authentication
- Network changes
- Duplicate events
- Offline events

### 109. How would you design authentication?

**Answer:**

A common architecture is:

```text
Login
 ↓
Access Token + Refresh Token
 ↓
Secure Storage
 ↓
Authenticated API requests
 ↓
Token refresh
 ↓
Logout
```

The access token should have a limited lifetime.

### 110. How do you make an architecture maintainable?

**Answer:**

I focus on:

- Clear responsibilities
- Low coupling
- High cohesion
- Reusable components
- Consistent coding standards
- Documentation
- Automated testing
- Code reviews
- Dependency control

---

# 7. Mobile Security — Questions 111–120

### 111. Where should authentication tokens be stored?

**Answer:**

Sensitive tokens should be stored using platform-secure storage rather than plain AsyncStorage.

Examples include:

```text
iOS → Keychain
Android → Keystore-backed secure storage
```

### 112. Is AsyncStorage secure for JWT tokens?

**Answer:**

AsyncStorage should not be treated as secure storage for highly sensitive credentials.

For sensitive authentication tokens, I prefer secure storage.

### 113. What is JWT?

**Answer:**

JWT stands for JSON Web Token.

It commonly contains claims about the authenticated user and is digitally signed.

Typical flow:

```text
Login
 ↓
JWT
 ↓
Authorization header
 ↓
API
```

### 114. What is OAuth 2.0?

**Answer:**

OAuth 2.0 is an authorization framework that allows applications to obtain limited access to resources.

It is commonly used with:

- Google login
- Facebook login
- Enterprise authentication

### 115. How do you protect sensitive information?

**Answer:**

I avoid:

- Hardcoding secrets
- Logging tokens
- Storing credentials in plain text
- Sending sensitive data over HTTP

I use secure storage and HTTPS.

### 116. Should API keys be stored in a React Native application?

**Answer:**

A mobile application is ultimately distributed to users, so secrets embedded in the binary should not be considered truly secret.

Sensitive server credentials should remain on the backend.

### 117. How do you prevent accidental secret exposure?

**Answer:**

I use:

- Environment configuration
- Secret management
- `.gitignore`
- CI/CD secret variables
- Code review
- Secret scanning

### 118. What is certificate pinning?

**Answer:**

Certificate pinning allows an application to verify that the server certificate/public key matches an expected value.

It can provide additional protection against certain man-in-the-middle attacks.

It also adds operational complexity because certificates need proper rotation planning.

### 119. How do you secure deep links?

**Answer:**

I don't trust deep-link parameters blindly.

I validate:

- Parameters
- Authentication state
- Authorization
- Destination resources

Sensitive operations should require server-side authorization.

### 120. What security practices do you follow in React Native?

**Answer:**

My checklist includes:

```text
HTTPS
Secure token storage
JWT/OAuth handling
Input validation
No secrets in source code
Safe logging
Dependency updates
Secure deep linking
Authentication/authorization
Server-side validation
```

---

# 8. CI/CD, Xcode, Gradle & Release — Questions 121–130

### 121. What is Gradle?

**Answer:**

Gradle is the build system used by Android projects.

It manages:

- Dependencies
- Build variants
- Signing
- APK/AAB generation
- Build configurations

### 122. What is Xcode?

**Answer:**

Xcode is Apple's development environment.

For React Native, it is used for:

- iOS builds
- Signing
- Provisioning
- Certificates
- Debugging
- Archive generation
- App Store submission

### 123. What is Fastlane?

**Answer:**

Fastlane automates mobile development tasks.

For example:

```text
Build
 ↓
Test
 ↓
Sign
 ↓
Upload
 ↓
App Store / Play Store
```

It reduces manual release work.

### 124. What is CI/CD?

**Answer:**

CI/CD automates software delivery.

```text
Developer pushes code
       ↓
CI pipeline
       ↓
Build
       ↓
Tests
       ↓
Quality checks
       ↓
Release/deployment
```

### 125. What is an Android AAB?

**Answer:**

AAB stands for Android App Bundle.

It is the preferred format for distributing Android applications through Google Play.

### 126. What is an APK?

**Answer:**

APK is the Android application package.

It can be installed directly on Android devices.

### 127. What is iOS provisioning?

**Answer:**

Provisioning connects:

```text
App ID
+
Certificate
+
Device/distribution configuration
```

It allows the application to be built and distributed correctly.

### 128. What is a release build?

**Answer:**

A release build is optimized for production.

Compared with debug builds, it generally has:

- Optimization
- Minification where applicable
- Production configuration
- Release signing
- Debug tooling removed/reduced

### 129. How do you release an app to Play Store?

**Answer:**

Typical process:

```text
Update version
 ↓
Build signed AAB
 ↓
Test
 ↓
Upload to Play Console
 ↓
Internal testing
 ↓
Production release
```

### 130. How do you release an app to App Store?

**Answer:**

Typical process:

```text
Update version
 ↓
Configure signing
 ↓
Archive in Xcode
 ↓
Upload to App Store Connect
 ↓
TestFlight
 ↓
Submit for review
 ↓
Release
```

---

# 9. Project-Based Questions — Questions 131–140

### 131. Explain the architecture of Examly.

**Answer:**

Examly is a React Native application using TypeScript and Redux. I worked on an offline-first exam module where user progress could be stored locally and synchronized with the backend when connectivity returned. The application also integrated APIs, Firebase, Razorpay and in-app purchases. The architecture was designed to keep UI, state management and API/data responsibilities separated.

### 132. How did you implement offline functionality in Examly?

**Answer:**

When the user starts an exam, progress and answers are maintained locally. The UI can continue working without network connectivity. Once the network becomes available, pending data is synchronized with the server. This avoids losing user progress during network interruptions.

### 133. How did you handle a 3-hour exam?

**Answer:**

Important areas were:

- Efficient state management
- Local persistence
- Timer management
- Avoiding unnecessary renders
- Memory optimization
- Efficient question rendering

### 134. How did you improve Best Price India's performance?

**Answer:**

The application needed to work well on mid-to-low-range Android devices. I focused on list virtualization and lazy rendering to reduce the rendering workload and improve scrolling performance. I also removed unused libraries and used code-splitting to reduce build size.

### 135. How did you implement real-time bidding?

**Answer:**

I used WebSockets to receive supplier bidding updates in real time. The important parts were maintaining the connection, handling reconnection, updating the UI efficiently and avoiding duplicate events.

### 136. How did you handle WebSocket reconnection?

**Answer:**

A basic strategy is:

```text
Connection lost
 ↓
Detect disconnect
 ↓
Wait/retry
 ↓
Reconnect
 ↓
Authenticate
 ↓
Resume communication
```

I also need to handle network changes and prevent multiple simultaneous connections.

### 137. Explain CamUK architecture.

**Answer:**

CamUK used React Native with TypeScript and Redux. I followed modular and Clean Architecture principles so features could be expanded without tightly coupling components. The app also used Firebase, Socket.IO, Agora and backend APIs.

### 138. How did you fix image-related memory issues?

**Answer:**

I would check:

- Image dimensions
- Image caching
- Number of simultaneously rendered images
- Image preloading
- Component lifecycle
- List virtualization

The solution included image caching and pre-loading.

### 139. How did you integrate Razorpay?

**Answer:**

Typical flow:

```text
User selects product
 ↓
Backend creates order
 ↓
React Native opens payment SDK
 ↓
Payment completed
 ↓
Payment response
 ↓
Backend verifies transaction
 ↓
Order confirmed
```

The important security point is that payment verification should happen server-side.

### 140. How did you integrate in-app purchases?

**Answer:**

The mobile application communicates with Apple's/Google's billing system through the appropriate SDK/library.

The flow is:

```text
User selects subscription
 ↓
Store billing
 ↓
Purchase result
 ↓
Backend verification
 ↓
Subscription activated
```

---

# 10. Architect / Leadership Questions — Questions 141–150

### 141. How do you review another developer's code?

**Answer:**

I check:

1. Correctness
2. Architecture
3. Readability
4. Performance
5. Security
6. Error handling
7. Testing
8. Maintainability

I prefer constructive feedback rather than simply pointing out problems.

### 142. How do you mentor junior developers?

**Answer:**

I generally:

- Explain the reason behind decisions.
- Review their code.
- Give smaller ownership initially.
- Pair on difficult problems.
- Encourage documentation and testing.
- Gradually increase responsibility.

### 143. How do you handle disagreement with a developer?

**Answer:**

I focus on the technical requirement rather than personal preference.

I compare:

- Performance
- Maintainability
- Security
- Complexity
- Business requirements

Then we choose the solution supported by evidence.

### 144. How do you communicate technical problems to stakeholders?

**Answer:**

I avoid unnecessary technical terminology.

I explain:

```text
Problem
Impact
Root cause
Options
Recommended implementation
Timeline
Risk
```

### 145. How do you estimate a feature?

**Answer:**

I break it into:

```text
UI
API
State management
Native integration
Testing
Bug fixing
Release
```

Then estimate each piece and add contingency for unknowns.

### 146. How do you handle production issues?

**Answer:**

My approach is:

```text
Detect
 ↓
Reproduce
 ↓
Assess impact
 ↓
Find root cause
 ↓
Fix
 ↓
Test
 ↓
Release
 ↓
Monitor
```

### 147. How do you prioritize bugs?

**Answer:**

I consider:

- Number of affected users
- Business impact
- Security impact
- Crash severity
- Workaround availability
- Frequency

A production crash affecting many users gets high priority.

### 148. How do you handle technical debt?

**Answer:**

I identify technical debt and document its impact.

Then I prioritize it alongside feature work.

For critical debt, I explain the business risk to stakeholders.

### 149. How do you decide whether to introduce a library?

**Answer:**

I consider:

- Is it actually needed?
- Maintenance activity
- Community adoption
- Bundle size
- Security
- Native compatibility
- License
- Long-term support

### 150. What makes someone a good React Native architect?

**Answer:**

A good React Native architect should understand both application-level architecture and mobile-specific concerns. They should be able to design scalable React Native applications, make appropriate decisions around state management and APIs, understand Android/iOS build systems, handle security and performance, and communicate those decisions clearly to developers and stakeholders.

---

# Top 20 Questions to Prioritize

For this JD, prepare these especially well:

1. Explain React Native architecture.
2. Explain React Native New Architecture.
3. JSI vs Bridge.
4. Fabric and TurboModules.
5. Redux Toolkit architecture.
6. Redux vs Context.
7. GraphQL vs REST.
8. GraphQL integration architecture.
9. Authentication/token refresh.
10. Secure token storage.
11. Clean Architecture.
12. HLD vs LLD.
13. Design a scalable React Native application.
14. React Native performance optimization.
15. FlatList optimization.
16. Offline-first architecture.
17. WebSocket/reconnection architecture.
18. CI/CD pipeline.
19. Xcode + Gradle release process.
20. Explain your Examly, Best Price India and CamUK architecture.

---

# Important Resume/JD Gap Question

The JD asks for **5–12 years**, while the resume states **4.5+ years** of React Native experience.

If they ask about this, give a factual answer:

> "I currently have 4.5+ years of professional software development experience, with strong hands-on React Native experience across architecture, APIs, Redux, GraphQL, CI/CD, performance optimization and production releases. Although the JD mentions 5+ years, my experience closely aligns with the technical responsibilities of the role."

Do not inflate your experience. Focus on the technical responsibilities you have actually handled.

---

# Quick Revision Checklist

Before the interview, make sure you can explain each of these without memorizing a definition:

- JavaScript closures
- Hoisting
- Event loop
- Promises
- Async/await
- Debounce/throttle
- React Hooks
- React rendering
- React.memo
- FlatList
- React Navigation
- Deep linking
- Redux Toolkit
- Middleware
- Selectors
- GraphQL
- REST
- Authentication
- JWT
- OAuth 2.0
- Token refresh
- Secure storage
- Clean Architecture
- MVVM
- SOLID
- HLD
- LLD
- New Architecture
- JSI
- Fabric
- TurboModules
- Hermes
- Native Modules
- WebSockets
- Offline-first
- Performance optimization
- Memory leaks
- Gradle
- Xcode
- Fastlane
- CI/CD
- App Store release
- Play Store release
- Production troubleshooting
- Code review
- Mentoring
- Stakeholder communication
