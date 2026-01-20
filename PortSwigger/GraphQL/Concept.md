They said, that GraphQL is quering or something json format look like to CRUD on database, with addition some functional

### GraphQL Schema
```graphql
#Example schema definition

type Product { 
	id: ID! 
	name: 
	String! 
	description: 
	String! 
	price: Int 
}
```
symbol ! meaning that is required

### GraphQL Query
```graphql
#Example query

query myGetProductQuery { 
	getProduct(id: 123) {
		name 
		description 
	}
}
```
on example above, `myGetProductQuery` is name of query, getProduct() is the function or `data Sturcture` they called, and we can give one or more argument on that function

### GraphQL Mutations
```graphql
#Example mutation request

mutation { 
	createProduct(name: "Flamin' Cocktail Glasses", listed: "yes") { 
		id 
		name 
		listed 
	} 
}
```

```graphql
#Example mutation response

{ 
	"data": {
		"createProduct": { 
			"id": 123, 
			"name": "Flamin' Cocktail Glasses", 
			"listed": "yes" 
		} 
	} 
}
```

Mutation is CRUD things, which also have name, field, arguments

### Variables
```grahpql
#Example query with variable

query getEmployeeWithVariable($id: ID!) {
    getEmployees(id:$id) {
        name {
            firstname
            lastname
        }
    }
}

Variables:
{
    "id": 1
}

```

$id means, value that will be filled by variables

### Aliases
```graphql
#Valid query using aliases

query getProductDetails {
    product1: getProduct(id: "1") {
        id
        name
    }
    
    product2: getProduct(id: "2") {
        id
        name
    }
}
```

```graphql
#Response to query

{
	"data": {
		"product1": {
			"id": 1,
			"name": "Juice Extractor"
		},
		"product2": {
			"id": 2,
			"name": "Fruit Overlays"
		}
	}
}
```

Since on GraphQL can called same type, we can use aliases by give it name

### Fragment
Fragments are reusable parts of queries or mutations. They contain a subset of the fields belonging to the associated type.

Once defined, they can be included in queries or mutations. If they are subsequently changed, the change is included in every query or mutation that calls the fragment.

```graphql
#Example fragment

fragment productInfo on Product {
	id
	name
	lasted
}
```

```graphql
#Query calling the fragment

query {
	getProduct(id: 1) {
		...productInfo 
		stock
	}
}
```

```graphql
#Response including fragment fields

{
	"data": {
		"getProduct": {
			"id": 1,
			"name": "Juice Extractor",
			"listed": "no",
			"stock": 5
		}
	}
}
```

```graphql
query IntrospectionQuery { __schema { queryType { name } mutationType { name } subscriptionType { name } types { ...FullType } directives { name description args { ...InputValue } onOperation #Often needs to be deleted to run query onFragment #Often needs to be deleted to run query onField #Often needs to be deleted to run query } } } fragment FullType on __Type { kind name description fields(includeDeprecated: true) { name description args { ...InputValue } type { ...TypeRef } isDeprecated deprecationReason } inputFields { ...InputValue } interfaces { ...TypeRef } enumValues(includeDeprecated: true) { name description isDeprecated deprecationReason } possibleTypes { ...TypeRef } } fragment InputValue on __InputValue { name description type { ...TypeRef } defaultValue } fragment TypeRef on __Type { kind name ofType { kind name ofType { kind name ofType { kind name } } } }
```