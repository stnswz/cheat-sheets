## Using Apollo with React
Getting startet:  
[apollographql.com/docs/react/get-started/](https://www.apollographql.com/docs/react/get-started/)  
&nbsp;  
### **Initialising**  
```javascript
import ReactDOM from 'react-dom'
import { ApolloClient, InMemoryCache } from '@apollo/client'
import { ApolloProvider } from '@apollo/client/react'

// Create a apollo client.
const client = new ApolloClient({
  uri: 'https://48p1r2roz4.sse.codesandbox.io',
  cache: new InMemoryCache()
});

// Use ApolloProvider as composing component around your App component. Under the hood,
// it uses React's Context API to pass the Apollo Client through your application.
ReactDOM.render(
  <ApolloProvider client={client}>
    <App />
  </ApolloProvider>,
  document.getElementById('root')
);
```

**Customizing request headers**  
[apollographql.com/docs/react/networking/basic-http-networking/](https://www.apollographql.com/docs/react/networking/basic-http-networking/)  
You can specify the names and values of custom headers to include in every HTTP request to a GraphQL server. To do so, provide the headers parameter to the ApolloClient constructor.  
```javascript
import { ApolloClient, InMemoryCache } from '@apollo/client';

const client = new ApolloClient({
  uri: 'https://api.example.com',
  cache: new InMemoryCache(),
  headers: {
    authorization: localStorage.getItem('token'),
    'client-name': 'WidgetX Ecom [web]',
    'client-version': '1.0.0'
  }
});
```
Or configuring it using **HttpLink**   
```javascript
import { ApolloClient, HttpLink, InMemoryCache } from '@apollo/client';

const GITHUB_BASE_URL = 'https://api.github.com/graphql';
 
const httpLink = new HttpLink({
  uri: GITHUB_BASE_URL,
  headers: {
    authorization: 'bearer YOUR_GITHUB_PERSONAL_ACCESS_TOKEN',
  },
});

const cache = new InMemoryCache();

const client = new ApolloClient({
  link: httpLink,
  cache
});
```


### **useQuery Hook examples**  
[apollographql.com/docs/react/data/queries/](https://www.apollographql.com/docs/react/data/queries/)  
Detailed **useQuery** hook reference:  
[apollographql.com/docs/react/api/react/hooks/#usequery](https://www.apollographql.com/docs/react/api/react/hooks/#usequery)
```javascript
import { gql, useQuery } from '@apollo/client'

const GET_DOGS = gql`
  query GetDogs {
    dogs {
      id
      breed
    }
  }
`;

function Dogs({ onDogSelected }) {
  const { loading, error, data } = useQuery(GET_DOGS)

  if (loading) return 'Loading...'
  if (error) return `Error! ${error.message}`

  return (
    <select name="dog" onChange={onDogSelected}>
      {data.dogs.map(dog => (
        <option key={dog.id} value={dog.breed}>
          {dog.breed}
        </option>
      ))}
    </select>
  );
}
```

**Updating (cached) query results and loading-states**  
[apollographql.com/docs/react/data/queries/#updating-cached-query-results](https://www.apollographql.com/docs/react/data/queries/#updating-cached-query-results)  
Apollo Client supports two strategies to make sure that cached data is up to date with your server: **polling** and **refetching**. Also, the useQuery hook's result object provides fine-grained information about the status of the query via the **networkStatus** property.

```javascript
import { gql, useQuery, NetworkStatus } from '@apollo/client';

const GET_DOG_PHOTO = gql`
  query Dog($breed: String!) {
    dog(breed: $breed) {
      id
      displayImage
    }
  }
`;

function DogPhoto({ breed }) {
  const { loading, error, data, refetch, networkStatus } = useQuery(
    GET_DOG_PHOTO,
    {
      variables: { breed },
      // Important: set the notifyOnNetworkStatusChange option to true, to get status information via the networkStatus property.
      notifyOnNetworkStatusChange: true
    },
  );

  if (networkStatus === NetworkStatus.refetch) return 'Refetching!'
  else if (loading) 'Loading...'
  if (error) return `Error! ${error.message}`

  return (
    <div>
      <img src={data.dog.displayImage} style={{ height: 100, width: 100 }} />
      <button onClick={() => refetch()}>Refetch!</button>
    </div>
  );
}
```

### **Executing queries manually (using useLazyQuery)**
[apollographql.com/docs/react/data/queries/#executing-queries-manually](https://www.apollographql.com/docs/react/data/queries/#executing-queries-manually)  
Starts executing a query only in response to a different event, such as a user clicking a button. This hook acts just like **useQuery**, with one key exception: when **useLazyQuery** is called, it does not immediately execute its associated query. Instead, it returns a function in its result tuple that you can call whenever you're ready to execute the query:  

```javascript
import React from 'react';
import { useLazyQuery } from '@apollo/client';

const GET_DOG_PHOTO = gql`
  query Dog($breed: String!) {
    dog(breed: $breed) {
      id
      displayImage
    }
  }
`;

function DelayedQuery() {
  const [startLoading, { loading, data }] = useLazyQuery(GET_DOG_PHOTO);

  if (loading) return <p>Loading ...</p>;

  return (
    <div>
      {data && data.dog && <img src={data.dog.displayImage} />}
      <button onClick={() => startLoading({ variables: { breed: 'bulldog' } })}>
        Click me!
      </button>
    </div>
  );
}
```

### **Setting a fetch policy (for caching adjustment)**  
[apollographql.com/docs/react/data/queries/#setting-a-fetch-policy](https://www.apollographql.com/docs/react/data/queries/#setting-a-fetch-policy)  
The **cache-first** policy is Apollo Client's default fetch policy. Means, if all data is already available locally, useQuery returns that data and doesn't query your GraphQL server.  You can optionally specify a different fetch policy for a given query. To do so, include the fetchPolicy option in your call to useQuery:

```javascript
const { loading, error, data } = useQuery(GET_DOGS, {
  fetchPolicy: "network-only"
});
```
List with all supported fetch policies: 
[apollographql.com/docs/react/data/queries/#supported-fetch-policies](https://www.apollographql.com/docs/react/data/queries/#supported-fetch-policies)  

### **TypeScript**  
[apollographql.com/docs/react/development-testing/static-typing/](https://www.apollographql.com/docs/react/development-testing/static-typing/)  
Apollo Client's **useQuery**, **useMutation** and **useSubscription** React hooks are fully typed. React Hook options and result types are listed in the [Hooks API](https://www.apollographql.com/docs/react/api/react/hooks/) section of the docs.  

**typed useQuery Example**  
```javascript
import React from 'react'
import { useQuery, gql } from '@apollo/client'

interface RocketInventory {
  id: number,
  model: string,
}

interface RocketInventoryData {
  rocketInventory: RocketInventory[]
}

interface RocketInventoryVars {
  year: number
}

const GET_ROCKET_INVENTORY = gql`
  query GetRocketInventory($year: Int!) {
    rocketInventory(year: $year) {
      id
      model
    }
  }
`;

export function RocketInventoryList() {
  const { loading, data } = useQuery<RocketInventoryData, RocketInventoryVars>(
    GET_ROCKET_INVENTORY,
    { variables: { year: 2019 } }
  );
  return (
    {loading && <p>Loading...</p>}
    <ul>
      {data && data.rocketInventory.map(inventory => (
        <li>
          Model: {inventory.model}, Id: {inventory.id} 
        </li>
      ))}
      )}
    </ul>
  );
}
```