---
tags: 
- PG
MOC: Knowledge Base
---
[[_0000 Home|Home]] | [[_0001 Knowledge Base MOC|Back to Knowledge MOC]] | [[PG C index|Back to index]]
- A runner is a program that runs the instructions file

# Steps
- Start the deployment server and make sure its running
- Start a gitlab runner
- Install and register the runner on the server
- Create an instructions yaml file
- Define the scope of the instructions
	- Via stages: sequentially define the name of each stage
- Add tags for different jobs under the stages
- Cleanup