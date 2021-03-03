# React 🌋

[Return to Table of Contents](../README.md)

## Content ✈️ 

  - [**Working with hooks**](#working-with-hooks)
  - [**Optimization**](#optimization)
  - [**Redux**](#redux)

## **Working with hooks**

- ### Try to always move complex mount logic into a dedicated function.

> Instead of

```javascript
useEffect(() => {
  getUserData()
    .then()
    .catch()
    // ...another complex logic
}, []);
```

> Just do

```javascript
useEffect(() => {
  // a function with all the mount operations inside
  initializeData();
}, []);
```
#### `NOTE!`
Do not use useEffect quite frequently, since it might significantly slow down the performance of your app as well as lead to some unexpected issues with while re-rendering.
Ideally there should be only one useEffect(() => {}, []) - which is **didMount**
