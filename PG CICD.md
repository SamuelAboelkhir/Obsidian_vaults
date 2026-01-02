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
name: ci

on:
  pull_request:
    branches: [main]

jobs:
  tests:
    name: Tests
    runs-on: ubuntu-latest

    steps:
      - name: Check out code
        uses: actions/checkout@v4

      - name: Set up Go
        uses: actions/setup-go@v5
        with:
          go-version: "1.25.1"

      - name: Force Failure
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
- 