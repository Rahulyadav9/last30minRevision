Perfect 👍 — here’s a **“React.js 30-minute interview Q&A”** set curated **for 7+ years of experience** — covering **deep concepts, performance, design patterns, architecture, and real-world problem-solving**.

---

## ⚡ React.js Interview (Senior-Level — 30 mins, 7+ years exp)

### 🔹 1. What happens during the React render phase?

React creates a virtual DOM tree, compares it (diffing) with the previous one (reconciliation), and schedules minimal updates to the real DOM via the Fiber architecture.

---

### 🔹 2. What is React Fiber and why was it introduced?

React Fiber is a complete rewrite of the reconciliation algorithm.
It allows **asynchronous rendering**, **interruptible work**, and **better scheduling** of rendering tasks for improved UI responsiveness.

---

### 🔹 3. How does React handle concurrent rendering?

Concurrent rendering allows React to **pause, resume, or discard rendering work** to keep the UI responsive. It’s achieved through **Concurrent Mode (React 18)** with features like `useTransition` and `Suspense`.

---

### 🔹 4. Difference between useMemo and useCallback?

| Hook                    | Purpose                         |
| ----------------------- | ------------------------------- |
| `useMemo(fn, deps)`     | Memoizes **computed value**     |
| `useCallback(fn, deps)` | Memoizes **function reference** |

Used to prevent unnecessary re-renders when passing functions or computed values as props.

---

### 🔹 5. What are React Suspense and lazy loading?

`Suspense` lets you wait for something before rendering (like a lazy-loaded component or data).
Example:

```jsx
const Profile = React.lazy(() => import('./Profile'));
<Suspense fallback={<Spinner />}>
  <Profile />
</Suspense>
```

---

### 🔹 6. How do you manage global state in a large-scale React app?

* **Redux Toolkit** (standard for enterprise apps)
* **React Query / SWR** for server-state
* **Recoil or Zustand** for simpler use-cases
* Context + Reducer for small-scale shared states

---

### 🔹 7. What are controlled vs uncontrolled components?

* **Controlled:** React manages form input state
* **Uncontrolled:** DOM manages input state via refs
  Controlled ensures data consistency but more re-renders.

---

### 🔹 8. What is prop drilling and how to avoid it?

Prop drilling = passing props through multiple layers unnecessarily.
Avoid using:

* Context API
* Redux / Zustand
* Custom hooks for encapsulated logic

---

### 🔹 9. Explain React’s reconciliation algorithm.

It uses:

* **Diffing:** Compares virtual DOM trees
* **Keys:** Identify elements uniquely
* **Fiber:** Enables incremental rendering

If keys differ → React re-renders that node.

---

### 🔹 10. How do you handle performance issues in React?

✅ Use `React.memo`, `useMemo`, `useCallback`
✅ Split large components
✅ Lazy load heavy modules
✅ Virtualize long lists (e.g., `react-window`)
✅ Optimize rendering with keys
✅ Avoid unnecessary state updates

---

### 🔹 11. Explain React’s batching behavior.

React batches multiple state updates in one render for performance.
In React 18, batching happens for **all updates** (including async calls).

---

### 🔹 12. What are custom hooks and when to use them?

Reusable logic extracted from components.
Example:

```jsx
function useFetch(url) {
  const [data, setData] = useState();
  useEffect(() => { fetch(url).then(r => r.json()).then(setData); }, [url]);
  return data;
}
```

---

### 🔹 13. How do you handle error boundaries?

They catch JS errors in the component tree and display fallback UI.

```jsx
class ErrorBoundary extends React.Component {
  componentDidCatch(error) { log(error); }
  render() { return this.props.children; }
}
```

---

### 🔹 14. What is hydration in React?

Hydration attaches React’s virtual DOM to **server-rendered HTML (SSR)**, enabling interactivity without re-rendering the whole page.

---

### 🔹 15. What is SSR, SSG, and CSR?

| Type    | Description                              |
| ------- | ---------------------------------------- |
| **CSR** | Rendering happens in browser             |
| **SSR** | Server renders HTML on request (Next.js) |
| **SSG** | Static HTML pre-generated at build time  |

---

### 🔹 16. Explain reconciliation vs rendering.

* **Reconciliation:** Determines *what* changed (diffing).
* **Rendering:** Applies changes to the real DOM.

---

### 🔹 17. How do you handle large lists in React?

Use **windowing/virtualization** via `react-window` or `react-virtualized` — render only visible items for better performance.

---

### 🔹 18. What’s the difference between context and Redux?

| Context API           | Redux                             |
| --------------------- | --------------------------------- |
| Built-in              | External library                  |
| No middleware         | Middleware support                |
| Simple for small apps | Best for enterprise/global states |
| No DevTools           | Great DevTools                    |

---

### 🔹 19. Explain Pure Components.

They perform a **shallow comparison** of props and state to skip re-renders. Equivalent of `React.memo` for class components.

---

### 🔹 20. Explain the use of useTransition and useDeferredValue.

* `useTransition`: makes state updates **non-blocking** (for concurrent rendering).
* `useDeferredValue`: delays re-rendering of expensive components.

---

### 🔹 21. What are higher-order components (HOC)?

Functions that take a component and return an enhanced component.
Example:

```jsx
function withLogger(WrappedComponent) {
  return (props) => { console.log(props); return <WrappedComponent {...props} /> }
}
```

---

### 🔹 22. How to optimize React app loading time?

* Code splitting (lazy)
* Minimize bundle size (Tree Shaking)
* Use CDN and caching
* Compress images
* Prefetch critical assets

---

### 🔹 23. Difference between React 17 and 18?

* Automatic batching for all updates
* `useId`, `useTransition`, `useDeferredValue`
* Streaming SSR
* Concurrent features

---

### 🔹 24. What is React Query and how is it better than Redux for API calls?

React Query manages **server-state**, handles caching, retries, pagination, and background refetching — no reducers or actions needed.

---

### 🔹 25. Explain Refs and useImperativeHandle.

Refs allow direct DOM access; `useImperativeHandle` customizes what a ref exposes to parent components.

---

### 🔹 26. Difference between mounting and updating phases.

* **Mounting:** component created → constructor → render → DOM insert → `componentDidMount`
* **Updating:** props/state change → render → DOM update → `componentDidUpdate`

---

### 🔹 27. How do you debug React performance?

Use React DevTools → Profiler tab → check “Commit” durations and flame graph to identify re-renders.

---

### 🔹 28. What is React hydration mismatch?

Occurs when **server-rendered HTML** doesn’t match client-rendered output (common in SSR with dynamic data).

---

### 🔹 29. How do you prevent memory leaks?

* Clean up side effects in `useEffect`
* Abort fetch requests
* Unsubscribe from event listeners

---

### 🔹 30. Real-world question:

👉 *If your React app becomes slow after adding large datasets and multiple states — what will you check?*
✅ Check for unnecessary renders
✅ Optimize state placement (lift only when needed)
✅ Use virtualization
✅ Use memoization hooks
✅ Avoid anonymous functions in JSX
✅ Lazy load routes/components

---
