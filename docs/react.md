# React 🌋

[Return to Table of Contents](../README.md)

## Basic conventions

#### Import
> Instead of
```javascript
import * as React from 'react';
```
> Use
```javascript
import React, { useState, Component } from 'react';
```

#### Typed component state for non trivial values
> On the rare occasions where you need to initialize a hook with a null-ish value, you can make use of a generic and pass a union to correctly type your hook. See this instance:
```javascript
type User = {
  email: string;
  id: string;
}

// the generic is the < >
// the union is the User | null
// together, TypeScript knows, "Ah, user can be User or null".
const [user, setUser] = useState<User | null>(null);
```

#### Reducer example
> The other place where TypeScript shines with Hooks is with userReducer, where you can take advantage of [`discriminated unions`](https://www.typescriptlang.org/docs/handbook/advanced-types.html#discriminated-unions). Here’s a useful example:
```javascript
type AppState = {};
type Action =
  | { type: "SET_ONE"; payload: string }
  | { type: "SET_TWO"; payload: number };

export function reducer(state: AppState, action: Action): AppState {
  switch (action.type) {
    case "SET_ONE":
      return {
        ...state,
        one: action.payload // `payload` is string
      };
    case "SET_TWO":
      return {
        ...state,
        two: action.payload // `payload` is number
      };
    default:
      return state;
  }
}
```

#### Extending Component Props
> Sometimes you want to take component props declared for one component and extend them to use them on another component. But you might want to modify one or two. Well, remember how we looked at the two ways to type component props, types or interfaces? Depending on which you used determines how you extend the component props. Let’s first look at the way using `type`:
```javascript
type ButtonProps = {
  /** the background color of the button */
  color: string;
  /** the text to show inside the button */
  text: string;
}

type ContainerProps = ButtonProps & {
  /** the height of the container (value used with 'px') */
  height: number;
}

const Container: React.FC<ContainerProps> = ({ color, height, width, text }) => {
  return <div style={{ backgroundColor: color, height: `${height}px` }}>{text}</div>
}
```

> If you declared your props using an `interface`, then we can use the keyword extends to essentially `extend` that interface but make a modification or two:
```javascript
interface ButtonProps {
  /** the background color of the button */
  color: string;
  /** the text to show inside the button */
  text: string;
}

interface ContainerProps extends ButtonProps {
  /** the height of the container (value used with 'px') */
  height: number;
}

const Container: React.FC<ContainerProps> = ({ color, height, width, text }) => {
  return <div style={{ backgroundColor: color, height: `${height}px` }}>{text}</div>
}
```

#### Privat methods or props
> With the new ECMAScript [`class fields`](https://github.com/tc39/proposal-private-methods) proposal we can do this easily and gracefully by using [`private fields`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_class_fields) as seen below:
```javascript
class Friends extends Component {
  #fetchProfileByID () {}

  render () {
    return // jsx blob
  }
}
```

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

type Props = React.PropsWithChildren<{
	color: string;
	disabled: boolean;
	onClick: () => void;
}>;
 
const Component = ({ color, disabled, onClick, children }: Props) => {
  return (
    <Button
      $color={color}
      disabled={disabled}
      onClick={onClick}
    >
      {children}
    </Button>
  );
};
```

> `ComponentPropsWithoutRef` is a generic type that supplies props for built-in React handlers and native HTML attributes. By passing in `"button"` as the template, you specify that the component is extending the HTML `button` element.

```javascript
 const Button = styled.button`
  color: ${(p) => p.$color};
`;

type Props = React.ComponentPropsWithoutRef<"button"> {
  color: string;
};
 
const Component = ({ children, onClick, color, type }: Props) => {
  return (
    <Button
      $color={color}
      onClick={onClick}
      type={type}
    >
      {children}
    </Button>
  );
};
```

### Styling: spacings / colors / fontSize / lineHeight ✅

> Try to always use predefined values in terms of styling your app in order to increase reusability of your code-base. Also it is important to keep in mind that spacings should be equal to `4`! Create, for instance, `theme.ts` file where you can keep all the possible values.

Instead of
```
text: {
  paddingVertical: 15;
  marginHorizontal: 13;
  lineHeight: 11;
  fontSize: 9;
  color: 'yellow'
}
```

Use
```
import { Spacings, LineHeight, FontSize, Colors } from '../../theme';

text: {
  paddingVertical: Spacings.s16;
  marginHorizontal: Spacings.s12;
  lineHeight: LineHeight.lh10;
  fontSize: FontSize.fs10;
  color: Colors.yellow90;
}
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

#### In case you have mapped values and a function that should be passed into each child - use closure and avoid in-line functions. ✅

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
