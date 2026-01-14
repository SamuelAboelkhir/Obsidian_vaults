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
## Running Tests
- A good CI pipeline typically includes:
	- Unit tests
	- Integration tests
	- Styling checks
	- Linting checks
	- Security checks
	- Any other kind of automated test
## Tests
#### Code Coverage
- This is a measure of how much of the code is being tested
- Basically it's a metric that checks how many lines of code are being tested
- Testing 500 out of 1000 lines is 50% coverage for example
- This is actually a controversial topic it seems, but I haven't developed an opinion on it yet
#### Formatting
- For formatting we added a new job with the only difference being a step where the formatter runs
- Since the backend is written in go, this was the command `test -z $(go fmt ./...)`
#### Linting
- Linting is the analysis of the code to detect functional issues
- Staticcheck is the most popular Go linter, even beating golint
- To install staticcheck `go install honnef.co/go/tools/cmd/staticcheck@latest`, then run with `staticcheck ./...`
- These steps apply to the CI workflow too, so we added the staticcheck install and run commands to the style job
## Security
- We can also run static security checks on our code during CI to make sure we don't miss potential security vulnerabilities
- A popular open source tool that does that is Gosec
- Installation `go install github.com/securego/gosec/v2/cmd/gosec@latest`
- Running `gosec ./...`
## Review
- Automated tests help us eliminate around 80% of the most obvious bugs, like stylistic anti-patterns and security vulnerabilities, but they can't catch everything, writing good secure code, is still a must
- Final CI yaml
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
        run: go test ./... -cover

      - name: Install gosec
        run: go install github.com/securego/gosec/v2/cmd/gosec@latest

      - name: Security Check
        run: gosec ./...

  style:
    name: Style
    runs-on: ubuntu-latest

    steps:
      - name: Check out code
        uses: actions/checkout@v4

      - name: Set up Go
        uses: actions/setup-go@v5
        with:
          go-version: "1.25.1"

      - name: Format Files
        run: test -z $(go fmt ./...)

      - name: Install staticcheck
        run: go install honnef.co/go/tools/cmd/staticcheck@latest

      - name: Lint Files
        run: staticcheck ./...
```
# Continuous Deployment
- This step starts after CI and after the PR has been accepted, meaning the repo has been pushed to main
- Here is where we deploy the app to the server that will host the app and serve it to users
- This part can be as manual or as automated as the company sees fit, as you can deploy immediately to a cloud provider such as the google cloud platform GCP, where you can publish [[PG Docker#Publishing]] your images in an archive [[PG Docker#What is this thing?]] and even host the app on their servers where they will handle DNS, Load balancing and automatic scaling, but you'll pay more than if you take more responsibility [[NET Shared Responsibility Model]] and just pay to rent a server from the cloud provider and handle the security and DNS and scaling yourself. It mostly depends on the company needs
- When using a provider such as GCP, you'll have to create an account and setup billing even when using their free-tier, if they even have a free-tier
- You'll also need to update your CD workflow to add an authentication step to the provider, setup their CLI SDK and build/deploy your "latest image", that's why we said you should add a latest image that's a copy of the last semantic version [[PG Docker#Publishing]]
```yaml
name: cd

on:
  push:
    branched: [main]

jobs:
  deploy:
    name: Deploy
    runs-on: ubuntu-latest

    steps:
      - name: Check out code
        uses: actions/checkout@v4

      - name: Set up Go
        uses: actions/setup-go@v5
        with:
          go-version: "1.25.1"

      - name: Build App
        run: ./scripts/buildprod.sh

      - id: auth
        uses: google-github-actions/auth@v2
        with:
          credentials_json: '${{ secrets.GCP_CREDENTIALS }}'

      - name: Set up Cloud SDK
        uses: google-github-actions/setup-gcloud@v3

      - name: Build and Deploy
        run: gcloud builds submit --tag us-central1-docker.pkg.dev/notely-483210/notely-ar-repo/blackdovah/notely:latest .
```
- The credentials you'll generate are similar to ssh keys, in this case, it was a JSON key, and since github runners will do the work, they need to have access to the key so that they can authenticate with the cloud provider
# Deploy
## What Makes a “Good” CI/CD Pipeline?
- Deterministic builds. The same code should always produce the same build.
- Fast builds. The faster the better. This makes getting bug fixes and new features out to users faster.
- Portable. This is why I love when the majority of a CI/CD pipeline is just bash scripts. It's easy to run locally, and it's easy to run on any CI/CD platform.
- Fully automated. The fewer manual steps, the better. It's really annoying to manually run database migrations and click buttons. It's also error-prone.
## Cleaning Up
- We tested the service creation with boot.dev's getting started image. Now that we're done with it, we shouldn't leave unused resources hanging around, that's bad practice
- However, we now want to try building and deploying notely automatically as part of the CD instead of only building it
```yaml
name: cd

on:
  push:
    branched: [main]

jobs:
  teploy:
    name: Deploy
    runs-on: ubuntu-latest

    steps:
      - name: Check out code
        uses: actions/checkout@v4

      - name: Set up Go
        uses: actions/setup-go@v5
        with:
          go-version: "1.25.1"

      - name: Build App
        run: ./scripts/buildprod.sh

      - id: auth
        uses: google-github-actions/auth@v2
        with:
          credentials_json: '${{ secrets.GCP_CREDENTIALS }}'

      - name: Set up Cloud SDK
        uses: google-github-actions/setup-gcloud@v3

      - name: Build and Deploy
        run: gcloud builds submit --tag us-central1-docker.pkg.dev/notely-483210/notely-ar-repo/blackdovah/notely:latest .
        # Deployment to cloud run
	  - name: Deploy to Cloud Run
        run: gcloud run deploy notely --image us-central1-docker.pkg.dev/notely-483210/notely-ar-repo/blackdovah/notely:latest --region us-central1 --allow-unauthenticated --project notely-483210 --max-instances=4
```
## Database
- For the database, we will use Turso, a cloud provider that specializes in hosting serverlesss SQLite-like databases
- Installation: `curl -sSL tur.so/install | sh`
- We will also use goose for the DB migrations
- Installation: `go install github.com/pressly/goose/v3/cmd/goose@latest`
- Next. we need to generate a key for the DB `turso db tokens create notely-db`, that we will add to the connection string, and the .env file
- The connection string is obtainable from torso itself from the database, and will look like this `libsql://notely-db-YOURNAME.turso.io`
- With the token in the .env `DATABASE_URL=libsql://notely-db-YOURNAME.turso.io?authToken=YOURTOKENHERE`
- Now, we can add the token as a secret to github, and add this new environment variable to the script
```yaml
jobs:
  deploy:
    name: Deploy
    runs-on: ubuntu-latest

    # This part
    env:
      DATABASE_URL: ${{ secrets.DATABASE_URL }}
```
- We can also add the goose installation step and migration step (the migration itself is a shell script in the codebase) after the build step so we don't run into migration issues if the build to the latest version fails, and before the deployment step so that the migration will be live before the new code gets deployed
- Remember to follow best migration practices [[PG SQL#Migrations]]
- We can also use `git diff` or `git diff HEAD` to make sure no sensitive data made it to the source code
## Using the DB
- Now we can configure GCP to use the database
- We will add the database URL, the one with the token, as a secret, and configure GCP's runner to use that secret when deploying our app
## Some Things to Keep in Mind
- Google Cloud Platform (GCP) is just one of the 3 major cloud providers. AWS and Azure are also popular choices. In many ways, their offerings are similar, but sometimes the differences matter.
- Google Cloud Run handles a lot of complexity for you. Managing DNS, SSL, load balancing, and auto-scaling are all things that many companies do manually, so those are still useful skills to have, but are outside the scope of this course.
- Turso is a fully-managed third-party database host. There are _many_ options out there for databases and database hosting that are worth learning about, but again, outside the scope of this course.
- Essentially every technology/product we used in this course has viable alternatives. You don't need to know how to use all of them before your first job, but you should understand _some_ of them.