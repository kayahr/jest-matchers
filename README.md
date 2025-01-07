jest-matchers
=============

| :warning: This project is no longer maintained because the author switched from Jest to [Vitest](https://vitest.dev/) :warning: |
| ------------------------------------------------------------------------------------------------------------------------------- |

Some additional [Jest] matchers I usually use in my projects.


Usage
-----

* Install dependency:

    ```
    npm install --dev @kayahr/jest-matchers
    ```

* Import it in your Jest test file:

    ```typescript
    @import "@kayahr/jest-matchers";
    ```


Matchers
--------

### toEqualCloseTo(value, numDigits?)

Recursive version of [toBeCloseTo()] so you can compare arrays or objects with numeric values with a
specific precision. **numDigits** defaults to 2 if not specified.

```typescript
expect(values).toEqualCloseTo([ 10.3132, 40.1224, 50.1311 ], 4);
```

### toBeEquatable(equalValues, unequalValues)

Checks if the tested object has an `equals` method which correctly implements an equivalence relation on non-null object references (See Java's [object.equals()] method for details).

```typescript
expect(new Color(0, 0, 0)).toBeEquatable(
    [ new Color(0, 0, 0), new Color("black") ],
    [ new Color(1, 0, 0), new Color(0, 1, 0), new Color(0, 0, 1), new Color("white") ]
);
```

### toBeGarbageCollected(destructor, timeout)

Checks if the test object (which must be passed via a `WeakRef`) is correctly garbage collected. For this you have to specify a destructor function which
removes all references to the test object. You can also optionally specify a timeout which defaults to 5000 milliseconds. When the object was not garbage
collected in this time then the test fails.

```typescript
let testSubject: Record<string, unknown> | undefined = { a: 34 };
await expect(new WeakRef(testSubject)).toBeGarbageCollected(() => { testSubject = undefined; });
```

[Jest]: https://jestjs.io/
[toBeCloseTo()]: https://jestjs.io/docs/en/expect#tobeclosetonumber-numdigits
[object.equals()]: https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/lang/Object.html#equals(java.lang.Object)
