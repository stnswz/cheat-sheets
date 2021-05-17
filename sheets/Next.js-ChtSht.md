## Rendering SSR & Static Generation
### **Server-side Rendering (SSR)**
To use Server-side Rendering for a page, you need to **export** an **async** function called **getServerSideProps**. This function will be called by the server on every request. The returned data from getServerSideProps is passed to the related component as props.  

For a more detailed reference see: [data fetching and getServerSideProps](https://nextjs.org/docs/basic-features/data-fetching#getserversideprops-server-side-rendering)  
See here for **TyeScript** support: [typescript use getServerSideProps](https://nextjs.org/docs/basic-features/data-fetching#typescript-use-getserversideprops)  

```javascript
function Page({ data }) {
  // Render data...
}

// This gets called on every request
export async function getServerSideProps() {
  // Fetch data from external API
  const res = await fetch(`https://.../data`)
  const data = await res.json()

  // Optional boolean value to allow the page to return a 404 status and page. 
  if (!data) {
    return {
      notFound: true
    }
  }

  // Pass data to the page via props
  return { props: { data } }
}

export default Page
```  

### **Static Site Generation**  
If you **export** an **async** function called **getStaticProps** from a page, Next.js will pre-render this page at build time using the props returned by getStaticProps.  

For a more detailed reference see: [data fetching and getStaticProps](https://nextjs.org/docs/basic-features/data-fetching#getstaticprops-static-generation)  
For Incremental Static Regeneration see: [incremental static regeneration](https://nextjs.org/docs/basic-features/data-fetching#incremental-static-regeneration)  
See here for **TyeScript** support: [typescript use getStaticProps](https://nextjs.org/docs/basic-features/data-fetching#typescript-use-getstaticprops) 

```javascript
function Page({ data }) {
  // Render data...
}

// This function gets called at build time on server-side.
// It may be called again, on a serverless function, if
// revalidation is enabled and a new request comes in
export async function getStaticProps() {
  // Fetch data from external API
  const res = await fetch(`https://.../data`)
  const data = await res.json()

  // Optional boolean value to allow the page to return a 404 status and page. 
  if (!data) {
    return {
      notFound: true
    }
  }

  // Pass data to the page via props (see also revalidation)
  return { 
    props: { data },
    // Next.js will attempt to re-generate the page:
    // - When a request comes in
    // - At most once every second
    revalidate: 1, // In seconds
  }
}

export default Page
``` 

### **Static Site Generation using getStaticPaths()**  
Next.js allows you to create pages with **dynamic routes**. For example, you can create a file called pages/posts/[id].js to show a single blog post based on id. This will allow you to show a blog post with id: 1 when you access posts/1, id: 2 when you access posts/2 etc.  
To handle this, Next.js lets you export an **async** function called **getStaticPaths** from a dynamic page (pages/posts/[id].js in this case). This function gets called at build time and lets you specify which paths you want to pre-render.  

See also: [page paths depend on external data](https://nextjs.org/docs/basic-features/pages#scenario-2-your-page-paths-depend-on-external-data)  
For a more detailed reference see: [data fetching with getStaticPaths static generation](https://nextjs.org/docs/basic-features/data-fetching#getstaticpaths-static-generation)
```javascript
// This function gets called at build time
export const getStaticPaths = async () => {
  const res = await fetch('https://jsonplaceholder.typicode.com/users');
  const data = await res.json();

  // map data to an array of path objects with params (id)
  const paths = data.map(ninja => {
    return {
      params: { id: ninja.id.toString() }
    }
  })

  // We'll pre-render only these paths at build time.
  // { fallback: false } means other routes should 404.
  return {
    paths, 
    fallback: false
  }
}

// 'context' is the object returned by getStaticPaths. You also could use "async ({ params }) => {"
export const getStaticProps = async (context) => {
  const id = context.params.id;
  const res = await fetch('https://jsonplaceholder.typicode.com/users/' + id);
  const data = await res.json();

  return {
    props: { ninja: data }
  }
}

const Details = ({ ninja }) => {
  return (
    <div>
      <h1>{ ninja.name }</h1>
      <p>{ ninja.email }</p>
      <p>{ ninja.website }</p>
      <p>{ ninja.address.city }</p>
    </div>
  );
}

export default Details;
```

----  

## Next CSS
### **Normal CSS**  
  
```javascript
import '../styles/globals.css'

function MyApp({ Component, pageProps }) {
  return (
    <div className="container">
      <Component {...pageProps} />
    </div>
  )
}
```  
  
### **CSS Modules**  
With using CSS Modules the used CSS is scoped  
  
```scss
/* MyComponent.module.css */
.title {
  ...
}
.text {
  ...
}
```  

```javascript
import styles from '../styles/MyComponent.module.css'

export default function MyComponent() {
  return (
    <div>
      <h1 className={styles.title}>Homepage</h1>
      <p className={styles.text}>Hello</p>
    </div>
  )
}
```  

----  

## Next Link Component

The Link component is similar to using &lt;a&gt; tags, but instead of &lt;a href="..."&gt;, you use &lt;Link href="..."&gt; and put an &lt;a&gt; tag inside.  
Link Attributes and more: [nextjs.org/docs/api-reference/next/link](https://nextjs.org/docs/api-reference/next/link)  
```javascript
import Link from 'next/link'

<div>
  <Link href="/posts/first-post">
    <a>Link to first Post</a>
  </Link>
</div>
```  

----  

## Next Image Component
Image Optimization can be enabled via the &lt;Image /&gt; component exported by next/image.  
[nextjs.org/docs/api-reference/next/image](https://nextjs.org/docs/api-reference/next/image)   
```javascript
import Image from 'next/image'

function Home() {
  return (
    <>
      <h1>My Homepage</h1>
      <Image
        src="/me.png"
        alt="Picture of the author"
        width={500}
        height={500}
      />
      <p>Welcome to my homepage!</p>
    </>
  )
}

export default Home
``` 
**Example with loader**  
[nextjs.org/docs/api-reference/next/image#loader](https://nextjs.org/docs/api-reference/next/image#loader)
```javascript
import Image from 'next/image'

const myLoader = ({ src, width, quality }) => {
  return `https://example.com/${src}?w=${width}&q=${quality || 75}`
}

const MyImage = (props) => {
  return (
    <Image
      loader={myLoader}
      src="/me.png"
      alt="Picture of the author"
      width={500}
      height={500}
    />
  )
}
``` 
----  

## Next Routing / Router
### **Routing**  
File-System based routing built on the concept of pages:  
[nextjs.org/docs/routing/introduction](https://nextjs.org/docs/routing/introduction)  

```javascript
import Link from 'next/link'

function Home() {
  return (
    <ul>
      <li>
        <Link href="/">
          <a>Home</a>
        </Link>
      </li>
      <li>
        <Link href="/about">
          <a>About Us</a>
        </Link>
      </li>
      <li>
        <Link href="/blog/hello-world">
          <a>Blog Post</a>
        </Link>
      </li>
    </ul>
  )
}

export default Home
```

### **Router**  
If you want to access the **router object** inside any function component, you can use the **useRouter** hook:  
[nextjs.org/docs/api-reference/next/router](https://nextjs.org/docs/api-reference/next/router)  

```javascript
import { useRouter } from 'next/router'

function ActiveLink({ children, href }) {
  const router = useRouter()
  const style = {
    color: router.asPath === href ? 'red' : 'black',
  }

  const handleClick = (e) => {
    e.preventDefault()
    router.push(href)
  }

  return (
    <a href={href} onClick={handleClick} style={style}>
      {children}
    </a>
  )
}

export default ActiveLink
```
