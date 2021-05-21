## Initial
[material-ui.com/getting-started/installation/](https://material-ui.com/getting-started/installation/)  
After installing Material UI package, add the Font to use with (default is Roboto) to the index.html in the public folder:  

```html
<!DOCTYPE html>
<html>
  <head>
    ...
    <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,700&display=swap" />
    ...
  </head>
  <body>
    ...
  </body>
</html>
``` 

----

## TypeScript  

You can add static typing to JavaScript to improve developer productivity and code quality thanks to TypeScript.  
[https://material-ui.com/guides/typescript/](https://material-ui.com/guides/typescript/)  

----

## Typography
[material-ui.com/components/typography/](https://material-ui.com/components/typography/)  
   
```javascript
import React from 'react'
import Typography from '@material-ui/core/Typography'

export default function MyComponent() {

  return (
    <div>
      // 'component': is used for the root node (HTML element or a component). Defaults to <p> Tag
      // 'variant':   applies the theme typography styles
      <Typography
        component="h2" 
        variant="h6"
        align="center"
        color="secondary"
        gutterBottom
      >
        My Title Text
      </Typography>
    </div>
  )
}
```  

----

## Icons  
[material-ui.com/components/icons/](https://material-ui.com/components/icons/)  

Material Design has standardized over 1,100 official icons, each in five different "themes" (see below). For each SVG icon, we export the respective React component from the @material-ui/icons package. You can search the [full list of these icons](https://material-ui.com/components/material-icons/).  
**Installation**  
```bash
// with npm
npm install @material-ui/icons

// with yarn
yarn add @material-ui/icons
```  
**Using**  

```javascript
import {SendIcon, KeyboardArrowRightIcon, AcUnitOutlinedIcon} from '@material-ui/icons/'

export default function Create() {
  return (
    <Container size="sm">
      <Button
        ...
        startIcon={<SendIcon />}
        endIcon={<KeyboardArrowRightIcon />}>
      </Button>

      <AcUnitOutlinedIcon color="secondary" fontSize="large" />
    </Container>
  )
}
```  

----

## Theming
[material-ui.com/styles/advanced/](https://material-ui.com/styles/advanced/)  
[material-ui.com/customization/default-theme/](https://material-ui.com/customization/default-theme/)  
[material-ui.com/customization/color/](https://material-ui.com/customization/color/)  

**Overwrite Default Theme**  
For overwriting default theme, overwrite all neccessary properties of the default theme object: [material-ui.com/customization/default-theme/](https://material-ui.com/customization/default-theme/)  
```javascript
import { createMuiTheme, ThemeProvider } from '@material-ui/core'
import { purple } from '@material-ui/core/colors'

// For custom Font (added in typography property), import the related Font at top of the index.css file:
// @import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@300;400;500;600;700&display=swap');
const theme = createMuiTheme({
  palette: {
    primary: {
      main: '#fefefe'
    },
    secondary: purple
  },
  typography: {
    fontFamily: 'Quicksand',
    fontWeightLight: 400,
    fontWeightRegular: 500,
    fontWeightMedium: 600,
    fontWeightBold: 700,
  }
})

function App() {
  return (
    <ThemeProvider theme={theme}>
      <MyApplication />
    </ThemeProvider>
  );
}

export default App;
```  
**Accessing the theme in a component (useTheme Hook)**  
[material-ui.com/styles/advanced/#accessing-the-theme-in-a-component](https://material-ui.com/styles/advanced/#accessing-the-theme-in-a-component)
```javascript
import { useTheme } from '@material-ui/core/styles';

function DeepChild() {
  const theme = useTheme();
  return <span>{`spacing ${theme.spacing}`}</span>;
}
```  

----

## Styles  
[material-ui.com/styles/basics/](https://material-ui.com/styles/basics/)  
### **Simple makeStyles example**
```javascript
import { makeStyles } from '@material-ui/core'

const useStyles = makeStyles({
  btn: {
    fontSize: 60,
    backgroundColor: 'violet',
    '&:hover': {
      background: 'blue'
    },
  },
  title: {
    textDecoration: 'underline',
    marginBottom: 20,
  }
})

export default function MyComponent() {
  const classes = useStyles()

  return (
    <Container size="sm">
      <Typography
        className={classes.title}
        ...
      >
        My pretty styled Text
      </Typography>

      <Button>
        className={classes.btn}
        ...
      </Button>
    </Container>
  )
}
``` 
### **makeStyles example using "theme" parameter**  
All properties offered in the theme object: [material-ui.com/customization/default-theme/](https://material-ui.com/customization/default-theme/)  
```javascript
import { makeStyles } from '@material-ui/core'

const useStyles = makeStyles((theme) => ({
  title: {
    textDecoration: 'underline',
    marginBottom: theme.spacing(3)
  }
}))

export default function MyComponent() {
  const classes = useStyles()
  ...
}
``` 
### **makeStyles example using custom parameter** 
```javascript
import { makeStyles } from '@material-ui/core'

const useStyles = makeStyles({
  optionalBorder: {
    border: (note) => {
      if (note.category == 'work') {
        return '1px solid red'
      }
    }
  }
})

export default function MyComponent({ note }) {
  const classes = useStyles(note)

  return (
    <div>
      <Card className={classes.optionalBorder}>
        ...
      </Card>
    </div>
  )
}
``` 
### **Overriding styles with classes**  
[material-ui.com/customization/components/#overriding-styles-with-classes](https://material-ui.com/customization/components/#overriding-styles-with-classes)  
You can take advantage of the classes object property to customize (override) all the CSS classes (injected by Material-UI) for a given component.  
The list of classes for each component is documented in the component API page. Alternatively, you can use the browser dev tools.  
Material-UI's class names follow a simple pattern in development mode: Mui[component name]-[style rule name]-[UUID] (e.g. "MuiButton-label-tqykb1").
```javascript
const useStyles = makeStyles({
  root: {
    background: 'linear-gradient(45deg, #FE6B8B 30%, #FF8E53 90%)',
    color: 'white',
  },
  label: {
    textTransform: 'capitalize',
  },
});

export default function ClassesNesting() {
  const classes = useStyles();

  return (
    <Button
      classes={{
        root: classes.root, 
        label: classes.label,
      }}
    >
      Button Text
    </Button>
  );
}
``` 

----

## Lists  
[material-ui.com/components/lists/](https://material-ui.com/components/lists/)  
**Simple List example**
```javascript
import List from '@material-ui/core/List'
import ListItem from '@material-ui/core/ListItem'
import ListItemIcon from '@material-ui/core/ListItemIcon'
import ListItemText from '@material-ui/core/ListItemText'
import { makeStyles } from '@material-ui/core'

const useStyles = makeStyles({
  active: {
    color: 'red'
  },
  listItemTextPrimary: {
    fontWeight: 'bold',
  }
})

export default function SimpleList() {

  const classes = useStyles()

  const menuItems = [
    { 
      text: 'My Notes', 
      icon: <SubjectOutlined color="secondary" />, 
      path: '/' 
    },
    { 
      text: 'Create Note', 
      icon: <AddCircleOutlineOutlined color="secondary" />, 
      path: '/create' 
    },
  ];

  return (
    <div>
      <List>
        {menuItems.map((item) => (
          <ListItem 
            button 
            key={item.text} 
            onClick={() => router.push(item.path)}
            className={location.pathname == item.path ? classes.active : null}
          >
            <ListItemIcon>{item.icon}</ListItemIcon>
            <ListItemText
              classes={{primary: classes.listItemTextPrimary}}
              primary={item.text} 
            />
          </ListItem>
        ))}
      </List>
    </div>
  )
}
```