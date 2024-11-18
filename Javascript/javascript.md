### What is the difference in following code:

```ts
Array.new(2).map(() => { return "1" });

// vs

Array.from({length: 2}, () => { return "1" });
```

`map` tries to operate on elements, but since `Array.new` only reserves slots, there are no elements ( even `undefined` ones )
This in turn prevents `map` from saving the result - which outputs `array` with `n` `undefined` results.