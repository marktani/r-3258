# r-3258

Reproduction for https://github.com/prisma/prisma/issues/3258

## Reproduction

```
git clone git@github.com:marktani/r-3258.git
cd r-3258
docker-compose up -d
npm install -g prisma@beta # prisma/1.19.0-beta.2
prisma deploy
prisma playground
```

Run this query, and take a note of the `id` of the first user with `numFollowers` of `-10`. Let's call that `id` `__ID__`.

```graphql
query allUsers {
  users(
  orderBy: numFollowers_DESC,
  after: null,
  first: 20) {
    id
    numFollowers
  }
}
```

Then run this query again, this time using the previous id:

```graphql
query noUsers {
  users(
  orderBy: numFollowers_DESC,
  after: "__ID__",
  first: 20) {
    id
    numFollowers
  }
}
```

The `noUsers` query returns no users, but it should return all users with `numFollowers` of `-10` except the one we provided.
