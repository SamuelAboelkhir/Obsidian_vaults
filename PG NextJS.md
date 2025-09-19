---
tags:
- JS/TS
- Framework
- PG
MOC: Knowledge Base
---
[[_0000 Home|Home]] | [[_0001 Knowledge Base MOC|Back to Knowledge MOC]] | [[PG JS-TS index|Back to index]]
# Personal thoughts (SSR)
- When working with nextJS it's important to always keep its SSR (server side rendering) nature in mind
- Certain development patters will not work out of the box like the do in [[PG React|React]] as you must wait and consider "is this component static? (SSR) or is it dynamic? (CSR)". If the component turns out to be dynamic and reliant on user input (has elements that can't be pre-rendered) the component will be considered CSR, and you must mark it with the `'use client'` directive.
- If you forget to mark a CSR component with `'use client'` nextJS will remind you at runtime
# Project Structure
- [From the docs](https://nextjs.org/docs/app/getting-started/project-structure)
- A valid structure that's similar to react is:
```
app/ //use it like the router folder
features/
	user/
		components
		hooks
		types
shared/
```
# Server and Client Components
- [From the docs](https://nextjs.org/docs/app/getting-started/server-and-client-components)
- All components are server components by default
### [When to use Server and Client Components?](https://nextjs.org/docs/app/getting-started/server-and-client-components#when-to-use-server-and-client-components)

The client and server environments have different capabilities. Server and Client components allow you to run logic in each environment depending on your use case.

Use **Client Components** when you need:

- [State](https://react.dev/learn/managing-state) and [event handlers](https://react.dev/learn/responding-to-events). E.g. `onClick`, `onChange`.
- [Lifecycle logic](https://react.dev/learn/lifecycle-of-reactive-effects). E.g. `useEffect`.
- Browser-only APIs. E.g. `localStorage`, `window`, `Navigator.geolocation`, etc.
- [Custom hooks](https://react.dev/learn/reusing-logic-with-custom-hooks).

Use **Server Components** when you need:

- Fetch data from databases or APIs close to the source.
- Use API keys, tokens, and other secrets without exposing them to the client.
- Reduce the amount of JavaScript sent to the browser.
- Improve the [First Contentful Paint (FCP)](https://web.dev/fcp/), and stream content progressively to the client.

For example, the `<Page>` component is a Server Component that fetches data about a post, and passes it as props to the `<LikeButton>` which handles client-side interactivity.

`app/[id]/page.tsx`

```
import LikeButton from '@/app/ui/like-button'
import { getPost } from '@/lib/data' 

export default async function Page({  
params,
}: {  
params: Promise<{ id: string }>}) {  
const { id } = await params  
const post = await getPost(id)   
return (    
<div>     
	<main>        
		<h1>{post.title}</h1>        
		{/* ... */}        
		<LikeButton likes={post.likes} />      
	</main>    
</div>  
)}
```

`app/ui/like-button.tsx`

```
'use client' 
import { useState } from 'react' 
export default function LikeButton({ 
likes 
}: { 
likes: number }) {  
// ...
}
```
# All features of next
- [From the docs](https://nextjs.org/docs/app/api-reference)