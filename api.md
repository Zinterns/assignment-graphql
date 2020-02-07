# API 명세

## Basic Schemas
```graphql
type Mutation {
    login(email: String!, password: String!, name: String!): AuthPayload
    writedocument(title: String!, contetns: String!, category: Category!, signers: [String!]!): Document
}

type User {
    id: ID!
    email: String!
    name: String!
    documents: [Document!]!
    signs: [Sign!]!
}

type Document {
    id: ID!
    title: String!
    contetns: String!
    category: Category!
    author: User!
    signs: [Sign!]!
}

type Sign {
    id: ID!
    signer: User!
    document: Document!
    opinion: String!
}

type DocumentCreateConfirmation {
    code: String!
}

type AuthPayload {
    token: String
    user: User
}

enum Category {
    NORMAL
    SECRET
    CHORE
    EMPLYOYMENT
}
enum Status {
    APPROVED
    DENIED
    PROCESSING
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

## 문서 생성 api
```graphql
REQUEST
mutation {
    writedocument(
        title: "새로운 패션 디자인 결재 부탁드립니다"
        contents: "일본 시장을 겨냥하여 새로운 패션몰들을 리스트업해봤습니다..."
        category: NORMAL // or "NORMAL?"
        signers: ["kimzigzag", "leezigzag", "onzigzag"]
    ) {
        id
        title
        contents
        category
        author
        signers {
            id
            name
            email
        }
    }
}

RESPONSE
{
    "data": {
        "writedocument": {
            "id": "asehfsjfh1"
            "title": "새로운 패션 디자인 결재 부탁드립니다"
            "contents": "일본 시장을 겨냥하여 새로운 패션몰들을 리스트업해봤습니다..."
            "category": NORMAL // or "NORMAL?"
            "signers": {
                "id": "saeifjsdlfkj123"
                "name": "kimzigzag"
                "email": "kimzigzag@croquis.com"
            }
        }
    }
}
```