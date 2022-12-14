There are two ways to deploy
1. Standard build
	- Run by `next build`
	- This spits out a bundle that can ONLY be run by a NodeJS server. Cannot put the bundle on a static host, like we can do on React apps.
	- Pages with or without getStaticProps will still be pre-generated. The NodeJs server will be needed for getStaticPaths, getServerSideProps, API routes and revalidation etc..
	- You need to redeploy everytime your code changes (common stuff) but you also need to redeploy if your data changes and you are not using revalidation.  
3. Full Static build
	 - Run by `next export`
	 - Does not need NodeJs host
	 - Produces fully static pages (HTML, CSS, JS) with no way to leverage server side features of NextJs such as getServerSideProps, API routes and recvalidation
	 - Need to redeploy whenerver code changes (common stuff) but you also need to redeploy whenever data changes since revalidations simply won't work.

Steps:
1. Add all metadata such as head tags, optimize code etc.
2. Check env variables
	- Use next.config.js for using env variables
1. Push to testbed 
2. Deploy

Recommended platforms
- Vercel
- Netlify