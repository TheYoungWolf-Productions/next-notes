We need to optimize our websites to become SEO-able and also improve user experience.

#### Head section

`import Head from 'next/head'`

This can be imported in any page and whatever we enter as children to the <Head> tag will be injected by next in the head of the page. Ideally, we have to set the head content for every page and every conditional JSX of that page.

There are three ways to reuse the head logic to avoid repitition. 
1. React approach - We assign a common jsx element to a variable and use that variable in all conditional JSX's.
2. We can utilize the _app.js file which is responsible for rendering all the subsequent pages on our website. Essentially, app file encapsulates all pages files and anything defined in the app file will show up for all pages
	- Next is responsible for merging Head contents defined across different pages i.e. the head content defined in app file, as well as, defined in the page file. Next will therefore, merge these head contents into single head tag and also resolve any conflicts by choosing the latest entry.  
	- We can add a `key` attribute to the JSX elements in the Head tag for letting Next know which category the element belongs to and Next will priortize the latest element with the said key while merging. 
	- Usecase: We can provide a generic title for all pages by defining it in the app file so all pages will have a title by default that will subsequently get overwritten if the page carries its own title. 
3. We can also add a special `_document.js` file. This file is responsible for manipulating document wide content unlike app file which is reponsible only for manipulating body wide content.
	- This is a class based component as it has to extend another class called 'Document'.
	- It also has to be structured in a very specific manner containing `Head` (not the same as Head from next/head), `Main`, `NextScript` and `HTML` jsx elements, which are also imported.
	- We use this file to create extra components that are outside the component tree of our application such as portals.

#### Images
In order to optimize images, we can simply use Image element provided by next. 

`import Image from 'next/image'

- This will create multiple versions of images, optimized for devices which requests are sending it. 
- The Image JSX tag takes in src, alt, height and width. The width and height provided here only determines the dimension of the image that will be fetched and not the width and height of the acutal image that will show up. For that, normal css will work. 
- This will also create images that will be lazy loaded on their own
- 

