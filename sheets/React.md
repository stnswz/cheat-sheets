## Cheat Sheet
[https://devhints.io/react](https://devhints.io/react)  
  
## Typed Props for function components  
See also [Different ways to strongly type function component props with typescript](https://www.carlrippon.com/Different-ways-to-strongly-type-function-component-props-with-typescript/)
```javascript
interface ITodoRowProps {
  item: ITodoItem,
  onDeleteClick: (id: string) => void;
}

function TodoRow( {item, onDeleteClick}: ITodoRowProps ) {
}  

<TodoRow key={item.id} {...TodoRowProps} />  
// Or optionaly  
<TodoRow key={item.id} item={item} onDeleteClick={onDeleteClick} />
```  

----

## Input Element & Events  
```javascript
import React, { MouseEvent, KeyboardEvent, SyntheticEvent } from 'react';

// Button
const onButtonClick = (event: MouseEvent) => {
  event.preventDefault();
  alert(event.currentTarget.tagName); // alerts BUTTON
}
<button onClick={onButtonClick}>Click Here</button>

// Text Input
const [inputText, setInputText] = useState('')
const onKeyPress = (ev:KeyboardEvent) => {
  if( ev.key === "Enter") {
      // do any action
  }
}
<input type="text" value={inputText} onChange={event => setInputText(event.target.value)} onKeyPress={onKeyPress} />

// CheckBox 
const [checked, setChecked] = useState(item.done)
const checkBoxChange = (event: any) => {
  event.preventDefault();
  setChecked(event.target.checked)
}
<input type="checkbox" name={item.text} checked={checked} onChange={checkBoxChange} />

// Radio
const [selectedFilter, setSelectedFilter] = useState('1');
const onOptionChange = (event:any) => {
  setSelectedFilter(event.target.id)
}
<label><input type="radio" id="1" name="filter" value="No Filter" checked={selectedFilter === '1'} onChange={onOptionChange} /> No Filter</label>
<label><input type="radio" id="2" name="filter" value="Filter" checked={selectedFilter === '2'} onChange={onOptionChange} /> Filter</label>
```  

----

## Event Types  
[fettblog.eu/typescript-react/events/](https://fettblog.eu/typescript-react/events/)  

----

## Conditional rendering
See also: [robinwieruch.de/conditional-rendering-react](https://www.robinwieruch.de/conditional-rendering-react)
```javascript
return (
  <>
    {isLoading && <div>Search for : {searchText}</div>}
    {loadingError && <div>loadingError: {loadingError}</div>}
  </>
);
```  

----

## Axios  
See also: [blog.logrocket.com/how-to-make-http-requests-like-a-pro-with-axios/](https://blog.logrocket.com/how-to-make-http-requests-like-a-pro-with-axios/)
```javascript
import axios, {AxiosResponse} from 'axios'

await axios.get(url, {params: {query: searchText}})
.then ((response: AxiosResponse<Record<string, unknown>>) => {
  setResponseData(response.data)
})
.catch((error) => {
  setIsError(true)
})
``` 

----

## Children
See also:  
[fettblog.eu/typescript-react/children/](https://fettblog.eu/typescript-react/children/)  
[reactjs.de/artikel/react-children/](https://reactjs.de/artikel/react-children/)
```javascript
import React, { FunctionComponent } from 'react';

// Because of "FunctionComponent" no children neccesary to define here
type ComponentProps = {
  title: string,
  paragraph: string
}

const MyComponent: FunctionComponent<ComponentProps> = (props) => {
  return (
    <>
      <h2>{ props.title }</h2>
      <p>
        { props.paragraph }
      </p>
      { /* still in our props */ }
      { props.children }
    </>
  );
}
```  

----

## Hooks   
[reactjs.org/docs/hooks-reference.html](https://reactjs.org/docs/hooks-reference.html)  
[fettblog.eu/typescript-react/hooks/](https://fettblog.eu/typescript-react/hooks/)  

----

## Redux Hooks
```javascript
import { useDispatch, useSelector } from "react-redux";

const isLoading: boolean = useSelector( (state: IRootState) => state.newsState.isLoading);
const dispatch: Function = useDispatch()

// Loads immediately after mounting or when inputText changes
useEffect(() => {
  if(inputText) dispatch(loadNews(inputText));

// eslint-disable-next-line 
}, [inputText]);
```  

----

## useContext / useReducer
[reactjs.org/docs/hooks-reference.html#usecontext](https://reactjs.org/docs/hooks-reference.html#usecontext)  
[reactjs.org/docs/hooks-faq.html#how-to-avoid-passing-callbacks-down](https://reactjs.org/docs/hooks-faq.html#how-to-avoid-passing-callbacks-down)  

```javascript
// Reducer
export interface ICountState {
  count: number
}
export interface IReducerAction {
  type: string,
  payload?: any
}

export const INCREMENT = 'INCREMENT'
export const DECREMENT = 'DECREMENT'

function reducer(state: ICountState, action: IReducerAction): ICountState {
  switch (action.type) {
    case INCREMENT:
      return {count: state.count + 1};
    case DECREMENT:
      return {count: state.count - 1};
    default:
      throw new Error();
  }
}

export default reducer
```

```javascript
// Main App
import {createContext, useReducer} from 'react'
import reducer, {ICountState, IReducerAction} from './reducer/CountReducer'
import ShowStateCompo from './ShowStateCompo'
import IncrementCompo from './IncrementCompo'

const initialState: ICountState = {count: 0};

export const DispatchContext = createContext((a:IReducerAction) => {});
// export const StateContext = createContext(initialState);

function ContextReducerApp() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <DispatchContext.Provider value={dispatch}>
      <ShowStateCompo {...state} />
      <IncrementCompo />
    </DispatchContext.Provider>
  )
}

export default ContextReducerApp
```  

```javascript
// State Component
import {ICountState} from './reducer/CountReducer'

function ShowStateCompo(state: ICountState) {
  return (
    <div>
      Count is: {state.count}
    </div>
  )
}

export default ShowStateCompo
``` 

```javascript
// Dispatch Component
import {useContext} from 'react'
import {DispatchContext} from './ContextReducerApp'
import {INCREMENT} from './reducer/CountReducer'

function IncrementCompo() {
  const dispatch = useContext(DispatchContext);

  return (
    <button onClick={() => dispatch({type: INCREMENT})}>
      Incremnt
    </button>
  )
}

export default IncrementCompo
```

```javascript

```

----

## Binding 
**Bind Value to an Event**
```javascript
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```  
**Bind Value to a function**
```javascript
setTimeout(setSearchTextDelayed.bind(null, anyValue), 500)
```  

----

## Using Environment variables (.env file)
Environment variables are defined in a ".env" file. Be sure to follow the correct naming constraints. When using create-react-app, it uses REACT_APP as prefix for each key. In the .env file, paste the following key value pair. The key has to have the REACT_APP prefix, and (in the example) the value has to be an access token.
```javascript
REACT_APP_ACCESS_TOKEN=abcdefg12345678hijklmnop
``` 
Now, you can pass the personal access token as environment variable to your configuration like this (example uses an axios initial setup):
```javascript
const axiosGitHubGraphQL = axios.create({
  baseURL: 'https://api.github.com/graphql',
  headers: {
    // will look like "Authorization: 'bearer abcdefg12345678hijklmnop'"
    Authorization: "'bearer " + process.env.REACT_APP_ACCESS_TOKEN + "'"
  },
});
``` 