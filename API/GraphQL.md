# 🫧 GraphQL

> GraphQL은 REST API에 대한 직접적인 해결책으로 마케팅되고 있다.

## Apollo server

> GraphQL을 이해하는 server

## Query Type

> server의 동작을 위한 필수적인 type  
>  like REST API의 GET

```javascript
const typeDefs = gql`
  type Query {
    title: String
  }
`;
```

## Scalar Type

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

## Non Scalar Type

> Scalar Type 만으로 표현할 수 없는 type의 경우 직접 custom할 수 있다.

```javascript
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

## Mutation Type

> server의 state를 mutate하는 경우 `Mutation` type에 지정해줘야 한다.  
> like REST API의 POST, PUT, DELETE

```javascript
const typeDefs = gql`
  type Movie {
    id: ID
    name: String
  }
  type Query {
    movie(id: ID): Movie
  }
  type Mutation {
    postMovie(id: ID, name: String): Movie
  }
`;
```

## Non Nullable

> type이 null이 될 수 없음을 !(절대적)을 활용해 표현할 수 있다.

```javascript
const typeDefs = gql`
  type Movie {
    id: ID!
    name: String!
  }
  type Query {
    movie(id: ID!): Movie!
  }
  type Mutation {
    postMovie(id: ID!, name: String!): Movie!
  }
`;
```

## Resolver

## Documentation

> GraphQL에는 문서화 기능(like swagger)도 포함되어 있어 `"""`를 활용해 API나 데이터에 대한 상세설명을 작성할 수 있다.

## Migrating REST to GraphQL
