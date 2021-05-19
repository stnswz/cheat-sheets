## GraphQL Queries and Mutations
[https://graphql.org/learn/queries/](https://graphql.org/learn/queries/)  
&nbsp;  
GitHub's GraphQL API and Tutorial:  
[docs.github.com/en/graphql/overview/explorer](https://docs.github.com/en/graphql/overview/explorer)  
https://www.robinwieruch.de/graphql-tutorial/  
### **Fields**  
[graphql.org/learn/queries/#fields](https://graphql.org/learn/queries/#fields)  
The 'hero' is an object in GraphQL terms. Objects hold data about an entity. This data is accessed using a so-called field in GraphQL. In its most basic form, a query is just objects and fields, and objects can also be called fields. In the example, there are 2 fields: 'name' and 'friends', where 'friends' is also an object.

```graphql
{
  hero {
    name
    # Queries can have comments!
    friends {
      name
    }
  }
}
```
Result:
```json
{
  "data": {
    "hero": {
      "name": "R2-D2",
      "friends": [
        {
          "name": "Luke Skywalker"
        },
        {
          "name": "Han Solo"
        }
      ]
    }
  }
}
```
GraphQL queries look the same for both single items or lists of items, however we know which one to expect based on what is indicated in the schema.  
### **Arguments**  
[graphql.org/learn/queries/#arguments](https://graphql.org/learn/queries/#arguments)  
You can add arguments to various fields using GraphQL. It grants a great deal of flexibility for structuring queries, because you can make specifications to requests on a field level.  
Arguments can be of many different types. In the example, we use a string (for id) and an Enumeration type (for unit), which represents one of a finite set of options (in this case, units of length, either METER or FOOT). GraphQL comes with a default set of types, but a GraphQL server can also declare its own custom types, as long as they can be serialized into your transport format.
```graphql
{
  human(id: "1000") {
    name
    height(unit: FOOT)
  }
}
```
Result
```json
{
  "data": {
    "human": {
      "name": "Luke Skywalker",
      "height": 5.6430448
    }
  }
}
```
### **Aliases**  
[graphql.org/learn/queries/#aliases](https://graphql.org/learn/queries/#aliases)  
Aliases let you rename the result of a field to anything you want. In the example, the two hero fields would have conflicted, but since we can alias them to different names, we can get both results in one request.  

```graphql
{
  empireHero: hero(episode: EMPIRE) {
    name
  }
  jediHero: hero(episode: JEDI) {
    name
  }
}
```
Result
```json
{
  "data": {
    "empireHero": {
      "name": "Luke Skywalker"
    },
    "jediHero": {
      "name": "R2-D2"
    }
  }
}
```
### **Fragments**  
[graphql.org/learn/queries/#fragments](https://graphql.org/learn/queries/#fragments)  
GraphQL includes reusable units called **fragments**. Fragments let you construct sets of fields, and then include them in queries where you need to. You have to specify on which **type** of object the fragment should be used. In the example, it is the type 'Character' related to the Object 'hero', which is a defined custom type.  
```graphql
{
  leftComparison: hero(episode: EMPIRE) {
    ...comparisonFields
  }
  rightComparison: hero(episode: JEDI) {
    ...comparisonFields
  }
}

fragment comparisonFields on Character {
  name
  friends {
    name
  }
}
```
Result
```json
{
  "data": {
    "leftComparison": {
      "name": "Luke Skywalker",
      "friends": [
        {
          "name": "Han Solo"
        },
        {
          "name": "R2-D2"
        }
      ]
    },
    "rightComparison": {
      "name": "R2-D2",
      "friends": [
        {
          "name": "Han Solo"
        },
        {
          "name": "Leia Organa"
        }
      ]
    }
  }
}
```
### **Operation name**  
[graphql.org/learn/queries/#operation-name](https://graphql.org/learn/queries/#operation-name)  
The query statement is also called **operation type** in GraphQL lingua. For instance, it can also be a mutation statement. In addition to the operation type, you can also define an **operation name** (here 'HeroNameAndFriends' in the example).  
Compare it to anonymous and named functions in your code. A **named query** provides a certain level of clarity about what you want to achieve with the query in a declarative way, and it helps with debugging multiple queries, so it should be used when you want to implement an application.  
```graphql
query HeroNameAndFriends {
  hero {
    name
    friends {
      name
    }
  }
}
```
Result
```json
{
  "data": {
    "hero": {
      "name": "R2-D2",
      "friends": [
        {
          "name": "Han Solo"
        },
        {
          "name": "Leia Organa"
        }
      ]
    }
  }
}
```
### **Variables**  
[graphql.org/learn/queries/#variables](https://graphql.org/learn/queries/#variables)  
Essentially, variables can be used to create dynamic queries. Variables allows **arguments** to be extracted as variables from queries. In the example, it defines the 'episode' argument as a variable using the $ sign. Also, the argument's type is defined as a String. Since the argument is required to fulfil the query, the String type has an exclamation point.  

When we start working with variables, we need to do three things:  
1. Replace the static value in the query with **$variableName**
2. Declare **$variableName** as one of the variables accepted by the query
3. Pass **variableName: value** in the separate, transport-specific (usually JSON) variables dictionary
```graphql
query HeroNameAndFriends($episode: String!) {
  hero(episode: $episode) {
    name
    friends {
      name
    }
  }
}
----------
 VARIBLES
----------
{
  "episode": "JEDI"
}
```
Result
```json
{
  "data": {
    "hero": {
      "name": "R2-D2",
      "friends": [
        {
          "name": "Han Solo"
        },
        {
          "name": "Leia Organa"
        }
      ]
    }
  }
}
```
Default values can also be assigned to the variables in the query by adding the default value after the type declaration.  When using default values, it has to be a non-required argument, or an error will occur about a **nullable variable** or **non-null variable**. 
```graphql
query HeroNameAndFriends($episode: String = "JEDI") {
  hero(episode: $episode) {
    name
    friends {
      name
    }
  }
}
```
### **Directives**  
[graphql.org/learn/queries/#directives](https://graphql.org/learn/queries/#directives)  
The core GraphQL specification includes exactly two directives, which must be supported by any spec-compliant GraphQL server implementation:  
* **@include(if: Boolean)** Only include this field in the result if the argument is true.
* **@skip(if: Boolean)** Skip this field if the argument is true.
```graphql
query Hero($episode: String, $withFriends: Boolean!) {
  hero(episode: $episode) {
    name
    friends @include(if: $withFriends) {
      name
    }
  }
}
----------
 VARIBLES
----------
{
  "episode": "JEDI",
  "withFriends": false
}
```
Result
```json
{
  "data": {
    "hero": {
      "name": "R2-D2"
    }
  }
}
```
