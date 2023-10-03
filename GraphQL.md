# GraphQL Pentest Recap


###### GRAPHQL
- Mutation => modify data
- Query => get data
- Use introspection query to know what i can do
    - Use Inql Scanner with burp suite
    ```graphql
    __Schema
    types
     __type(<name>:"<User>")
     qyeryType
    ```
    ```graphql
    query Introspection{
        __Schema {
            types {
                name
            }
        }
    }


    ```

- Verify if introspection is available

```
apis.guru/graphql-voyager

# 1 - Go to introspection

#2 - Copy query

# 3 - past the result 
```

###### POSTMAN
```bash
postman >> import >> raw text >> <past>-Element>

put url into variable

config proxy
ssl off
```
