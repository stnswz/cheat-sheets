## Testing with Jest and React Test Library  
### **Links**  
* Jest Setup and Teardown  
  [Setup and Teardown](https://jestjs.io/docs/setup-teardown)  
* Jest Global functions **(beforeAll, beforeEach, afterAll, afterEach etc.)**  
  [Global Methods](https://jestjs.io/docs/api)
* Jest Matcher/Assertion Functions **(expect..., toBe...)**   
  [Using Matchers](https://jestjs.io/docs/using-matchers)  
  [Matcher/Assertion Methods](https://jestjs.io/docs/expect)  
  [Additional more specific Matchers by the Jest Community](https://github.com/jest-community/jest-extended)  
* Robin Wieruch: How to test React with Jest  
  [How to test React with Jest](https://www.robinwieruch.de/react-testing-jest)  

### **Using Matchers**  
[Using Matchers (Common / Truthiness / Numbers / Strings / Array's etc.)](https://jestjs.io/docs/using-matchers)  
```javascript
describe('Few matcher example tests', () => {
  test('two plus two is four', () => {
    expect(2 + 2).toBe(4)
  })

  test('object assignment', () => {
    const data = {one: 1}
    data['two'] = 2
    expect(data).toEqual({one: 1, two: 2})
  })

  test('test for null', () => {
    const n = null
    expect(n).toBeNull()
  })

  test('two plus two', () => {
    const value = 2 + 2
    expect(value).toBeGreaterThan(3)
    expect(value).toBeLessThan(5)
    expect(value).toBeGreaterThanOrEqual(3.5)
    expect(value).toBeLessThanOrEqual(4.5)

    // toBe and toEqual are equivalent for numbers
    expect(value).toBe(4)
    expect(value).toEqual(4)
  });

  test('there is no I in team', () => {
    expect('team').not.toMatch(/I/)
  });

  test('but there is a "stop" in Christoph', () => {
    expect('Christoph').toMatch(/stop/)
  })
})
``` 

### **Snapshot Testing**  
[jestjs.io/docs/snapshot-testing](https://jestjs.io/docs/snapshot-testing)  
A typical snapshot test case renders a UI component, takes a snapshot, then compares it to a reference snapshot file stored alongside the test. The test will fail if the two snapshots do not match: either the change is unexpected, or the reference snapshot needs to be updated to the new version of the UI component.  

```javascript
import renderer from 'react-test-renderer'
import CountrySelectBox from '../components/apollo/CountrySelectBox'

describe('Any Site Components', () => {
  test('Renders CountrySelectBox', () => {
    const countries: Array<IBasicCountry> = [
      { name: 'Name1', code: 'Code1', emoji: 'Emoji1' },
      { name: 'Name2', code: 'Code2', emoji: 'Emoji2' }
    ]
    const componentTree = renderer.create(<CountrySelectBox countries={countries} countrySelected={(s:string) => {}} />).toJSON();
    expect(componentTree).toMatchSnapshot();
  })
})
```  
For Styled Components in React for CSS-in-JS, check out jest-styled-components for testing your CSS style defintions with snapshot tests. This package improves the snapshot testing experience and provides a brand new matcher to make expectations on the style rules: [Jest Styled Components](https://github.com/styled-components/jest-styled-components)  

### **React Testing Library**  
[CheatSheet](https://testing-library.com/docs/react-testing-library/cheatsheet)  
[Robin Wieruch: How to use React Testing Library](https://www.robinwieruch.de/react-testing-library)  
[testing-library.com/docs/react-testing-library/intro/](https://testing-library.com/docs/react-testing-library/intro/)  
[Common mistakes with react-testing-library](https://kentcdodds.com/blog/common-mistakes-with-react-testing-library)   

* [About 'getBy...' Queries](https://testing-library.com/docs/queries/about/)  
* [Testing DOM Elements with jest-dom](https://github.com/testing-library/jest-dom/)  
* [Appearance and Disappearance of DOM Elements](https://testing-library.com/docs/guide-disappearance/)  

```javascript
import { render, screen } from '@testing-library/react'
import SearchResult from '../components/SearchResult'

test('Renders for existing PriviewBox elements', () => { 
  const items= [
    { title: "Title 1", description: "Description 1", id: "Id1" },
    { title: "Title 2", description: "Description 2", id: "Id1" }
  ]

  render(<SearchResult items={items} />)

  expect(screen.getByText(/Title 1/)).toBeInTheDocument();
  expect(screen.getByText(/Description 1/)).toBeInTheDocument();
  expect(screen.getByText(/Title 2/)).toBeInTheDocument();
  expect(screen.getByText(/Description 2/)).toBeInTheDocument();
})
```  

* [How to firing Events](https://testing-library.com/docs/dom-testing-library/api-events)  
* [Additional User-Event](https://github.com/testing-library/user-event)  

```javascript
import { render, screen } from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import Header from '../components/Header'

test('Renders for testing text-input element', () => { 
  render(<Header inputKeyPress={() => {}} />)

  const inputField = screen.getByTestId('q-input')
  const inputText = 'test-input-text'

  userEvent.type( inputField, inputText )
  expect(inputField).toHaveValue(inputText)
})
```  

* [Async Methods](https://testing-library.com/docs/dom-testing-library/api-async)  

```javascript
import { render, screen } from '@testing-library/react'

test('Testing button clicks', () => { 
  render(<App />)

  const button = screen.getByRole('button', { name: 'Click Me' })
  fireEvent.click(button)
  await screen.findByText('Clicked once')

  fireEvent.click(button)
  await screen.findByText('Clicked twice')

  // Or also just
  const componentTree = await screen.findByTestId('special-tree-section')
  expect(componentTree).toMatchSnapshot()
})
``` 
```
  getBy / queryBy / findBy

  getBy: for existing elements, if not there throws error.
  queryBy: every time you are asserting that an element isn't there, use queryBy. Otherwise default to getBy.
  findBy: The findBy search variant is used for asynchronous elements (promises/fetching) which will be there eventually (after any timedelay).
          Needs async/await test functions.
```  

### **Test Files**  
* Have to be in a Folder named "\_\_test\_\_"  
* File Name extension xxxx.test.ts or xxxx.test.tsx  
