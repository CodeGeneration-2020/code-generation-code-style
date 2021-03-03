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

- ### Use async/await instead of then/catch.

> Instead of

```javascript
getListOfProducts()
  .then(({ data }) => setData(data))
  .catch()
```

> Just use

```javascript
try {
  const { data } = await getListOfProducts();
} catch(error) {
  ...
}
```

- ### Always try to move reusable methods in utils.js file.

> Instead of writing similiar methods again and again just use

```javascript
import { calculateTotalPrice } from '../utils.js';
```

- ### ALWAYS avoid nested if/else statements.

> Instead of

```javascript
if (isLoggedIn) {
  setLoggedIn(true);
  if (isPremium) {
    return { premiumStatus: 'activated' };
  } else {
    return { premiumStatus: 'not_available' };
  }
}
```

> Better to use

```javascript
if (isLoggedIn) {
  return isPremium ? 
    { premiumStatus: 'activated' } :
    { premiumStatus: 'not_available' };
}
```

## **Hacks and tricks**

- ### To remove duplicates from array, you can use new Set() or new Map() contstructors.

> With primitives inside

```javascript
const unique = [...new Set(array)];
```

> With complex data types;

```javascript
const removeDuplicates = (array, key) => {
  return [...new Map(array.map(item => [item[key], item])).values()]
}
```

- ### To find a difference between 2 arrays.

> With primitives inside

```javascript
const difference = biggerArray.filter(x => !smallerArray.includes(x));
```

> With complex data types;

```javascript
const difference = biggerArray.filter(x => !smallerArray.some(y => y.id === x.id));  
```

- ### Check if array contains false value / exclude false values.

```javascript
const arrayWithFalseValues = ['value_1', 'value_2', 'value_3', null, 'value_4', undefined, 0];

const operateWithFalseValues = (array, type) => {
  const excludeFalseValues = array.filter(item => !!item === true);
  if (type === 'exclude') {
    return excludeFalseValues;
  } else {
    return excludeFalseValues.length !== array.length;
  }
};
```
