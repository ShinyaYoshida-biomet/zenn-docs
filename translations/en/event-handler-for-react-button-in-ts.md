---
title: "Caution When Setting Default Args for Event Handlers in React"
emoji: "🔨"
type: "tech" # tech: technical article / idea: idea article
topics: [JavaScript, React, TypeScript]
published: true
---

# Background

In `React.js`, event handlers receive an event object (such as `MouseEvent`) as the first argument.
Humans are fallible creatures, and we sometimes forget this basic fact and **set incorrect default arguments, causing bugs in our code**.

I have personally experienced a bug where the return value of an event handler became NaN.
Since the bug did not explicitly throw an error, it took a long time to identify the cause, which was very stressful.

Therefore, I decided to document this incident as a lesson for myself.

# Problematic Code Snippet

```jsx
export default function App() {
  // A function that assumes numbers are passed in the order of a, b
  // However, if passed directly to onClick, a will be a mouse event
  const handleClick = (a = 1, b = 5) => {
    console.log("------- args : ", a, b);
    console.log("a - b = ", a - b);
    return a - b;
  };

  return (
    <div
      style={{
        display: "flex",
        flexDirection: "column",
        justifyContent: "center",
        gap: "10px",
      }}
    >
      <h1 style={{ textAlign: "center" }}>
        Caution When Setting Default Args for Event Handlers
        <br />
        in React
      </h1>

      {/* Correct order (e.g., a=5, b=3) */}
      <button onClick={() => handleClick(5, 3)}>Case 1: (a=5, b=3) = 2</button>

      {/* No arguments specified but using an anonymous function */}
      <button onClick={() => handleClick()}>Case 2: (a=1, b=5) = -4</button>

      {/* Event object goes into a, but no error occurs (NaN is returned) */}
      <button onClick={handleClick}>Case 3: ??? = ???</button>
    </div>
  );
}
```

![alt text](/images/doc11/image.png)

At first glance, the code seems fine, but when executed, the results are as follows:

- Case 1: (a=5, b=3) → 2 is returned (normal operation)
- Case 2: (a=1, b=5) → -4 is returned (also normal operation)
- Case 3: NaN is returned

# Cause and Prevention

The reason Case 3 returns NaN is that in React event handlers, the first argument is the event object.
When `onClick={handleClick}` is passed directly without wrapping in an anonymous function,
the event object goes into `a`, while `b` is assigned the default argument 5.
As a result, `a - b` cannot perform numerical calculations, and NaN is returned.

The tricky part is that in JavaScript, due to dynamic typing, it is not explicitly an error.
However, in TypeScript, it can be detected as an error as follows:

```text
Type '(a?: number, b?: number) => number' is not assignable to type 'MouseEventHandler<HTMLButtonElement>'.
```

## Prevention
To summarize the prevention measures:

1. **Strictly type with TypeScript**
   Design to avoid mixing types that receive event objects and functions that perform numerical calculations.
2. **Separate event object handling and calculation processing**
   Design to separate roles, such as having the event handler receive the MouseEvent and call a business logic function within it.
3. **Explicitly specify arguments with anonymous functions**
   Always wrap in an anonymous function when passing arguments accurately, like `onClick={() => handleClick(5, 3)}`.

# Conclusion
In conclusion, I have chosen to **"live with TypeScript"**.
JavaScript has high flexibility but also the drawback of easily allowing such bugs to slip in and not explicitly throwing runtime errors.
Taking this incident as a lesson, I vow to use type checking to prevent bugs in the future.
