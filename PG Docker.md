---
tags: 
- Other
MOC: Programming
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Other index|Back to index]]

# What is this thing?
- Well, docker is basically a kind of lightweight VM that allows us to run code inside a "container", which is basically an isolated environment that can let you run an app that maybe isn't supported on your own OS with its dependencies and environment variables all packaged in the container
- Docker images have hubs just like code and apps have hubs (Github, Gitlab, etc), docker has [Docker hub](https://www.docker.com/products/docker-hub/) as well as AWS ECR, GCP Artifact Registry and Azure Container Registry
- Now, lets get back to containers and virtual machines
- Docker also requires the "Docker server" or "Docker Daemon" to be running and listening to requests from the desktop app. If they're not running nothing else will work
#### Virtual machines
- Virtual machines are great as they allow us to have an entire 2nd OS running on your machine, problem is, they're really slow, taking even longer than a physical machine to boot up
- Depending on the underlying technology, hypervisor can be installed on the bare metal, and each guest VM will be running on the machine directly, or hypervisor will be running on top of a host OS, and the VMs will be running on top of the host OS
![[VM_architecture.png]]
#### Containers
- They offer 90% of the benefits of a VM while being super fast and lightweight
- Containers can boot up in seconds, while a VM can take minutes
- VMs virtualize the entire hardware, and emulate what a physical computer does at a low level while containers virtualize at the OS level
- Containers gives the impression that they have isolated operating and filesystems, while in reality they share a lot of resources, however, they share them securely through namespaces
- The fact containers virtualize at the OS level, means that they share the host kernel, and generally speaking, you can't have a windows docker run on a linux machine for example. However, that's only because linux uses docker natively, but you can still have a wine or a VM acting as a compatibility layer inside a container which would run the windows or mac app inside just fine
![[Containers_architecture.png]]
# Images
- An image is a read-only blueprint, or kinda like a class. It's meant to define the container, and the containers are then like objects or instances of the class
- Containers though are read-write and you can change them
#### Running a container
```zsh
docker run -d -p hostport:containerport namespace/name:tag
```
- `-d`: Run in detached mode (doesn't block your terminal)
- `-p`: Publish a container's port to the host (forwarding)
- `hostport`: The port on your local machine
- `containerport`: The port inside the container
- `namespace/name`: The name of the image (usually in the format `username/repo`)
- `tag`: The version of the image (often `latest`)
- Docker has many commands and honestly, a decent help menu that you can access with `docker --help`
- Other useful commands are:
	- `docker stop`: Stops a container safely by issuing `SIGTERM`
	- `docker kill`: Stops a container forcefully by issuing `SIGKILL`
- You can also have multiple containers running at the same time, made from different images, or the same image, which is useful if you need to limit the number of users capable of interacting with one instance of your backend (1 container) and so that you can automatically create new containers during CICD to spread the load
# Volumes
- Just like running an app and declaring variables inside of it, where the variables are cleared from memory at the end of the run, docker containers don't persist their data. They do as long as they exist at least, but if you remove a container, all of its data will be lost
- In order to prevent this, you can create a volume, which is basically just a path on the host machine (an empty folder) that you can map to a path on the docker filesystem where the app in docker saves its data, then the data will be saved on the host machine too, and other containers can also use the same volume and see this data
- It's basically like having an old playstation 2, where the image is the game disc, the container is a running instance of the game, and the volume is the external memory you used to save your progress
# Exec
- You can also run commands inside the container if needed using the `docker exec`command, for example `docker exec <CONTAINER_ID> ls`
- You can also have an open terminal session with `docker exec -it <CONTAINER_ID> sh or /bin/sh` where:
	- `-i`: interactive exec
	- `-t`: TTY (teletypewriter) interface, which just means terminal really
- You can execute commands without entering the container by adding `-c "<command"` after the `-sh` argument
- You can pass a `--user <username>` after the `exec` argument to enter the container as a specific user
# Networking
- Usually when you run a container it will have external network access by default, however, sometimes, that's not desirable. You may not want the container to have internet access if:
	- You're running 3rd party code that you don't trust, and it shouldn't need network access
	- You're building an e-learning site, and you're allowing students to execute code on your machines
	- You know a container has a virus that's sending malicious requests over the internet, and you want to do an audit
- In this case, you can use the `--network none` flag to isolate the container
#### Load Balancers
- Normally, one container, especially one that's hosting the backend of a popular high traffic service, can't handle all that traffic on its own, and as mentioned in [[#Running a container]] you can spread the load over multiple containers
- Spreading the load is a task delegated to a "load balancer" which receives the request, and then forwards it to one of the running containers
- Now, normally load balancers are actual servers that balances the load over multiple other servers all hosting the same app (they're basically copies of each other) via a load balancing strategy like "round robin" where you just go in a repeating loop, server 1, 2, 3 then again 1, 2, 3 and so on, or a more sophisticated approach where the balancer checks actual resource utilization on the server (memory and CPU) and forwards the request to the least overwhelmed server
- It's possible to apply this idea though to anything that counts as a separate computer, so full physical servers, VMs, or even containers
#### Custom Network
- A bridge allows us to establish communications between our containers while still keeping them isolated
- To try out load balancing we used [Caddy](https://hub.docker.com/_/caddy) and made 2 docker containers using 2 index.html files
	- `docker run -d -p 8881:80 -v $PWD/index1.html:/usr/share/caddy/index.html caddy`
- The system we then built made sure that our application servers are hidden within this custom network, and only the load balancer was exposed to the actual host machine, so we could only access the applications via the load balancer itself
- Then to create the network `docker network create caddytest`
- To attach our containers and have them exposed to the load balancer only, we had to remove the old containers and make new ones, because `-p` can't be modified
- The new command was `docker run -d --network caddytest --name caddy1 -v $PWD/index1.html:/usr/share/caddy/index.html caddy`
- Now, that we have the custom bridge network, we can make the load balancer
- To do so, we need a "Caddyfile" to tell caddy how to do its load balancing (round robin in our case)
```
localhost:80

reverse_proxy caddy1:80 caddy2:80 {
	lb_policy       round_robin
}
```
- This file tells caddy:
	- Run on localhost:80
	- Round robin incoming traffic to `caddy1:80` and `caddy2:80`
- The load balancer will be created with `docker run -d --network caddytest -p 8880:80 -v $PWD/Caddyfile:/etc/caddy/Caddyfile caddy`
- Hurray, now when we go to `localhost:8880` and refresh we keep getting alternating responses from both servers
# Dockerfiles
- Dockerfiles function like shell script as they run from top to bottom
- They're basically "Infrastructure as Code (IaC)" for an image
- Using dockerfiles, we wont need to manually install dependencies on the server, just add the dockerfile into source control and build it automatically
#### Commands
##### Dockerizing GO
- The base image is selected with the `FROM` command
	- `FROM debian:stable-slim`
- You can also make the container run a command upon running with `CMD`
	- `CMD ["echo", "hello world"]`
- Then to build the dockerfile `docker build . -t helloworld:latest`
	- `-t` is used to give images a name and tag, so in our case, the image will be called helloworld:latest, where helloworld is the name and latest is the actual tag
	- This way we can keep track of the image, and its version
- Note, this dockerfile created a container that runs a single command and exists, this is not a running server
- We built a go server but I will cover that later in a go note file instead
- The next command is `COPY` which we used to specify to docker that when building the image, it needs to copy over the binary of the go server
	- Apparently we could have also used `ADD` which has extra functionalities that `COPY` doesn't have
- So the final outcome was
```dockerfile
# Pulls a lightweight debian OS as our base image
FROM debian:stable-slim 
# Copies over the server binary and adds it in /bin/
COPY goserver /bin/goserver
# Runs the server binary upon starting the container
CMD ["/bin/goserver"]
```
- We then built the image with `docker build . -t goserver:latest`
- And created a container with `docker run -p 8010:8010 goserver`
- We can also add environment variables to the image with `ENV`, but we need to do that before running the server
```dockerfile
FROM debian:stable-slim
COPY goserver /bin/goserver
# Sets a port env variable
ENV PORT=8991
CMD ["/bin/goserver"]
```
- The Go server is configured to use the "PORT" variable from the OS environment, so it will use any "PORT" value that we set as its port
##### Dockerizing python
- Unlike GO which was super easy to dockerize since it's a compiled language that like `C` just gives us a binary to run, python is interpreted, and we would need the python interpreter as a dependency
- For that we can use `RUN`
	- `RUN` will execute any command passed to it to create a new layer on top of the current image, which is then used in the next step in the Dockerfile
	- This is unlike `CMD` which executes commands once the container is actually running
	- `RUN` can work in two forms:
		- Shell form: `RUN [OPTIONS] <command>`
		- Exec form: `RUN [OPTIONS] ["<command>", ...]`
- The `debian` base image we use doesn't have any package repos, so we have to first use `RUN apt-get update`, then `RUN apt-get install python3 -y` to get python
	- `-y` means assume yes to all prompts
	- Also, we could have also said
```dockerfile
# EOT and EOF are identical, you can even say BANANA, as long as the label after << matches the end then you're good
RUN <<EOT
apt-get update
apt-get install python3 -y
EOT
```
- The Problem with the above method though is that docker can actually cache repeated commands, but here it will cache the update and install under the same key, so if you want to change the `apt-get install` part, you'll also need to redo the `apt-get update` part, so it's best to keep them spearated with two `RUN` commands
- BUUUT, if we separate them like in the above example, that can cause cache poisining
## Final comments
### Alpine
- A note on `Alpine` is that it uses `Musl LibC` which is is a small implementation of the C standard library that a lot of languages, such as Python, Go, JS, and many many other rely on for OS level commands such as `open` and `read`, which contributes to its small size
- However, `Musl LibC` is not compatible with `GLibC` which is what most software actually uses and builds against, such as debian and ubuntu
- This means that apps may end up rebuilding native dependencies from scratch and just be overall slower as a cost for the smaller size
- Due to this, `slim` can be a better alternative to alpine despite its slightly larger size, as it's fully capable of using binaries as is without having to recompile them against a different version of `LibC`
- If using alpine is a must, at least make sure not to use the `--only-binary=:all` flag as it forces docker to use default binaries and not rebuild from source, which as mentioned would cause issues for alpine
- Alpine may also require you to add C's build tools for it to be able to do its required recompiling, and again, build time is larger with alpine than with slim
### Cache
- Remember that docker caches previous steps as long as they remain unchanged
- Introduce a change in a layer though, and you invalidate the cache for all subsequent layers
- This is an important concept, as for a language like JS, you mainly want to copy over your `package.json` and `package-lock.json` files first, then build, then copy the rest of the project
- Doing this means that we wont be rebuilding the entire app everytime a change is introduced to the code as you wont be invalidating everything starting from the very first `COPY` line, so you wont have to rebuild over and over again
### Separate the build stage from the runtime stage
- This wont matter if you only copy already built binaries into the container really, but consider this following docker file
```dockerfile
FROM golang:alpine as builder
WORKDIR /build
COPY . .
RUN go build -o /app .

FROM alpine
COPY --from=builder /app /bin/app
CMD ["/bin/app"]
```
- In the above file, we used golang:alpine to build our app, but instead of keeping it in the final container, we then created another base image, and only copied the built binary inside of it so that we wont also keep Go's toolchain with us needlessly and wasting space
- We could have conserved even more space, by using `FROM scratch` where scratch is a special docker image that basically has nothing, it's completely empty and very small, so no shell, or tools, or anything, but can run a statically linked binary perfectly fine. A dynamically linked binary requiring shared libraries may be another story though
- Although, distroless images are arguably more useful, even if they're larger, because they provide a minimal runtime environment with things that applications commonly need, such as CA certificates and a non-root user, while still omitting shells and most general-purpose OS utilities, that's otherwise missing in scratch
### Stages
- It's possible to have multiple `FROM` statements as seen in the above example, and each one is called a stage
- You can have as many stages as you want, and by default the last one becomes the actual image
- We can actually pick a stage if we want though
```dockerfile
FROM golang:alpine AS builder

FROM alpine AS production
COPY --from=builder /app /app

FROM golang:alpine AS development
COPY --from=builder /app /app
```
```bash
docker build --target production .
```
- or
```bash
docker build --target development .
```
### Digest hash
- Using `docker buildx imagetools inspect node:26-slim` can show the digest hash of the image
- It's usually better to add that digest to the image tag to make sure you're always using the same version, as tags may be made to point at different images
- For example the `latest` tag always point at the latest image, so it's not exactly consistent
```dockerfile
FROM node:26-slim@sha256:4ebb5ace66f15a24c14c492e01a8beeed4fddf970a856109f5126e703e5fe503
```
### Droast
- This is just a good CLI tool that lints dockerfiles, pointing out mistakes, and roasting you for making them along the way 😅
# Debugging
- `docker logs [OPTIONS] CONTAINER` is the command you use to see what's happening in your container as long as it's running in detached mode `-d`
- You can also pass a command to the container to run as you're running it with `sh -c`
	- `docker run -d --name logdate alpine sh -c 'while true; do echo "LOGGING: $(date)"; sleep 1; done'`
	- Here we passed a shell script to the container that will print the current time every second
- Adding the `-f` flag to the logs command lets you follow it in real time, otherwise you only get the most recent logs
- You can also specify the logs you want to see with commands like `--tail`
- You can also check your container's resource utilization `docker stats [OPTIONS] [CONTAINER...]`
- To try this out, we used the "stress-ng" image which artificially uses a lot of resources both CPU and memory
	- `docker run -d --name cpu-stress alexeiled/stress-ng --cpu 2 --timeout 10m`
- Also, note that docker flags end before the image name, so after `alexeiled/stress-ng` these are image specific arguments that we're passing to it so that it can use them to generate the container
- `docker top CONTAINER [ps OPTIONS]` shows the processes running inside a container
	- the `C` column is for CPU usage
- `docker run` also has flags for limiting resource usage in a container
	-  `--memory`: Limit the memory available to the container
	- `--cpus`: Limit the CPU shares available to the container
# Publishing
- You can push a docker image to docker using `docker push USERNAME/image`
- Note that your hub username is your namespace
- Inside your namespace you'll find your repositories whose name should normally be image:tag by convention, so a full name would be namespace/image:tag
- You can also use `docker pull USERNAME/image` to pull an image
- Docker by default gives your image the "latest" tag if you don't specify a tag yourself
- You normally want to add versions using [semantic versioning](https://semver.org) to your images which helps in deployment pipelines as you can then set the production server to always pull the latest version
- Here is an image of boot.dev's deployment pipeline
![[deployment-pipeline.png]]
- By convention, you should use semantic versioning when making new versions of your image or app, but also create a latest image that points to the most up to date version
- As an example from boot.dev's own workflow
```zsh
docker build -t bootdotdev/awesomeimage:5.4.6 -t bootdotdev/awesomeimage:latest .
docker push bootdotdev/awesomeimage --all-tags
```
# The Deployment Process (as an example)
1. The developer (you) writes some new code
2. The developer commits the code to Git
3. The developer pushes a new branch to GitHub
4. The developer opens a [pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests) to the `main` branch
5. A teammate reviews the PR and approves it (if it looks good)
6. The developer merges the pull request
7. Upon merging, an automated script, perhaps a [GitHub action](https://docs.github.com/en/actions), is started
8. The script builds the code (if it's a compiled language)
9. The script builds a new docker image with the latest program
10. The script pushes the new image to Docker Hub
11. The server that runs the containers, perhaps a [Kubernetes](https://kubernetes.io/) cluster, is told there is a new version
12. The k8s cluster pulls down the latest image
13. The k8s cluster shuts down old containers as it spins up new containers of the latest image

# Docker compose
- Not getting into documenting this yet, but, if you want to expose a container's network to your host network, you can add the following parameter under the `services` field
```yml
    extra_hosts:
      - "host.docker.internal:host-gateway"
```
- You may need to make sure whatever port on localhost that you want to reach from the container is listening on all ports, as if it's just listening on `127.0.0.1` then it will only be reachable from localhost