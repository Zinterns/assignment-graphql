# API 명세

## Basic Schemas
```graphql
type Mutation {
    login(email: String!, password: String!, name: String!): AuthPayload
}

type AuthPayload {
    token: String
    user: User
}
```


## 로그인 기능
```graphql
REQUEST
mutation {
    login(
        email: "zigzag@zigza.com"
        password: "zigzagda"
        name: "kimzigzag"
    ) {
        token
        user {
            id
            name
        }
    }
}

RESPONSE 
{
    "data": {
        "login": {
            "token" : "asdkjflsdhflasdjlkfjasdlkfjkjwrpjdlkfjsadlknvlj"
            "user" : {
                id: "1"
                name: "kimzigzag"
            }

        }
    }
}
```

