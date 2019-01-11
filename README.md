# GraphQLess

Write GraphQL like you would vanilla Express.js.

## Setup

```bash
yarn install graphqless
```

And here is how you write a server... Look familiar?

```jsx
const graphQLess = require('graphqless');
const app = new graphQLess();

const db = { users: [{ name: 'Tyler' }] };

app.get('/users', (req, res) => res.send(db.users));
app.get('/user', (req, res) =>
  res.send(db.users.find(user => user.name === req.body.name))
);
app.post('/createUser', (req, res) =>
  res.send(db.users.push({ name: req.body.name }))
);

// This is the only substantial difference
// you need to write a schema that describes the
// .get and .post functions inputs and outputs
// .get === Query && .post === Mutation
app.useSchema(`
  type Query {
    users: [User]
    user(name: String): User
  }
  type Mutation {
    createUser(name: String): Int
  }
  type User {
    name: String
  }
`);

app.listen(3000, () => {
  console.log('Visit: http://localhost:3000/playground');
});
```

You can find more examples in the [examples](/examples) folder.

## Queries for examples/example.js

```bash
npx nodemon examples/example.js
```

```graphql
mutation createUser {
  createUser(name: "Buchea")
}

query getUsers {
  users {
    name
  }
  user(name: "Tyler") {
    name
  }
}
```

## Queries for examples/exampleWithAuth.js

```bash
npx nodemon examples/exampleWithAuth.js
```

```graphql
query getToken {
  getToken
}

# Add this to "HTTP HEADERS" in GraphQL Playground:
# { "Authorization": "Bearer eyJhbGciOiJIUzI1NiJ9.YWJj.4noRC-c0ay0hOeZ5Cgc80MVS0P4p4FrR2lJFzMNSnE4" }

query getMe {
  me {
    id
    name
  }
}
```
