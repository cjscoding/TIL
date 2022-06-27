# 🫧 GraphQL

> GraphQL은 REST API에 대한 직접적인 해결책으로 마케팅되고 있다.

---

## Apollo server

> GraphQL을 이해하는 server

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

> GraphQL의 schema를 정의하는 것

### Query Type

> server의 동작을 위한 필수적인 type  
>  like REST API의 GET

```javascript
const typeDefs = gql`
  type Query {
    title: String
  }
`;
```

### Scalar Type

> GraphQL에 built-in(내장)되어있는 type.  
> String, Int, Boolean, ID 등의 type이 있음

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

> Scalar Type 만으로 표현할 수 없는 type의 경우 직접 custom할 수 있다.

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

> server의 state를 mutate하는 경우 `Mutation` type에 지정해줘야 한다.  
> like REST API의 POST, PUT, DELETE

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

> type이 null이 될 수 없음을 !(절대적)을 활용해 표현할 수 있다.

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

> 객체 형식으로 schema의 data를 가지고 동작하는 방식을 정한다. (like SQL)  
> 객체별 `key는 type명과 같아`야 한다.

> 각 resolver 내 `state를 변경하는 (POST/PUT/DELETE와 같은 기능을 하는) 함수들은 두가지의 argument`를 가진다.  
> 첫번째는 root이고 두번째는 user가 함수 호출 시 입력하는 argument이다.  
> `user의 입력값은 반드시 호출된 함수의 두번째 인자`로 들어가야 하기 때문에 root 대신 argument를 무시한다는 의미로 `_` 또는 `__`를 쓰기도 한다.

### Query Resolver

> Query type에 대한 resolver

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

> 예를 들어 위의 movie 정보를 GET하기 위해 아래와 같이 함수를 호출한다.

```
movie(id: 1){
    title
}
```

### Mutation Resolver

> Mutation type에 대한 resolver

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

state값 변경을 위한 action이라 할 수 있다.

### Type Resolver

> 어떤 type 내부의 어떤 field라도 resolver function을 만들 수 있다.

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

> GraphQL에는 문서화 기능(like swagger)도 포함되어 있어 `"""`를 활용해 API나 데이터에 대한 상세설명을 작성할 수 있다.

## Migrating REST to GraphQL

> REST API 처럼 url로 얻은 데이터를 GraphQL 방식으로 활용할 수 있다.

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
//호출 시

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

[참고 강의](https://nomadcoders.co/graphql-for-beginners/lectures/3700)
