# API 명세

## Basic Schemas
```graphql
type Query {
    user_list: [User!]!
    document_list: [Document!]!
    inbox: [Document]!
}

input LoginInput {
    email: String!
    password: String!
    name: String!
}

input WriteDocumentInput {
    title: String!
    contents: String!
    category: Category!
    signers: [String!]!
}

input SignDocumentInput {
    documentId: ID!
    status: Status!
    opinion: String
}

type Mutation {
    login(input: LoginInput): AuthPayload
    writeDocument(input: WriteDocumentInput): Document
    signDocument(input: SignDocumentInput): Sign
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
    status: Status!
    opinion: String
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
            "token" : "asdkjflsdhflasdjlkfjasdlkfjkjwrpjdlkfjsadlknvlj",
            "user" : {
                id: "1",
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
            "id": "asehfsjfh1",
            "title": "새로운 패션 디자인 결재 부탁드립니다",
            "contents": "일본 시장을 겨냥하여 새로운 패션몰들을 리스트업해봤습니다...",
            "category": NORMAL // or "NORMAL?",
            "signers": {
                "id": "saeifjsdlfkj123",
                "name": "kimzigzag",
                "email": "kimzigzag@croquis.com",
            },
        }
    }
}
```

## 유저 리스트 api
```graphql
REQUEST
query {
    user_list {
            id
            name
            email
        }
    }
}

RESPONSE
{
    "data": {
        "user_list": 
            [
                {
                    "id": "asehfsjfh1",
                    "name": "kimzigzag",
                    "email": "kimzigzag@croquis.com",
                },
                {
                    "id": "asehfsjfh1",
                    "name": "leezigzag",
                    "email": "leezigzag@croquis.com",
                },
                {
                    "id": "asehfsjfh1",
                    "name": "onzigzag",
                    "email": "onzigzag@croquis.com",
                },
            ]
    }
}
```

```graphql
REQUEST
query{
    document_list {
        id
        title
        contents
        author {
            id
            name
            email
        }
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
        "document_list": 
        [
            {
                "id": "aslfhdsljfksjkfajks",
                "title": "새로운 패션 디자인 결재 부탁드립니다",
                "contents": "일본 시장을 겨냥하여 새로운 패션몰들을 리스트업해봤습니다...",
                "author" : 
                {
                    "id" : "ajsdlkfjsdlkfajdsl",
                    "name": "kimzigzag",
                    "email": "kimzigzag@croquis.com",
                }
                "signers": 
                [
                    {
                        "id" : "ajsdlkfjsdlkfajdsl",
                        "name": "kimzigzag",
                        "email": "kimzigzag@croquis.com",
                    },
                    {
                        "id" : "ajsdlkfjsdlkfajdsl",
                        "name": "kimzigzag",
                        "email": "kimzigzag@croquis.com",
                    }
                ]
            }
        ]
    }
}
```

## 문서 결재 api
```graphql
REQUEST

mutation {
    signDocument(
        documentId: 1
        status: APPROVED
        opinion: "좋은 제안입니다!"
    ) {
        id
        status
        opinion
        document {
            title
            contents
            author {
                id
                name
                email
            }
        }
        signer {
            id
            name
            email
        }
    }
}

RESPONSE
{
    "data": {
        "signDocument": {
            "id": "sdlfdlsajflkdjslkfj",
            "status": "APPROVED",
            "opinion": "좋은 제안입니다",
            "document" {
                "title": "새로운 패션 디자인 결재 부탁드립니다",
                "contents": "일본 시장을 겨냥하여 새로운 패션몰들을 리스트업해봤습니다...",
                "author": {
                    "id" : "ajsdlkfjsdlkfajdsl",
                    "name": "kimzigzag",
                    "email": "kimzigzag@croquis.com",
                }
            }
            "signer": {
                "id" : "ajsdlkfjsdlkfajdsl",
                "name": "onzigzag",
                "email": "onzigzag@croquis.com",
            }
        }
    }
}

```

## INBOX API
```graphql
REQUEST
query{
    inbox {
        id
        title
        contents
        category
        author {
            name
            email
        }
    }
}

RESPONSE
{
    "data": {
        "inbox": 
        [
            {
                "id": "sdlfdlsajflkdjslkfj",
                "title": "새로운 패션 디자인 결재 부탁드립니다",
                "contents": "일본 시장을 겨냥하여 새로운 패션몰들을 리스트업해봤습니다...",
                "author": {
                    "name": "onzigzag",
                    "email": "onzigzag@croquis.com",
            },
            {
                "id": "sdlfdlsajflkdjslkfj",
                "title": "새로운 패션 디자인 결재 부탁드립니다",
                "contents": "일본 시장을 겨냥하여 새로운 패션몰들을 리스트업해봤습니다...",
                "author": {
                    "name": "onzigzag",
                    "email": "onzigzag@croquis.com",
            },
        }
    }
}

```

## ARCHIVE API
```graphql
REQUEST
query{
    archive {
        id
        title
        contents
        category
        author {
            name
            email
        }
    }
}

RESPONSE
{
    "data": {
        "archive": 
        [
            {
                "id": "sdlfdlsajflkdjslkfj",
                "title": "새로운 패션 디자인 결재 부탁드립니다",
                "contents": "일본 시장을 겨냥하여 새로운 패션몰들을 리스트업해봤습니다...",
                "author": {
                    "name": "onzigzag",
                    "email": "onzigzag@croquis.com",
            },
            {
                "id": "sdlfdlsajflkdjslkfj",
                "title": "새로운 패션 디자인 결재 부탁드립니다",
                "contents": "일본 시장을 겨냥하여 새로운 패션몰들을 리스트업해봤습니다...",
                "author": {
                    "name": "onzigzag",
                    "email": "onzigzag@croquis.com",
            },
        }
    }
}

```

## OUTBOX API
```graphql
REQUEST
query{
    outbox {
        id
        title
        contents
        category
        author {
            name
            email
        }
    }
}

RESPONSE
{
    "data": {
        "outbox": 
        [
            {
                "id": "sdlfdlsajflkdjslkfj",
                "title": "새로운 패션 디자인 결재 부탁드립니다",
                "contents": "일본 시장을 겨냥하여 새로운 패션몰들을 리스트업해봤습니다...",
                "author": {
                    "name": "onzigzag",
                    "email": "onzigzag@croquis.com",
            },
            {
                "id": "sdlfdlsajflkdjslkfj",
                "title": "새로운 패션 디자인 결재 부탁드립니다",
                "contents": "일본 시장을 겨냥하여 새로운 패션몰들을 리스트업해봤습니다...",
                "author": {
                    "name": "onzigzag",
                    "email": "onzigzag@croquis.com",
            },
        }
    }
}

```