# js-signals

## Description

`js-signals` is a lightweight JavaScript library for creating reactive signals and effects, providing a powerful way to manage state and dependencies in your applications. The library includes features for deep proxy handling, computed values, and both synchronous and asynchronous effects. It's designed to help developers create reactive data flows with minimal boilerplate and maximum efficiency.

## Features

- **Signals**: Reactive data containers that automatically notify dependents when their value changes.
- **Effects**: Functions that automatically re-run when the signals they depend on change.
- **Async Effects**: Handle asynchronous side effects and dependencies seamlessly.
- **Computed Values**: Automatically update based on the signals they depend on.
- **Batching**: Efficiently batch multiple updates to avoid redundant computations.
- **Deep Proxies**: Automatically track changes in nested objects and arrays.
- **Watching**: Observe multiple signals and execute a callback when they change.

## Usage

1. **Creating Signals**:
   ```javascript
   import { createSignal } from 'js-signals';

   const count = createSignal(0);

   count.value = 1; // Update signal value
   console.log(count.value); // Access signal value
   ```

2. **Creating Effects**:
   ```javascript
   import { createEffect } from 'js-signals';

   createEffect(() => {
       console.log(`Count is: ${count.value}`);
       return () => {
           console.log(`Cleanup for count: ${count.value}`);
       };
   });

   count.value = 2; // Triggers effect and cleanup
   ```
   - **Note**: The callback function can return a cleanup function, which runs before the effect re-runs or when the effect is stopped.

3. **Creating Async Effects**:
   ```javascript
   import { createAsyncEffect } from 'js-signals';
   
   const baseUrl = 'https://api.example.com/posts'
   const postUrl = createSignal(`${baseUrl}?page=1`)

   createAsyncEffect(async () => {
       const response = await fetch(postUrl.value);
       const data = await response.json();
       console.log(data);
       return async () => {
           console.log('Cleanup for async effect');
       };
   });
   
   postUrl = `${baseUrl}?page=2`
   ```
   - **Note**: An asynchronous effect does not re-run until the current execution is complete, regardless of signal updates. This ensures that async operations are not prematurely interrupted by new updates.

4. **Creating Computed Values**:
   ```javascript
   import { createComputed } from 'js-signals';

   const doubleCount = createComputed(() => count.value * 2);

   console.log(doubleCount.value); // Access computed value
   ```

5. **Batching Updates**:
   ```javascript
   import { batch, createSignal } from 'js-signals';

   const count = createSignal(0);
   const doubled = createComputed(() => count.value * 2);

  createEffect(() => {
    console.log(doubled.value); // is logged once
  });

   batch(() => {
       count.value = 1;
       count.value = 2;
   });
   ```
   - **Note**: Batching allows you to group multiple signal updates together into one single update, ensuring efficient re-computation and notification.

6. **Watching Signals**:
   ```javascript
   import { watch, createSignal } from 'js-signals';

   const count = createSignal(0);
   const doubleCount = createSignal(0);

   watch([count, doubleCount], (values) => {
       console.log(`Count is ${values[0]}, DoubleCount is ${values[1]}`);
   });

   count.value = 2;
   doubleCount.value = 4; // Triggers watch callback
   ```
   - **Note**: Watching allows you to observe multiple signals and run a callback whenever any of them change.

## Installation

Install the library using npm:

```bash
npm install js-signals
```

## License

This project is licensed under the MIT License.

---

*Author: Adeniji Oluwaferanmi*
*GitHub: [Judeadeniji](https://github.com/Judeadeniji)*