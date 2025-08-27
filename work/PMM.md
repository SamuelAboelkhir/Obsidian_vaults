## Things to fix in the PMM

1. Problem: The AdminPanel route is still accessible for non-admin users
	1. Solution: Add role restriction for the route as a whole
2. Problem: The app seems to be getting a bit sluggish and slow
	1. Solution: Check if the pages are lazy loading `CreateLazyFileRoute`, and if not, experiment with the idea of making them lazy