## SASS

**SASS Variables:**
```scss
$positive  : #21BA45;
$info      : #31CCEC;
$extra     : $positive;
$leftRightPadding: 40;
```  
  
**SASS Variable + Unit:**
```scss
$pageLeftRightPadding: (1000 + (2*$leftRightPadding)) + unquote("px");  
```  

**SASS Extend:**
```scss
.my-special-class-1 {
  ...
  ...
}
.my-special-class-2 {
  @extend .my-special-class-1;
  ...
  ...
}
```  
  
**The Sass Ampersand**  
For more options of using ampersand see also: [css-tricks.com/the-sass-ampersand/](https://css-tricks.com/the-sass-ampersand/)  
```scss
.button {
  &:visited { }
  &:hover { }
  &:active { }
}
```  

## Listen
See also: [developer.mozilla.org/de/docs/Web/CSS/list-style-type](https://developer.mozilla.org/de/docs/Web/CSS/list-style-type)
```scss
ul {
  list-style-type: none; /* disc, circle, square / upper-roman, lower-alpha */
  margin:0px;
  padding-left:0px;
  li {
    padding-left:22px;
    margin-top:0px;
    background: url(../assets/images/bubble_green.svg) no-repeat 0px 4px;
    background-size: 15px;
  }
}
```  
  
## Line Clampin
See also: [blog.kulturbanause.de/2020/12/text-css-kuerzen/](https://blog.kulturbanause.de/2020/12/text-css-kuerzen/)  
For understanding line clampin tricks see: [css-tricks.com/line-clampin/](https://css-tricks.com/line-clampin/)  

```scss
.line-clamp {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
```  
  
## Screenreader Only

See also: [stackoverflow.com/questions/...](https://stackoverflow.com/questions/26032089/in-html-how-can-i-have-text-that-is-only-accessible-for-screen-readers-i-e-fo)
```scss
.screenreader-only {
  position: absolute !important;
  height: 1px; width: 1px;
  overflow: hidden;
  clip: rect(1px, 1px, 1px, 1px);
}
```  
  
## Embedding Fonts
```scss
/* noto-sans-regular - latin */
@font-face {
  font-family: 'Noto Sans';
  font-style: normal;
  font-weight: 400;
  src: local('Noto Sans'), local('NotoSans'),
       url('../assets/fonts/noto-sans-v9-latin-regular.woff2') format('woff2'), /* Super Modern Browsers */
       url('../assets/fonts/noto-sans-v9-latin-regular.woff') format('woff'), /* Modern Browsers */
       url('../assets/fonts/noto-sans-v9-latin-regular.ttf') format('truetype'), /* Safari, Android, iOS */
       url('../assets/fonts/noto-sans-v9-latin-regular.svg#NotoSans') format('svg'); /* Legacy iOS */
}

/* noto-sans-700 - latin */
@font-face {
  font-family: 'Noto Sans';
  font-style: normal;
  font-weight: 700;
  src: local('Noto Sans Bold'), local('NotoSans-Bold'),
       url('../assets/fonts/noto-sans-v9-latin-700.woff2') format('woff2'), /* Super Modern Browsers */
       url('../assets/fonts/noto-sans-v9-latin-700.woff') format('woff'), /* Modern Browsers */
       url('../assets/fonts/noto-sans-v9-latin-700.ttf') format('truetype'), /* Safari, Android, iOS */
       url('../assets/fonts/noto-sans-v9-latin-700.svg#NotoSans') format('svg'); /* Legacy iOS */
}

/* noto-sans-italic - latin */
@font-face {
  font-family: 'Noto Sans';
  font-style: italic;
  font-weight: 400;
  src: local('Noto Sans Italic'), local('NotoSans-Italic'),
       url('../assets/fonts/noto-sans-v9-latin-italic.woff2') format('woff2'), /* Super Modern Browsers */
       url('../assets/fonts/noto-sans-v9-latin-italic.woff') format('woff'), /* Modern Browsers */
       url('../assets/fonts/noto-sans-v9-latin-italic.ttf') format('truetype'), /* Safari, Android, iOS */
       url('../assets/fonts/noto-sans-v9-latin-italic.svg#NotoSans') format('svg'); /* Legacy iOS */
}

/* noto-sans-700italic - latin */
@font-face {
  font-family: 'Noto Sans';
  font-style: italic;
  font-weight: 700;
  src: local('Noto Sans Bold Italic'), local('NotoSans-BoldItalic'),
       url('../assets/fonts/noto-sans-v9-latin-700italic.woff2') format('woff2'), /* Super Modern Browsers */
       url('../assets/fonts/noto-sans-v9-latin-700italic.woff') format('woff'), /* Modern Browsers */
       url('../assets/fonts/noto-sans-v9-latin-700italic.ttf') format('truetype'), /* Safari, Android, iOS */
       url('../assets/fonts/noto-sans-v9-latin-700italic.svg#NotoSans') format('svg'); /* Legacy iOS */
}

body {
  font-family: 'Noto Sans', sans-serif;
}
``` 