## Things to fix in the PMM

1. Problem: The AdminPanel route is still accessible for non-admin users
	- [ ] Solution: Add role restriction for the route as a whole
2. Problem: The app seems to be getting a bit sluggish and slow
	- [ ] Solution: Check if the pages are lazy loading `CreateLazyFileRoute`, and if not, experiment with the idea of making them lazy
3. Problem: To fetch a single user from useUser you have to fetch them all then filter
	- [ ] Solution: Update useUser hook to be able to accept an ID and fetch a single user by ID


kanban instead of scrum

1- Functions and constants defined in a hook or any other function will continue to be recreated and rerended every time it's called. 
If it's static define it outside the function.

2- Callback wrappers can achieve the same goal.