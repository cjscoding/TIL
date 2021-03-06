# ๐ซง GraphQL

> GraphQL์ REST API์ ๋ํ ์ง์ ์ ์ธ ํด๊ฒฐ์ฑ์ผ๋ก ๋ง์ผํ๋๊ณ  ์๋ค.

---

## Apollo server

> GraphQL์ ์ดํดํ๋ server

```javascript
import { ApolloServer, gql } from "apollo-server";

/*

types & resolvers

*/

const server = new ApolloServer({ typeDefs, resolvers });

server.listen().then(({ url }) => {
  console.log(`Running on ${url}`);
});
```

---

## Type

> GraphQL์ schema๋ฅผ ์ ์ํ๋ ๊ฒ

### Query Type

> server์ ๋์์ ์ํ ํ์์ ์ธ type  
>  like REST API์ GET

```javascript
const typeDefs = gql`
  type Query {
    title: String
  }
`;
```

### Scalar Type

> GraphQL์ built-in(๋ด์ฅ)๋์ด์๋ type.  
> String, Int, Boolean, ID ๋ฑ์ type์ด ์์

```javascript
const typeDefs = gql`
  type Query {
    id: ID
    title: String
    desc: String
  }
`;
```

### Non Scalar Type

> Scalar Type ๋ง์ผ๋ก ํํํ  ์ ์๋ type์ ๊ฒฝ์ฐ ์ง์  customํ  ์ ์๋ค.

```javascript
const movies = [];

const typeDefs = gql`
  type Movie {
    id: ID
    name: String
  }
  type Query {
    movie(id: ID): Movie
  }
`;
```

### Mutation Type

> server์ state๋ฅผ mutateํ๋ ๊ฒฝ์ฐ `Mutation` type์ ์ง์ ํด์ค์ผ ํ๋ค.  
> like REST API์ POST, PUT, DELETE

```javascript
const movies = [];

const typeDefs = gql`
  type Movie {
    id: ID
    title: String
  }
  type Query {
    movie(id: ID): Movie
  }
  type Mutation {
    postMovie(id: ID, title: String): Movie
  }
`;
```

### Non Nullable

> type์ด null์ด ๋  ์ ์์์ !(์ ๋์ )์ ํ์ฉํด ํํํ  ์ ์๋ค.

```javascript
const movies = [];

const typeDefs = gql`
  type Movie {
    id: ID!
    title: String!
  }
  type Query {
    movie(id: ID!): Movie!
  }
  type Mutation {
    postMovie(id: ID!, title: String!): Movie!
  }
`;
```

---

## Resolver

> ๊ฐ์ฒด ํ์์ผ๋ก schema์ data๋ฅผ ๊ฐ์ง๊ณ  ๋์ํ๋ ๋ฐฉ์์ ์ ํ๋ค. (like SQL)  
> ๊ฐ์ฒด๋ณ `key๋ type๋ช๊ณผ ๊ฐ์`์ผ ํ๋ค.

> ๊ฐ resolver ๋ด `state๋ฅผ ๋ณ๊ฒฝํ๋ (POST/PUT/DELETE์ ๊ฐ์ ๊ธฐ๋ฅ์ ํ๋) ํจ์๋ค์ ๋๊ฐ์ง์ argument`๋ฅผ ๊ฐ์ง๋ค.  
> ์ฒซ๋ฒ์งธ๋ root์ด๊ณ  ๋๋ฒ์งธ๋ user๊ฐ ํจ์ ํธ์ถ ์ ์๋ ฅํ๋ argument์ด๋ค.  
> `user์ ์๋ ฅ๊ฐ์ ๋ฐ๋์ ํธ์ถ๋ ํจ์์ ๋๋ฒ์งธ ์ธ์`๋ก ๋ค์ด๊ฐ์ผ ํ๊ธฐ ๋๋ฌธ์ root ๋์  argument๋ฅผ ๋ฌด์ํ๋ค๋ ์๋ฏธ๋ก `_` ๋๋ `__`๋ฅผ ์ฐ๊ธฐ๋ ํ๋ค.

### Query Resolver

> Query type์ ๋ํ resolver

```javascript
const resolvers = {
  Query: {
    allMovies() {
      return movies;
    },
    movie(root, { id }) {
      return movies.find((movie) => movie.id === id);
    },
  },
};
```

> ์๋ฅผ ๋ค์ด ์์ movie ์ ๋ณด๋ฅผ GETํ๊ธฐ ์ํด ์๋์ ๊ฐ์ด ํจ์๋ฅผ ํธ์ถํ๋ค.

```
movie(id: 1){
    title
}
```

### Mutation Resolver

> Mutation type์ ๋ํ resolver

```javascript
const resolvers = {
  Query: {
    allMovies() {
      return movies;
    },
    movie(root, { id }) {
      return movies.find((movie) => movie.id === id);
    },
  },
  Mutation: {
    postMovie(_, { title }) {
      const newMovie = {
        id: movies.length + 1,
        title,
      };
      movies.push(newMovie);
      return newMovie;
    },
  },
};
```

state๊ฐ ๋ณ๊ฒฝ์ ์ํ action์ด๋ผ ํ  ์ ์๋ค.

### Type Resolver

> ์ด๋ค type ๋ด๋ถ์ ์ด๋ค field๋ผ๋ resolver function์ ๋ง๋ค ์ ์๋ค.

```javascript
let users = [
  {
    id: "1",
    firstName: "sue",
  },
  {
    id: "2",
    firstName: "jisu",
    lastName: "choi",
  },
];

const typeDefs = gql`
  type User {
    id: ID!
    firstName: String!
    lastName: String
    fullName: String!
  }
`;

const resolvers = {
  User: {
    fullName({ firstName, lastName }) {
      return `${firstName} ${lastName}`;
    },
  },
};
```

---

## Documentation

> GraphQL์๋ ๋ฌธ์ํ ๊ธฐ๋ฅ(like swagger)๋ ํฌํจ๋์ด ์์ด `"""`๋ฅผ ํ์ฉํด API๋ ๋ฐ์ดํฐ์ ๋ํ ์์ธ์ค๋ช์ ์์ฑํ  ์ ์๋ค.

## Migrating REST to GraphQL

> REST API ์ฒ๋ผ url๋ก ์ป์ ๋ฐ์ดํฐ๋ฅผ GraphQL ๋ฐฉ์์ผ๋ก ํ์ฉํ  ์ ์๋ค.

```javascript
import fetch from "node-fetch";

const typeDefs = gql`
  type Query {
    allMovies: [Movie!]!
    allUsers: [User!]!
    allTweets: [Tweet!]!
    tweet(id: ID!): Tweet
    movie(id: String!): Movie
  }

  type Movie {
    id: Int!
    url: String!
    imdb_code: String!
    title: String!
    title_english: String!
    title_long: String!
    slug: String!
    year: Int!
    rating: Float!
    runtime: Float!
    genres: [String]!
    summary: String
    description_full: String!
    synopsis: String
    yt_trailer_code: String!
    language: String!
    background_image: String!
    background_image_original: String!
    small_cover_image: String!
    medium_cover_image: String!
    large_cover_image: String!
  }
`;

const resolvers = {
  Query: {
    allMovies() {
      return fetch("https://yts.mx/api/v2/list_movies.json")
        .then((r) => r.json())
        .then((json) => json.data.movies);
    },
    movie(_, { id }) {
      return fetch(`https://yts.mx/api/v2/movie_details.json?movie_id=${id}`)
        .then((r) => r.json())
        .then((json) => json.data.movie);
    },
  },
};
```

```
//ํธ์ถ ์

allMovies(){
    title
    genres
    runtime
    summary
}

movie(id: 1){
    title
    summary
}
```

[์ฐธ๊ณ  ๊ฐ์](https://nomadcoders.co/graphql-for-beginners/lectures/3700)
