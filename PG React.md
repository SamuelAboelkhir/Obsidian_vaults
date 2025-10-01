---
tags:
- Programming-Language/JS-TS
- Framework
- PG
MOC: Technology
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[_0006 Programming MOC|Back to index]]
### React tips
- useEffect always triggers after a render, don't use it to track changes based on user input
- Each react component has a default `key` prop that makes different instances of the same component render as different components in general
	- Useful when you need different instances of a component per user
- Functions and constants defined in a hook or any other function will continue to be recreated and rerended every time it's called. If it's static define it outside the function.
	- Callback wrappers can achieve the same goal.