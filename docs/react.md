# React 🌋

[Return to Table of Contents](../README.md)

## Content ✈️ 

  - [**Working with hooks**](#working-with-hooks)
  - [**Hacks and tricks**](#hacks-and-tricks)

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

- ### Try to use useMemo in case you have expensive calculations and do not forget to pass relevant dependencies this operation relies on.

```javascript
// runs only once while component`s initialization
const memoizedValue = useMemo(() => expensiveOperation(), []);

// runs every time X or Y get changed 
const memoizedValue = useMemo(() => expensiveOperation(X, Y), [X, Y]);
```

## **Hacks and tricks**

- ### In case you have mapped values and a function that should be passed into each child - use closure and avoid in-line functions.

> Instead of
```javascript
const getSomeValue = (id) => { // some operations }

map(({ id }) => <ChildComponent onGetValue={() => getSomeValue(item.id)} />)  
```
> Use
```javascript
const getSomeValue = (id) = () => { // some operations }

map(({ id }) => <ChildComponent onGetValue={getSomeValue(id)} />)  
```
