---
tags: 
- Other
MOC: Programming
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Other index|Back to index]]
# Components
- Active server
- A runner
	- A program that runs the instructions file
	- In gitlab the runner needs to be added to the repo, and registered as belonging to the specific repo
	- The runner can also be installed on the server and registered as belonging to that specific server
	- On gitlab, you need to pick a runner type
		- **Types**:
		    - **Shared Runners**: Provided by GitLab (on gitlab.com)
		    - **Group Runners**: Shared across projects in a group
		    - **Project Runners**: Dedicated to specific projects
		    - **Self-hosted Runners**: Your own machines/containers
	- On the server you need to specify the executor
		- **Executors** (how runners run jobs):
		    - **Docker**: Runs jobs in Docker containers (most common)
		    - **Shell**: Runs directly on the runner's shell
		    - **Kubernetes**: Runs jobs in Kubernetes pods
		    - **VirtualBox/VMware**: Runs in VMs
- Instructions
	- Normally a yaml file
	- A script with jobs that runners follow during CICD based on a trigger
	- Composed of different stages representing jobs to be carried out
	- A job is a collection of instructions
	- Jobs normally have tags
		- These tags are used to tell the runner which jobs to run
		- Multiple runners can work on the same yaml file, with each one taking a different set of jobs
# **Common Keywords**
- **`image`**: Docker image to run the job in
- **`before-script`**: Always runs before the script
- **`script`**: Commands to execute (required)
- **`stage`**: Which pipeline stage
- **`dependencies`**: Which jobs must complete first
- **`artifacts`**: Files to preserve and pass to next stages
- **`cache`**: Files to cache between pipeline runs
- **`variables`**: Environment variables
- **`only/except`**: Branch/tag conditions
- **`when`**: Conditions for running (on_success, on_failure, manual)
# Steps
- Start the deployment server and make sure its running
- Start a gitlab runner
- Install and register the runner on the server
- Create an instructions yaml file
- Define the scope of the instructions
	- Via stages: sequentially define the name of each stage
- Add tags for different jobs under the stages
- Cleanup
# Example flow with explanation
### 1. **Stages** (Pipeline Phases)
```yaml
stages: 
	- build 
	- test 
	- security 
	- deploy
```
### 2. **Jobs** (Individual Tasks)
```yaml
# Job name
build-frontend: 
# Which stage this job belongs to 
stage: build 
# Which Docker image to use 
image: node:18-alpine 
# Commands to execute 
script: 
	- cd frontend 
	- npm ci 
	- npm run build 
# Save build artifacts 
artifacts: 
	paths: 
	- frontend/.next/ 
  expire_in: 1 hour 
  # Only run on certain conditions 
  only: 
	- main 
```