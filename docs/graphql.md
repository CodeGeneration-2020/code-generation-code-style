# GraphQL ⚡

[Return to Table of Contents](../README.md)

## Shema advices

### Use CamelCase for fields ✅

> Use `camelCase` for all schema fields to keep your code base consistent  
> Also try to define your backend models/fields with `camelCase`

```graphql
type Task {
    id: ID!
    name: String!
    complete: Boolean!
}
```

### Use capitalization for your enums ✅

> Use capitalization for your `enums` to keep your teammates happy :)  

```graphql
enum Status {
    DONE
    IN_PROGRESS
    REJECTED
}
```

### Use scalar custom types for specific field ✅

> Use custom scalar types if you have some specific requirements for  basic types. For example you can right your own `Email` type  
> instead of using `String` in your schema.  
> Link: [Official docs about custom types](https://www.apollographql.com/docs/apollo-server/schema/custom-scalars/)


```javascript

// Schema code
const typeDefs = gql`
  scalar Email

  type User {
    id: ID!
    email: Email!
  }

`;
```

> Define logic related to scalar type in your resolver.

```javascript

const { GraphQLScalarType, Kind } = require('graphql');

const emailScalar = new GraphQLScalarType({
  name: 'Email',
  description: 'Email custom scalar type',
  serialize(value) {
    /// Logic For email validation
  },
  parseValue(value) {
    /// Logic For email validation
  },
  parseLiteral(value) {
    if (value.kind !== Kind.STRING) {
      throw new GraphQLError(
        `Email could be only string: ${value.kind}`,
      );
    }
    /// Logic For email validation
  },
});

const resolvers = {
  Email: emailScalar
  // ...other resolver definitions...
};

const server = new ApolloServer({
  typeDefs,
  resolvers
});
```

### Use enums if you field require a few specific values ✅

> In case if your schema should use few specific values in field you should define `enum` for that  
> instead of basic types.

```graphql
scalar Email

enum Status {
    APPROVED
    IN_PROGRESS
    REJECTED
}

type User {
    id: ID!
    status: Status
    email: Email
}
```

### Use proper naming for sort, filter, pagination ✅

> Lets use `sort`, `filter`, `pagination` inputs accordingly.

```graphql

enum UserSort {
    ASC
    DESC
}

input SortInput {
    type: UserSort
}

input FilterInput {
    search: String
    byName: String
    byRating: Integer
}
input PaginationInput {
    limit: Integer!
    offset: Integer!
}

type Query {
    users(sort: SortInput, filter: FilterInput, pagination: PaginationInput)
}
```

### Use mutation namespaces ✅

> Use namespaces for you mutations for better logic structure.

```javascript

// Create Namespace type for UserMutations
const UserMutations = new GraphQLObjectType({
  name: 'UserMutations',
  fields: () => {
    follow: {
      type: GraphQLBoolean,
      resolve: () => { /* resolver code */ },
    },
    unfollow: {
      type: GraphQLBoolean,
      resolve: () => { /* resolver code */ },
    },
  },
});

const Mutation = new GraphQLObjectType({
  name: 'Mutation',
  fields: () => {
    users: {
      type: UserMutations,
      resolve: () => ({}),
    }
  },
});
```

> This code gives your ability to use your mutations like that:

```graphql
mutation {
  users {
    follow(id: 15)
  }
}
```
