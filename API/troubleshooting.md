# ๐ฅ TroubleShooting

## 'Apollo Server requires either an existing schema, modules or typeDefs'

![ApolloServer](../imgs/apolloError.png)

Why?

> GraphQL์ด data์ shape (like schema) ์ ๋ฏธ๋ฆฌ ์๊ณ  ์์ด์ผ ํ๊ธฐ ๋๋ฌธ์ ๋ฐ์

So?

> data์ shape์ GraphQL์๊ฒ ์ค๋ชํด์ฃผ๋ฉด ๋๋ค.

```javascript
const typeDefs = gql`
  type sth {
    text: String
  }
`;
```

## 'Query root type must be provided.'

![QueryServer](../imgs/queryError.png)

Why?

> `Query` type์ ํ์์ ์ด๋ค. Query type ์์ด๋ server๊ฐ ์์ํ์ง ์๋๋ค.

So?

```javascript
const typeDefs = gql`
  type Query {
    name: String
    age: Int
  }
`;
```

=> request ์์ฒญ์ ํ  ๋ชจ๋  ๊ฒ์  ๋ฐ.๋.์. `Query` type ๋ด์ ์์ด์ผ ํ๋ค.  
์๋ฅผ ๋ค์ด ์ ์ฝ๋์ ๊ฐ์ ๊ฒฝ์ฐ REST API์ `'GET /name'`, ` 'GET /age'`์ ๊ฐ์ด ๋ณผ ์ ์๋ค.
