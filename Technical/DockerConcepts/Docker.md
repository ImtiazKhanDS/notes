---
id: Docker
aliases: []
tags: []
---

docker container run diamol/ch02-hello-diamol

![[Pasted_image_20240517055158.png]]

![[Pasted image 20240517055301.png]]

docker container ls
docker container ls --all

![[Pasted image 20240525135346.png]]

docker container stats e53
docker container rm --force $(docker container ls --all --quiet)

**docker image pull diamol/ch03-web-ping**

![[Pasted image 20240526154234.png]]

docker container run -d --name web-ping diamol/ch03-web-ping

The `-d` flag is a short form of `--detach`, so this container will run in the background.

docker container logs web-ping

![[Pasted image 20240526225014.png]]

docker rm -f web-ping 
docker container run --env TARGET=google.com diamol/ch03-web-ping

![[Pasted image 20240526225230.png]]

![[Pasted image 20240526225521.png]]

The Dockerfile is a simple script you write to package up an application--it’s a set of instructions, and a Docker image is the output. Dockerfile syntax is simple to learn, and you can package up any kind of app using a Dockerfile.

The web-ping Dockerfile

`FROM diamol/node` 
 `ENV TARGET="blog.sixeyed.com"`
  `ENV METHOD="HEAD"` 
  `ENV INTERVAL="3000"
  WORKDIR /web-ping` `
  COPY app.js .` `
  CMD ["node", "/web-ping/app.js"]`

- `FROM` --Every image has to start from another image. In this case, the `web-ping`image will use the `diamol/node` image as its starting point. That image has Node.js installed, which is everything the web-ping application needs to run.
- `ENV` --Sets values for environment variables. The syntax is `[key]="[value]"`, and there are three `ENV`instructions here, setting up three different environment variables.
- `WORKDIR` --Creates a directory in the container image filesystem, and sets that to be the current working directory. The forward-slash syntax works for Linux and Windows containers, so this will create `/web-ping` on Linux and `C:\web-ping` on Windows.
- `COPY` --Copies files or directories from the local filesystem into the container image. The syntax is `[source` `path]``[target` `path]` --in this case, I’m copying app.js from my local machine into the working directory in the image.
- `CMD` --Specifies the command to run when Docker starts a container from the image. This runs Node.js, starting the application code in app.js.

![[Pasted image 20240526231013.png]]

Turn this Dockerfile into a Docker image by running `docker` `image` `build` :

`docker image build --tag web-ping .`

The `--tag` argument is the name for the image, and the final argument is the directory where the Dockerfile and related files are. Docker calls this directory the “context,” and the period means “use the current directory.” You’ll see output from the `build`command, executing all the instructions in the Dockerfile.

![[Pasted image 20240527215828.png]]

List all the images where the tag name starts with “w”:

`docker image ls 'w*'`

You’ll see your web-ping image listed:

`> docker image ls w*` `REPOSITORY TAG    IMAGE ID     CREATED        SIZE` `web-ping   latest f2a5c430ab2a 14 minutes ago 75.3MB`

Run a container from your own image to ping Docker’s website every five seconds:

`docker container run -e TARGET=docker.com -e INTERVAL=5000 web-ping`

![[Pasted image 20240528074903.png]]

![[Pasted image 20240530061112.png]]

docker image ls
![[Pasted image 20240601224616.png]]

docker system df

![[Pasted image 20240601224646.png]]

`docker image build -t web-ping:v2 .`

![[Pasted image 20240601224851.png]]

Any Dockerfile you write should be optimized so that the instructions are ordered by how frequently they change--with instructions that are unlikely to change at the start of the Dockerfile, and instructions most likely to change at the end. The goal is for most builds to only need to execute the last instruction, using the cache for everything else. That saves time, disk space, and network bandwidth when you start sharing your images.

![[Pasted image 20240602054908.png]]

![[Pasted image 20240602055102.png]]

Containers access each other across a virtual network, using the virtual IP address that Docker allocates when it creates the container. You can create and manage virtual Docker networks through the command line.

**docker network create nat**

Now when you run containers you can explicitly connect them to that Docker network using the `--network` flag, and any containers on that network can reach each other using the container names.

Run a container from the image, publishing port 80 to the host computer, and connecting to the `nat` network:

`docker container run --name iotd -d -p 800:80 --network nat image-of-the-day`

Dockerfile for building a Node.js app with npm

`FROM diamol/node AS builder`  
 `WORKDIR /src` 
 `COPY src/package.json .`  
 `RUN npm install`  `# app` `FROM diamol/node`  `EXPOSE 80` `CMD ["node", "server.js"]`  `WORKDIR /app` `COPY --from=builder /src/node_modules/ /app/node_modules/` `COPY src/ .`

![[Pasted image 20240604074055.png]]

`cd ch04/exercises/access-log` 
`docker image build -t access-log .`

docker container run --name accesslog -d -p 801:80 --network nat access-log

![[Pasted image 20240626142524.png]]

