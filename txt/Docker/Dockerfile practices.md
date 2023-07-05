# Dockerfile practices

## .dockerignore file

Create a `.dockerignore` file in the same directory as your `Dockerfile` with following content:

```
dist
node_modules
npm-debug.log
```

This will prevent your local modules and debug logs from being copied onto your Docker image and possibly overwriting modules installed within your image.

## Environment Variables

Run with `NODE_ENV` set to `production`. This is the way you would pass in secrets and other runtime configurations to your application as well.

```shell
-e "NODE_ENV=production"
```

## Handling Kernel Signals

Node.js was not designed to run as PID 1 which leads to unexpected behaviour when running inside of Docker. For example, a Node.js process running as PID 1 will not respond to `SIGINT` (`CTRL-C`) and similar signals. As of Docker 1.13, you can use the `--init` flag to wrap your Node.js process with a [lightweight init system](https://github.com/krallin/tini) that properly handles running as PID 1.

```shell
docker run -it --init node
```

## Non-root user running the app

By default, Docker runs commands inside the container as root which violates the [Principle of Least Privilege (PoLP)](https://en.wikipedia.org/wiki/Principle_of_least_privilege) when superuser permissions are not strictly required. You want to run the container as an unprivileged user whenever possible. The node images provide the `node` user for such purpose. The Docker Image can then be run with the `node` user in the following way:

```
-u "node"
```

Alternatively, the user can be activated in the `Dockerfile`:

```Dockerfile
# At the end, set the non-root user to use when running this image
USER node
```

## `CMD`

When creating an image, you can bypass the `package.json`'s `start` command and bake it directly into the image itself. First off this reduces the number of processes running inside of your container. Secondly it causes exit signals such as `SIGTERM` and `SIGINT` to be received by the Node.js process instead of npm swallowing them.

```dockerfile
CMD ["node","index.js"]
```

## Build performance

Docker has the concept of layers that can be cached. There are 3 commands that create layers:
- `ADD`
- `COPY`
- `RUN`

Each of those commands changes the image's filesystem so a kind of snapshot is taken when they finish. This is called a layer and it is a delta between the filesystem state of previous layer the state after execution of a command.

Image build process is cached and if something changes in the project the cache must be invalidated. But each layer is cached separately and only needs to be invalidated if something changes for that layer:
- for `ADD` and `COPY` the filesystem is checked. Namely files' checksums but not created/modified dates
- for `RUN` command only the command definition is checked

However, since layers are filesystem deltas, once a layer is changed, not only filesystem state at this stage is affected but also at all the later stages. Thus, if something changes in one layer, both its cache and caches of later layers get invalidated.

It's important to create layers in such a way that most common development changes affected the least number of layers. Commonly the order is:
1. Install tools you need to build your application (`apt-get install`)
2. Install or update library dependencies
3. Generate your application

## Image performance

Keep the image size lowest possible. To do so:

### Do not install unnecessary packages

When you avoid installing extra or unnecessary packages, you images will have reduced complexity, reduced dependencies, reduced file sizes, and reduced build times.

### Use multi-stage builds

Where possible, use [multi-stage builds](https://docs.docker.com/build/building/multi-stage/), and only copy the artifacts you need into the final image. This allows you to include tools and debug information in your intermediate build stages without increasing the size of the final image.

```Dockerfile
FROM golang:1.16-alpine AS build
...

# This results in a single layer image
FROM scratch
COPY --from=build . .
```

## The good template

```Dockerfile
FROM node:18 AS build

# Install the Linux package dependencies, if needed
# node:18 image comes with Node.js and NPM already installed
RUN apt-get update
RUN apt-get -y install python-software-properties git build-essential

# Create app directory and cd into it
WORKDIR /usr/src/app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
COPY package*.json ./

RUN npm install
# If you are building your code for production
# RUN npm ci --omit=dev

# Bundle app source
COPY . .

# At the end, set the non-root user to use when running this image
USER node

RUN npm run lint && npm run build

FROM node:18-alpine
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install --production

COPY --from=build ./usr/src/app/dist ./usr/src/app

EXPOSE 8080
CMD [ "node", "index.js" ]
```

