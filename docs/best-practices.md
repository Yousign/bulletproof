# 🚦 Best practices

##  React Hooks: `useCallback` and `useMemo`

- *Avoid overusing* these hooks without good reason. If you don't have a specific performance issue, there is no need to add them systematically.

- *Measure performance before* introducing it. React already optimizes rendering very well in many cases, and adding unnecessary memorization can sometimes lead to unpredictable behavior.

- *Keep the code simple* and readable first. If you start to experience performance issues, this is where you might consider using useCallback or useMemo.

### `useCallback`

`useCallback` stores a function between renderings. It's useful to prevent functions from being recreated on each render, which can lead to unnecessary renders in child components 

#### When to use:
- If the function is passed to a child component wrapped in `React.memo`.
- If a function is included in a hook’s dependency array (e.g., `useEffect`, `useMemo`).

#### When not to use:
- When the function isn’t passed as a prop or included in dependencies.
- Avoid overuse, as it adds complexity and may have minimal impact if not needed.

⚠️ Avoid using it systematically everywhere, because useCallback also adds a small overhead (storing the function and checking dependencies), and the code complexity increases.

### `useMemo`

`useMemo` memoizes the result of expensive calculations, preventing recalculations on every render.

#### When to use:
- If a computationally expensive operation (e.g., filtering, sorting) needs optimization.
- If you’re passing an object/array prop to a memoized child component.

#### When not to use:
- Avoid using it for trivial computations.
- Overusing `useMemo` can reduce readability without significant performance gain.
