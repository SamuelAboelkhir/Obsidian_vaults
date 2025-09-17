---
tags:
- WR
- VA
MOC: Work
---
[[_0000 Home|Home]] | [[_0001 Knowledge Base MOC|Back to Knowledge MOC]] | [[WR VA Diagnostics index|Back to index]]
### Client Info
- Owner is Viateur Audet
- Furniture moving company
- They have a fleet of trucks

### Flow (Model)
1. Pick up furniture from different destinations
2. Go to a warehouse (hub)
3. Offload, sort, label
4. Load similar destination cargo onto trucks
5. Deliver them together

### Technology (AS400/High series)
- ERP enterprise resource planning ^112aba
- SAP (German company) is the biggest ERP company

### Digital transformation program
- Multi project program
1. Truck management
2. Warehouse management
3. Modernization

### Shadow system
1. An excel sheet is housing all their data
2. Operators, dispatchers, etc. can go on that excel sheet to go all the data they need
3. An interface on top of AS400 was attempted and failed (the employees were not consulted before the interface was made)
4. It's hard to onboard people on the excel sheet

### Problem
- We are going in with the assumption that they have obsolete steps in the system to reflect the processes
- Redundancy and process overload
- Some processes aren't needed and the software doesn't reflect the company's communication structure

### Request
- Find a way to leverage the data from the data collection software called Issac

### First Steps
1. Study and understand AS400 and VA's operational modal
2. Take a look at the DB and diagnose it
3. Look for inconsistencies and define/plan what needs to be fixed and how
4. Find operational problems as we're studying the software
5. Prepare a report to present the findings
6. Research Issac's capabilities to know what data we can gather from it about each truck
7. Research DB cosmos 2 (AS400's integrated DB)