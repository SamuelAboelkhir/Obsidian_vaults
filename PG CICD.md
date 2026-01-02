---
tags: 
- Other
MOC: Programming
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Other index|Back to index]]

# Continuous Integration
- This part of CICD is mostly about automating code testing and review
- When it comes to reviewing syntax, formatting and even security, computers are better at this than humans, which is where automated tests come into play in a PR, even before a human ever comes in to review things like subtle bugs or architectural decisions
- Lets take the below workflow as an example
```yaml
# Assigns human readable name to workflow
name: ci
# Creating a trigger with "on" to make the workflow run on PR to main
on:
  pull_request:
    branches: [main]
# The building blocks of workflows
jobs:
  tests:
  # Assign a human readable name
    name: Tests
    # Decide on which runner to use
    runs-on: ubuntu-latest
	# The building blocks of job
    steps:
	    # Step 1 clone the repo into the runner
      - name: Check out code
        # Specify the action with uses
        uses: actions/checkout@v4
		# Step 2 sets up go
      - name: Set up Go
        # Actions are reusable custom applications that help reduce the complexity of creating workflows
        uses: actions/setup-go@v5
        # Specify the inputs to the action with "with"
        with:
          go-version: "1.25.1"

      - name: Force Failure
        # Runs arbitrary command line commands in the runner
        run: (exit 1)
```
#### Workflows
- A workflow is triggered when an event occurs in a github repo, such as opening a PR into main
#### Jobs
- A workflow is made up of one or more of those
- A job is itself a set of steps that run on the same runner (a runner is a virtual machine that run your job on github's servers)
- We currently have 1 job only in our workflow, but you'd normally have more jobs in order to run your tests in parallel, or if you wanted to run the same tests on different operating systems
#### Steps
- A job is made up on one or more of those
- A step is a single tak that can run:
	- Commands
	- Scripts
	- Actions
- Example steps of a job could be:
	- Checking out the code
	- Installing dependencies
	- Running tests
- Our own tests job has 3 steps:
	- Check out the code
	- Set up Go
	- Force failure of the CI job
# Running Tests
- A good CI pipeline typically includes:
	- Unit tests
	- Integration tests
	- Styling checks
	- Linting checks
	- Security checks
	- Any other kind of automated test
	