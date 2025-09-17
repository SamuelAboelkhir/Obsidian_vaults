---
tags:
- WR
- Compass
MOC: Work
---
[[_0000 Home|Home]] | [[_0002 Work MOC|Back to Work MOC]]
## Things to fix in Compass

1. Problem: The AdminPanel route is still accessible for non-admin users
	- [ ] Solution: Add role restriction for the route as a whole
2. Problem: The app seems to be getting a bit sluggish and slow
	- [ ] Solution: Check if the pages are lazy loading `CreateLazyFileRoute`, and if not, experiment with the idea of making them lazy
3. Problem: To fetch a single user from useUser you have to fetch them all then filter
	- [ ] Solution: Update useUser hook to be able to accept an ID and fetch a single user by ID

# New Features
- Permissions for roles
- Team and department modifications
- Convert to modals like MyFleeta

kanban instead of scrum

## OKRs
- Driven top down
- OKRs should be visible by default for transparency, unless the creator makes it private
	- I can create a personal OKR which is private, and choose my contributors who can also see my private OKR
- OKR visibility should include its key-results and initiatives
### Performance reviews
- Manager driven/launched
- Yearly personal OKRs can be added or edited by personal contributors
- In addition to the OKRs there should be a total of three areas to evaluate
- I can see my personal reviews
- Managers can see their team's reviews
- You can't see siblings' role reviews
- A parent role can see all children reviews
- Performance reviews
	- Meeting objectives/key-results/initiatives
	- Competencies (soft skills) 
	- Skills (technical skills)
- future ... this should produce a skills matrix or what we call a capabilities matrix
### Career development
- Employee driven
- Your personal growth/development plans
- You can choose who to share it with
### Capabilities
- You can say the company have a capability if at least 4 people are capable of doing something.
