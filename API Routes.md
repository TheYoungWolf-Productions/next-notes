#### Static API Routes

This a feature of next that allows us to communicate with our backend server. It is meant to send non ui requests and response i.e. POST a feedback form.

To make this work, we need to create a folder named `api` in the pages folder, therefore, any **pages** that we put in this folder will be treated in a special way. 

Example: index file in api folder will be hit when 'domain/api' is called whereas feedback file in api folder will be hit when 'domain/api/feedback' is called. 

These files do not export a react component, rather, they export a **handler** function,
```
function handler(req, res) { ... }
export default handler;
```

Any code written here will be treated as server-side code and not be bundled in the final package (just like code written in getStaicProps and getServerSideProps)

This function will need a switch statement to handle all the HTTPs verbs we expect the route to receive.

We can create pages like usual and pre-render them with data fetched from our own api routes. However, although it is common to fetch data from external sources in the getStaticProps function, we should not use fetching when trying to get data from our own API. Reason being, its unneccessary to make a call that is meant to hit the same host as the requester (requester and responder will be the same server)

`Do not use fetch/axios to talk to your own APIs`

Instead, since this all part of the same server, we should reuse any nodejs logic written in the APIs, over here. Easy way is to export reusable functions from the file in the api folder and import them to getStaticProps

#### Dynamic API routes

- These are created just the same way dynamic user routes were created. Essentially, we will create files with identifiers i.e. [demoId].js 
- We have to define handler fucntion just as before.
- We can get access to the identifier passed using req.query i.e. req.query.demoId
- Next is smart about understanding which api route to prioritize (just like page routes).
- 

