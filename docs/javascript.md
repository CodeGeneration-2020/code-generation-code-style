# JavaScript ⚡

[Return to Table of Contents](../README.md)

## Content ✈️ 

  - [**Optimize your code-base**](#optimize-your-code-base)
  - [**Hacks and tricks**](#hacks-and-tricks)

## **Optimize your code-base**

- ### Try to always use object mappings and avoid if / else statements.

> This hack is optional, but can be used quite frequently to avoid redundant code spamming.

```javascript
  const STATUSES = {
    pending: 'Please wait',
    approved: 'Your payment has been approved!',
    rejected: 'Your payment has been rejected!',
  };

  const handledStatus = STATUSES[receivedStatus];
```
> Instead of

```javascript
  let handledStatus;
  if (receivedStatus === 'pending' {
    handledStatus = 'Please wait';
  } else if (receivedStatus === 'approved') {
    handledStatus = 'Your payment has been approved!';
  } else if (receivedStatus === 'rejected') {
    handledStatus = 'Your payment has been rejected!';
  }
```
> You can even use it with functions to handle extra operations

```javascript
  const STATUSES = {
    pending: () => {
      toggleLoader(true)
      return 'Please wait';
    },
    approved: () => {
      toggleLoader(false);
      return 'Your payment has been approved!';
    },
    rejected: () => {
      toggleLoader(false);
      return 'Your payment has been rejected!';
    },
  };
  
  const handledStatus = STATUSES[receivedStatus]();
```

- ### Try to always use ternary expressions. It helps to reduce redundant lines of code.

```javascript
  const status = user?.id ? 'online' : 'offline:
```

> Instead of

```javascript
  let status;
  if (user?.id) {
    status = 'online';
  } else {
    status = 'offlne'
  }
```

## **Hacks and tricks**

- ### To remove duplicates from array, you can use new Set contstructor in combination with spread operator.

> With primitives inside

```javascript
  const unique = [...new Set(array)];
```

> With complex data types;

```javascript
  const removeDuplicates = (array, key) => {
    return [...new Map(arr.map(item => [item[key], item])).values()]
  }
```
