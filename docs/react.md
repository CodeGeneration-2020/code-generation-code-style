# React 🌋

[Return to Table of Contents](../README.md)


## **Working with hooks**

### Try to always move complex mount logic into a dedicated function ✅

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

### Try to use useMemo in case you have expensive calculations ✅

```javascript
// runs only once while component`s initialization
const memoizedValue = useMemo(() => expensiveOperation(), []);

// runs every time X or Y get changed 
const memoizedValue = useMemo(() => expensiveOperation(X, Y), [X, Y]);
```

## Communication with your server

### Use react-query ✅

> In our company if your project doesn't use [GraphQL](./graphql.md) we prefer to use [react-query](https://react-query.tanstack.com/).  
> This lib helps you to solve two problems in once: global data storage and clear way for interaction with your backend.  

### Move query names to constant ✅

```javascript
/// global-constant.js
const REACT_QUERY_KEYS = {
  DATA: 'data',
};
export default REACT_QUERY_KEYS;
```

```javascript
/// component.js
import REACT_QUERY_KEYS from './path-to-global-const/global-constant';
function Example() {
   const { isLoading, error, data } = useQuery(REACT_QUERY_KEYS.DATA, () =>
      // Some code
   )
 
 
   return (<div>Example</div>)
 }

```

### Data fetching function ✅

> Try to hide all logic related to data serialization in your `service` file to keep your code clear.  
> Check our [best practices about services](./javascript.md#server-interations).

```javascript
import REACT_QUERY_KEYS from './path-to-global-const/global-constant';
import Service from './path-to-service'
function Example() {
   const { isLoading, error, data } = useQuery(REACT_QUERY_KEYS.DATA, Service.getSomeData)
 
 
   return (<div>Example</div>)
 }
```

## Styled component

### Use proper naming ✅

> In a large project its really hard to define where is `styled` components and where is `react` component.  
> So it make sense to import all `styled` components via `*`

```javascript

// component.styled.js

export const Button = styled('button')`
  color: red;
  border: 1px solid black;
`

```

> And use it in your component as `Styled.Button`

```javascript
import React from 'react'

import * as Styled from './component.styled'
import OtherComponent from './other.component'

const App = () => {
  return (
    <div>
      <OtherComponent/>
      <Styled.Button>Button Text</Styled.Button>
    </div>
  )
}
```

> With that syntax you know that `OtherComponent` - real react component and `Styled.Button` - `styled` component.

### Proper props naming ✅

> To make your code more readable you should define props for styled component with `$`.

```javascript
 const Button = styled.button`
  color: ${(p) => p.$color};
`;
 
const Component = ({ color, disabled, onClick }) => {
  return (
    <Button
      $color={color}
      disabled={disabled}
      onClick={onClick}
    >
      Click Me
    </Button>
  );
};
```

### Extend your style components ✅

> When you have some reusable logic in styled components it make sense to move it to `base` component and extend children  with that.  

```javascript
  const FlexContainer = styled('div')`
    font-size: 20px;
    display: flex;
    background-color: red;
  `;

  const TaskCardContainer = styled(FlexContainer)`
    border: 1px solid black;
    border-radius: 5px;
    background-color: white;
  `

  const UserProfileCardContainer = styled(FlexContainer)`
    font-weight: bold;
    color: blue;
  `
```
## **Hacks and tricks**

### In case you have mapped values and a function that should be passed into each child - use closure and avoid in-line functions. ✅

> Instead of

```javascript
const getSomeValue = (id) => { // some operations }

map(({ id }) => <ChildComponent onGetValue={() => getSomeValue(id)} />)  
```

> Use

```javascript
const getSomeValue = (id) = () => { // some operations }

map(({ id }) => <ChildComponent onGetValue={getSomeValue(id)} />)  
```
