#### Use it to determine where to redirect a user that is **logged in** or not.

```javascript
import React, { ComponentType } from 'react';

const ProtectedRouteHOC = <T extends {}>(WrappedComponent: ComponentType<T>) => (props: T) => {
  return (
    props.isLoggedIn ? (
      <DashboardNavigation />
    ) : (
      <AuthFlowNavigation />
    )
  )
}

const mapStateToProps = ({ user }) => ({ isLoggedIn: user.isLoggedIn });

export default connect(mapStateToProps)(ProtectedRouteHOC);
```

> Usage

```javascript
// the one that holds both main flow navigation and auth flow navigation
ProtectedRouteHOC(RootComponent);
```
