Perfect — here’s a **React Hooks Interview Q&A Set (for 7+ years experience)** — optimized for **last 30 minutes of interview prep** 🚀

It’s divided into **Core Hooks**, **Advanced Hooks**, and **Real-world Scenarios** — all senior-level.

---

## ⚛️ **React Hooks — Senior-Level Interview (30 min revision)**

---

### 🔹 **1. What are React Hooks and why were they introduced?**

Hooks let you **use state and lifecycle** features in functional components — introduced to avoid class components and simplify code reuse.

---

### 🔹 **2. Rules of Hooks**

1. Call hooks **only at the top level** (not inside loops/conditions).
2. Call hooks **only in React functions** (not normal JS functions).

---

### 🔹 **3. useState**

Used for local state management in a functional component.

```js
const [count, setCount] = useState(0);
```

Replaces `this.state` and `this.setState`.

---

### 🔹 **4. useEffect**

Handles **side effects** like API calls, subscriptions, timers.

```js
useEffect(() => {
  fetchData();
  return () => console.log('Cleanup'); // unmount
}, [dependency]);
```

Runs after render; cleanup runs before next effect or unmount.

---

### 🔹 **5. useContext**

Shares global data without prop drilling.

```js
const value = useContext(MyContext);
```

Used with `createContext()` and a Provider.

---

### 🔹 **6. useReducer**

Used for **complex state logic** (similar to Redux).

```js
const [state, dispatch] = useReducer(reducer, initialState);
```

Replaces multiple useState calls for structured state updates.

---

### 🔹 **7. useRef**

Keeps **mutable values** between renders or references DOM elements.

```js
const inputRef = useRef();
useEffect(() => inputRef.current.focus(), []);
```

---

### 🔹 **8. useMemo**

Memoizes **expensive computations** to avoid re-running on every render.

```js
const result = useMemo(() => heavyCalculation(data), [data]);
```

---

### 🔹 **9. useCallback**

Memoizes **function references** to prevent unnecessary re-renders of child components.

```js
const handleClick = useCallback(() => console.log('clicked'), []);
```

---

### 🔹 **10. useLayoutEffect**

Runs **synchronously after DOM mutations** (before paint).
Used for reading layout or DOM measurements.
⚠️ More blocking than `useEffect`.

---

### 🔹 **11. useImperativeHandle**

Customizes what’s exposed through `ref` in a child component.

```js
useImperativeHandle(ref, () => ({ focus: () => inputRef.current.focus() }));
```

---

### 🔹 **12. useId**

Generates **unique IDs** for accessibility and SSR consistency.

```js
const id = useId();
<label htmlFor={id}>Name</label>
<input id={id} />
```

---

### 🔹 **13. useTransition**

Allows **non-blocking UI updates** by marking state updates as “transitions”.

```js
const [isPending, startTransition] = useTransition();
startTransition(() => setValue(newValue));
```

---

### 🔹 **14. useDeferredValue**

Delays updates of expensive computations.

```js
const deferredValue = useDeferredValue(value);
```

Useful for search filters and typeahead UIs.

---

### 🔹 **15. useDebugValue**

Used inside custom hooks for debugging in React DevTools.

```js
useDebugValue(isOnline ? 'Online' : 'Offline');
```

---

## 🧩 **Advanced Hook Concepts**

---

### 🔹 **16. Custom Hooks**

Encapsulate reusable logic.

```js
function useFetch(url) {
  const [data, setData] = useState(null);
  useEffect(() => { fetch(url).then(res => res.json()).then(setData); }, [url]);
  return data;
}
```

Used to share logic between components (without HOC/render props).

---

### 🔹 **17. Difference between useMemo & useCallback**

| Hook        | Returns               | Used For                 |
| ----------- | --------------------- | ------------------------ |
| useMemo     | Memoized **value**    | Expensive calculations   |
| useCallback | Memoized **function** | Passing stable callbacks |

---

### 🔹 **18. useEffect vs useLayoutEffect**

| Hook            | Timing       | Use Case                     |
| --------------- | ------------ | ---------------------------- |
| useEffect       | After paint  | Fetching data, subscriptions |
| useLayoutEffect | Before paint | DOM measurement, sync layout |

---

### 🔹 **19. useState lazy initialization**

You can initialize with a function (runs once):

```js
const [value] = useState(() => heavyInit());
```

---

### 🔹 **20. Avoiding stale closures**

Use function updates or refs to access latest values inside async calls:

```js
setCount(prev => prev + 1);
```

---

## 🧠 **Real-World Scenario Questions**

---

### 🔹 **21. How do you optimize re-rendering with hooks?**

✅ Use `React.memo`, `useMemo`, and `useCallback`.
✅ Split components logically.
✅ Lift state only when necessary.

---

### 🔹 **22. What are pitfalls of useEffect?**

* Missing dependencies cause stale data.
* Over-dependency causes infinite loops.
* Cleanup not handled → memory leaks.

---

### 🔹 **23. How do you debounce API calls with hooks?**

```js
useEffect(() => {
  const id = setTimeout(() => fetchData(query), 300);
  return () => clearTimeout(id);
}, [query]);
```

---

### 🔹 **24. How to persist state using hooks?**

Use `useEffect` + `localStorage`:

```js
useEffect(() => localStorage.setItem('theme', theme), [theme]);
```

---

### 🔹 **25. How does React preserve hook state across renders?**

React keeps a **linked list of hooks** for each component Fiber; the order of hooks must remain same across renders.

---

### 🔹 **26. Can you use hooks in class components?**

❌ No, hooks only work in functional components.
But you can migrate class lifecycles → hooks (e.g., `componentDidMount` → `useEffect`).

---

### 🔹 **27. Difference between custom hook and HOC**

| Feature     | Custom Hook | HOC                    |
| ----------- | ----------- | ---------------------- |
| Type        | Function    | Component wrapper      |
| Reusability | Logic reuse | UI + logic reuse       |
| Simplicity  | Cleaner     | Can cause nesting hell |

---

### 🔹 **28. How to cancel async requests in useEffect**

```js
const controller = new AbortController();
fetch(url, { signal: controller.signal });
return () => controller.abort();
```

---

### 🔹 **29. What is batching in hooks?**

Multiple state updates inside events are **batched into one render** in React 18, improving performance.

---

### 🔹 **30. Explain React’s hook execution order**

React executes hooks in the order they appear — that’s why conditional hooks break the sequence and cause errors.

---

## ⚡ Quick Recap Table

| Hook                | Purpose                         |
| ------------------- | ------------------------------- |
| useState            | Local state                     |
| useEffect           | Side effects                    |
| useContext          | Access context                  |
| useReducer          | Complex state                   |
| useRef              | DOM or persistent mutable value |
| useMemo             | Memoize values                  |
| useCallback         | Memoize functions               |
| useLayoutEffect     | DOM sync                        |
| useImperativeHandle | Control ref exposure            |
| useId               | Unique IDs                      |
| useTransition       | Non-blocking UI                 |
| useDeferredValue    | Delayed rendering               |
| useDebugValue       | Debug custom hooks              |

---

Would you like me to give you a **“1-page cheat sheet” PDF** summarizing all React hooks with examples (ideal for last-minute revision)?
