## Explanation

When using useCallback, it expects a function. Because useCallback looks at the function is being passed in and tries to figure out it's return type.

Thus, this is wrong:

```ts
const onClick = useCallback<string>((name) => {
    console.log('test');
});
```

This is right
```ts
const onClick = useCallback((name: string) => {
    console.log('test');
});
```

Or:
```ts
const onClick = useCallback<(name: string) => void>((name) => {
    console.log('test');
});
```