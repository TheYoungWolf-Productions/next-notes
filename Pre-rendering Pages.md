As opposed to traditional react apps, where client is initially sent an empty html page, which will subsequently get populated with data; NextJs pre-renders, pre-populates the page before sending it to the client. Therefore, client receives a page that is (almost) fully constructed.

NextJs also does **Hydrating**: 
- The pre-rendered page sent back carries additional Javascript code that will be responsible for providing interactivity by means of React. 

Due to all content already being part of the page, crawlers have an easy time parsing it. They either way do not care about the interactions. 

##### Types
- Static site generation (recommended)
- Server side rendering

#### Static site generation

- **All** the pages are pre-generated in advance during build time i.e. before deployment to production. 
- Pages prepared ahead of time can be cached by the Server/CDN in order to instantly serve the page when requested.
- As usual, these pages will be hydrated by React so interactivity can be achieved on the client side.

In order to tell NesxtJs that a page needs to pre-generated. We use a special function in components of the Pages folder (ONLY)

`export async function getStaticProps(context) { ... }`

- This function has server-side code only, i.e. no client APIs. 
	- Moreover, anything written within this fucntion doesn't get compiled and sent to client.
- The use of this function only affirms that this page must be pre-rendered and is **not** a means of toggling pre-rendering
- Chronologically, this function runs before the component function is run. The objective is to **prepare** the props for your component function.
- Therfore, all this function returns is an object with a key called 'props' having another object (containing all props) as its value.
```
{ props: {
	prop1: ...,
	prop2: ...,
  },
  revalidate: number,
  notFound: boolean,
  redirect: { destination: string } 
}
```

Essentially, this way we do not have to run useEffects on client to fetch the data, instead, we run this function, that fetches all the data on the server side. Then, the page is pre-rendered and what the client receives is a content rich page.

This function also gets a context passed to it which can be used to do various tasks such as identify the params passed in the url. We will not use useRouter in this case because, getStaticProps is being run on server side before the component function runs.

**Tip**: For any page containing only static content, Next will simply, by default, pre-render it and send it to client. The exception to this law, are the dynamic routed pages i.e. [demoId].js. The reason being, NextJs does not know in advance about what the ids will be passed to this dynamic page, and in extension, what data will it carry.

`export async function getStaticPaths(context) { ... }`

The goal of this function is to tell Next about which *instances* of a dynamic page must be generated, therefore its only to be used in cases where there can be multiple instances of the same page i.e. dynamic pages like [demoId].js
- All this function returns is an object with a key called 'paths', which is an array.

```
{ paths: [
	{ params: { demoId: 1 } },
	{ params: { demoId: 1 } },
	{ params: { demoId: 1 } },
  ],
  fallback: boolean | "blocking"
}
```

This function is essentially required to tell Nextjs which concrete instances of this dynamic page must be re-generated.

The `fallback` key simply notifies Next whether to show 404 page for unknown params or not. The idea is, if we are setting fallback to false, then we need to ensure that we pass **all possible params** to the paths array i.e. we're passing all the possible params and if next encounters unknown params, then just show 404.
Similarly, if we set fallback to true, we're signalling Next that there might be params that turn out not to be a part of the paths array i.e. we are only passing in the important or featured params to be pre-rendered and expect others to not be pre-rendered.

Alternatively, fallback can be set to "blocking", which simply means that next will not serve anything till the non pre-rendered data is made available ?through running getStaticProps?.

###### Disadvantage

This technique is most useful when the content is relatively static in nature, meaning we don't have to update it from client side. Such as blog posts which will remain the same as pre-rendered content throughout its lifecycle. In such case, whenever a new blog post will need to be added, we will have to re-build the site so the pre-rendered content will now be updated with the latest changes.
	-The easiest solution is to prerender the page with outdated content and fetch the latest content using useEffect.

###### Resolution

NextJs has a feature called **Incremental Static Generation** wherein we can decide to *Regenerate a page on every request if X seconds have elapsed since last generation*.

![Screenshot 2022-11-28 at 10 30 07 PM](https://user-images.githubusercontent.com/68595463/207384616-e3fb1631-8aaa-4ab4-a96c-b64b61cf9ff7.jpg)

To leverage this, in the return object of getStatic props function, we can pass a `revalidate` key with seconds as value. This value will determine whether to re-generate a page (depending on whether the said number of seconds had elapsed since last generation)

Other keys in this return object could be `notFound: boolean` and `redirect: {destination: string}` 

#### Server side rendering

- Pages are created just in time i.e. after deployment after a request reaches the server.
- Used when we have to pre-render a page for every incoming request and/or we need access to the concrete request/response object
- To do so, we have a special function, that is executed everytime a request comes to the server.

`export async function getServerSideProps(context) { ... }`

**Important:** This function cannot be used along with getStaticProps as they clash in their nature

The return object for this fucntion is the same as the return object for getStaticProps, with exception of `revalidate` key.

```
{ props: {
	prop1: ...,
	prop2: ...,
  },
  notFound: boolean,
  redirect: { destination: string } 
}
```


#### Client side fetching

Data that is volatile in nature does not need to be pre-generated all the time, as it may become obsolete very soon. This could also be data that is behind auth gaurds which eitherway can never be SEO-ed. 
To do so, we take the traditional approach of using useEffect with functional components to fetch data.

Next team has created a react hook to ease data fetching with other capabilities that can be used instead of using fetch or axios. The library is called `swr` which provides a **useSWR** hook.


###### Disadvantage

The biggest issue with client fetching is definitely the fact that the initial page pre-rendered (by default) by next contains no content. Therefore, crawling is a no-go in this.


### SSG vs SSR vs CSR

Ask the following questions:
1. Does the data change frequently - Go CSR
2. Is the data user-specific which will be gauarded behind auth - Go CSR
3. Do you want SEO capabilities - Go SSG or SSR
4. Do you want server to pre-render on every request - Go SSR
5. Do you want server to pre-generate the data and only re-generate on set intervals - Go SSG
