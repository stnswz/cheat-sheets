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

<TodoRow key={item.id} ...TodoRowProps />  
// Or optionaly  
<TodoRow key={item.id} item={item} onDeleteClick={onDeleteClick} />
```  
  
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

## Event Types  
[fettblog.eu/typescript-react/events/](https://fettblog.eu/typescript-react/events/)  
  
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
  
## Hooks  
See also:  
[fettblog.eu/typescript-react/hooks/](https://fettblog.eu/typescript-react/hooks/)  
[reactjs.org/docs/hooks-reference.html](https://reactjs.org/docs/hooks-reference.html)  
  
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