# Hooks

[進階觀念](https://kentcdodds.com/blog/write-fewer-longer-tests)
   To learn the more advanced React hooks and different patterns to enable great developer APIs for custom hooks.
   Use useReducer to manage state and avoid stale state bugs (and learn when it's preferable over useState)
   Optimize expensive operations with useMemo and useCallback
   Interact with third party DOM libraries with useLayoutEffect
   Learn when to use (and when not to use) useImperativeHandle and useDebugValue
   Create custom hooks for complex use cases
[什麼時候用useReducer](https://kentcdodds.com/blog/should-i-usestate-or-usereducer)

1. 某些情況適合用來取代useState。
2. 裡面舉例一個需要undo，redo功能的app，因為一個state會影響到另一個state的顯示，用useReducer會簡潔很多，把state的邏輯都移到了reducer裡了。
3. app就剩很單純的view和listener的部份。

## useEffect

