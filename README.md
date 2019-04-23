# Cancelled in favor of Promise.prototype.flatten

See [Promise.prototype.flatten](https://github.com/asilvas/proposal-promise-flatten)


# `safeawait` keyword

- Spec in progress


## Champions

- Work in progress


## Status

This proposal is stage 0 in the [TC39 Process](https://tc39.github.io/process-document/).


## Inspired By

https://twitter.com/DavidWells/status/1119729914876284928


## Examples

While `try -> catch` works well, it introduces a lot of noise into software that can be avoided.

Consider:

```
async function test(promise1, promise2, promise3) {
  let val1, val2, val3;

  try {
    val1 = await promise1;
  } catch (ex) {
    // ignore exceptions
  }

  try {
    [val2, val3] = await Promise.all([promise2, promise3]);
  } catch (ex) {
    throw ex; // throw to caller
  }

  return val1 + val2 + val3;
}
```

Above contains a fair amount of boilerplate for a relatively simple logic flow. Now let's look at
an alternative using `safeawait`:

```
async function test(promise1, promise2, promise3) {
  const [, val1] = safeawait promise1; // ignore exceptions
  const [err, [val2, val3] = []] = safeawait Promise.all([promise2, promise3]);

  if (err) throw err; // throw to caller

  return val1 + val2 + val3;
}
```

Under the covers both examples accomplish the same, but `safeawait` can reduce code footprint and increase readability.


## Polyfill vs Transpile

Unfortunately keywords cannot be polyfilled, however `safeawait` can easily be built into
transpilers like [babel](https://babeljs.io/). Chances are, the overhead generated by the
transpiler extension will still net you considerable code reductions with even a modest usage
of `safeawait`.


## Promise.safe

An alternative direction would be to expose `Promise.safe(promise)`. While simpler to implement and polyfill,
it's believed that the long-term direction of turning this behavior into a first-class keyword will not only
reduce code footprint, it will eventually be able to be optimized away by ECMAScript engines once natively supported.


## Discussion

https://twitter.com/Aaron_Silvas/status/1120050278542090242
