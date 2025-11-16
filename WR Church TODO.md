---
tags:
- TODO
- Church
- Projects
- Service
MOC: Work
---

[[_0000 Home|Home]] | [[_0002 Work MOC|Back to Work MOC]] | [[WR Projects index]]

- [ ] Add an attendance entity to store all attendance data based on activity type
- [ ] Add activity type enum front/back
- [ ] Attendance component should:
	1. Receive an activity type, e.g اجتماع مدارس الاحد
	2. Use it to fetch activity members
	3. Create a checkbox per member
	4. When the checkbox is checked, add the member to an "attended array"
	5. Submit the array to the attendance endpoint and add +1 attendance for each member in the array