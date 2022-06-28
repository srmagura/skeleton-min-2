This demonstrates a ESM/CJS module resolution bug in Next.js.

## Repro Steps

1. Run `yarn`.
2. Copy the `react-loading-skeleton` directory from `copy-this-into-node-modules` into `node_modules`. This is a completely trimmed-down version of the library https://github.com/dvtng/react-loading-skeleton.
3. Run `yarn dev`.
4. Go to http://localhost:3000/.

### Actual Behavior

The server prints:

```text
----- CJS USED -----
Skeleton: { default: [Function: Skeleton] }

...

Error: Element type is invalid: expected a string (for built-in components) or a class/function (for composite components) but got: object.
```

If the line `return <Skeleton />;` is changed to `return <div />;`, the browser prints:

```text
----- ESM USED -----
index.js?bee7:3 Skeleton: ƒ Skeleton() {
    return react__WEBPACK_IMPORTED_MODULE_0__.createElement('div');
}
```

### Expected Behavior

The server should be using `react-loading-skeleton/dist/index.mjs` (the ES module), instead of `react-loading-skeleton/dist/index.js` (the CommonJS module).
