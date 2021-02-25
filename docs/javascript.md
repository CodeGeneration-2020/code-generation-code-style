# JavaScript ⚡

[Return to Table of Contents](../README.md)

## Content ✈️ 

  - [**Optimize your code-base**](#optimize-your-code-base)

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
