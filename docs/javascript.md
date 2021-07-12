# JavaScript ⚡

[Return to Table of Contents](../README.md)

## **Optimize your code-base**

### Try to always use object mappings and avoid if / else statements. ✅

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

### Try to always use ternary expressions. It helps to reduce redundant lines of code. ✅

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

### Use async/await instead of then/catch. ✅

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

### Always try to move reusable methods in utils.js file ✅

> Instead of writing similiar methods again and again just use

```javascript
import { calculateTotalPrice } from '../utils.js';
```

### ALWAYS avoid nested if/else statements ✅

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

### Server Interactions. ✅

> You should extract repetitive parts of the code to the separate functions / classes
> Instead of

```javascript
// user.service.js

class UserService {
  getUsers() {
    return axios.get('http://localhost:4200/api/users',
      {headers: {
        'Authorization': localStorage.getItem('token'),
      }}
    )
  }
  updateUser(user) {
    return axios.put('http://localhost:4200/api/users',
      user,
      {headers: {
        'Authorization': localStorage.getItem('token'),
      }}
    )
  }
}
```

> Better to use

The best way to handle that is to create `HttpService` class which will be responsible for all the basic things in all services

```javascript
// http.service.js
import axios from 'axios'; // It could be any fetching services, such as default fetch, call api, xhr, etc.

class HttpSerivce {
  constructor(baseUrl = process.env.SERVER_URL, fetchingService = axios, apiVersion = 'api') {
    this.baseUrl = baseUrl;
    this.fetchingService = axios;
    this.apiVersion = apiVersion
  }

  private getFullApiUrl(url) {
    return `${this.baseUrl}/${this.apiVersion}/${url}`;
  }

  private populateTokenToHeaderConfig() {
    return {
      'Authorization': localStorage.getItem('token'),
    }
  }
  
  private extractUrlAndDataFromConfig({data, url, ...configWithoutDataAndUrl}) {
    return configWithoutDataAndUrl;
  }

  get(config, withAuth = true) {
    if (withAuth) {
      config.headers = {
        ...config.headers,
        ...populateTokenToHeaderConfig(),
      }
    }
    return this.fetchingService.get(this.getFullApiUrl(config.url), extractUrlAndDataFromConfig(config));
  }

  put(config, withAuth = true) {
    if (withAuth) {
      config.headers = {
        ...config.headers,
        ...populateTokenToHeaderConfig(),
      }
    }
    return this.fetchingService.get(this.getFullApiUrl(config.url), config.data, extractUrlAndDataFromConfig(config));
  }
}
```

```javascript
// user.service.js
import HttpService from './http.service';

class UserService extends HttpSerivce {
  constructor() {
    super();
  }
  getUsers() {
    return this.get({
      url: 'users',
    })
  }
  updateUser(user) {
    return this.post({
      url: 'users',
      data: user,
  })
}
```

### Always try to create models to serialize / deserialize your entities. ✅

> Instead of

```javascript
// user.service.js
class UserService extends HttpSerivce {
  constructor() {
    super();
  }
  async getUsers() {
    const response = await this.get({
      url: 'users',
    });
    return response.data.map(user => ({
      ...user,
      fullName: user.full_name,
      dayOfBirth: new Date(user.dob),
    }))
  }
}
```

> Better to use

```javascript
// user.model.js

class UserModel {
  constructor(id, fullName, dayOfBirth, status) {
    this.id = user.id;
    this.fullName = user.full_name;
    this.dayOfBirth =  new Date(user.dob);
    this.status = user.status;
  }
  
  getFormatedDayOfBirth() {
    return this.dayOfBirth.toDateString();
  }
}

const createUserModel = (userFromServer) => {
  return new UserModel(userFromServer.id, userFromServer.full_name, userFromServer.dob, userFromServer.status);
};

export {createUserModel};

export default UserModel;

```

```javascript
  // user.service.js
  import { createUserModel } from './user.model';

  class UserService extends HttpService {
    constructor() {
      super();
    }
    async getUsers() {
      const response = await this.get({
        url: 'users',
      });
      return response.data.map(user => createUserModel());
    }
}
```

> With that approach you can work with all the data that comes from your server, such as entities. So your code will have better level of abstraction. Also as you see you can attach useful methods to the entity model.

## **Hacks and tricks**

### To remove duplicates from array, you can use new Set() or new Map() constructors. ✅

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

### To find a difference between 2 arrays. ✅

> With primitives inside

```javascript
const difference = biggerArray.filter(x => !smallerArray.includes(x));
```

> With complex data types;

```javascript
const difference = biggerArray.filter(x => !smallerArray.some(y => y.id === x.id));  
```

### Check if array contains false value / exclude false values. ✅

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
