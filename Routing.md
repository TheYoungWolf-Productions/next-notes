#### File based routing

We let Next infer the route structure from the folder structure in the `pages` folder as opposed to Code-based routing (in case of usual React)


![Screenshot 2022-11-25 at 7 05 08 PM](https://user-images.githubusercontent.com/68595463/207383923-1d1c869f-58c5-4684-83cb-5061adba532f.jpg)

--- 

#### Path Segments

##### Static path segments 

In tradiotional react apps, we use React-router, however, in nextjs we will not use react router at all. 

In the pages folder, index.js is a special file name where next will not use file name as a route, rather it will be rendered at '/' path. Within this file, we will simply create a react component.

When the file name is different than index, that file name is used as a route.

Creating an 'about.js' file at the root level of pages folder is equivalent to creating an 'about' folder with an index.js file within it. Both will render at /about path. The latter is a must in order to created nested paths. 

##### Dynamic path segments

This is a case where we want to dynamically render a component based on url parameter i.e., /demos/23 which will render the demo editor for the 23rd demo.

The naming pattern is always, **[someid].js** in the correct folder. In the example above, we will need an **[demoid].js**  file within the **demos** folder in *pages* folder

We can take advantage of ***useRouter*** hook which returns to us an object whose properties can be accessed to play around with routes.

`Case: demos/2`

```
const router = useRouter();
console.log(router.query) // { demoid: 2}
```

We can also have nested dynamic paths just like we have nested static paths. 

---
==Special case==

##### Implementing a "Catch All" path:

Create a file called [...slug].js. In this we can access all query params passed in the url.

`Case: demos/2/42/something`

```
const router = useRouter();
console.log(router.query) // { slug: [2, 42, 'something']}
```

---

#### Navigation

##### Navigate using Links

We use Link component (imported from next this time) just as before to navigate users on clicks

`import Link from 'next/link'`

- Pass in an href by means of using strings (template literals can be helpful)
```
<Link href='/demos/1'>First demo</Link>
```

- Pass in an href by means of using an object with pathname and query properties defined as follows:
```
<Link href={{ pathname: 'demos/[id]', query: {id: 1} }}>First demo</Link>
```

##### Programmitically Navigating

We can navigate based on some action user performs i.e. submits a form. We simply have to use router given from the useRouter. Then in a click handler, we can simply use `router.push('/clients/1')`. Just as before we can pass in an object instead.

---
#### Unknown routes - 404

When a user visits an undefined route, we need to show our own 404 error page.
To do so, we create a 404.js file in the pages folder. (It has to be named just as said)
