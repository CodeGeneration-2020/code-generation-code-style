```javascript
import React, { ComponentType } from 'react';
import { get } from 'lodash'; 

interface HeaderValues {
  absolute?: boolean; // for transparent header and specific screen configurations
  title: {
    item?: string; // item in the center
  };
  headerRight: { // item from the right side
    item?: Element;
    press?: () => void;
  };
  headerLeft: { // item from the left side
    item?: Element;
    press?: () => void;
  };
}

const withHeaderHOC = <T extends {}>(WrappedComponent: ComponentType<T>) => (
  props: T,
) => {
  const header = get(props, 'route.params.header', {});
  const headerValues: HeaderValues[] = Object.values(header);
  return (
    <SafeAreaView style={styles.safeAreaWrapper}>
      {header && <Header headerItemsArray={headerValues} />}
      <WrappedComponent {...props} />
    </SafeAreaView>
  );
};

const styles = StyleSheet.create({
  safeAreaWrapper: {
    height: '100%',
    backgroundColor: colors.primaryWhite,
    paddingBottom: 4,
  },
});
```
