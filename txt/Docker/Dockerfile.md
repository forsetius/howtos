# Dockerfile

```Dockerfile
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
EXPOSE 3000
```

- `FROM` - an image to start from
- `WORKDIR` - working directory
- `COPY` - 
- `RUN` - execute commands when starting
- `CMD` - default command to run when starting a container from this image
- `EXPOSE` - 

### Forms

Various commands can have alternative forms:

#### Shell form

```Dockerfile
RUN <command> 

# Example:
RUN yarn install \ 
  --production
```

The command is run in a shell, which by default is `/bin/sh -c`. It can be changed using the `SHELL` command. In the _shell_ form you can use a `\` (backslash) to continue a single RUN instruction onto the next line.

#### Exec form

```Dockerfile
RUN ["executable", "param1", "param2"]

# Example
RUN ["/bin/bash", "-c", "echo hello"]
```

To use a different shell, other than `/bin/sh`, use the _exec_ form passing in the desired shell, as in the example above. The _exec_ form is parsed as a JSON array, which means that you must use double-quotes (`"`) around words not single-quotes (`'`). Also, in the _JSON_ form, it is necessary to escape backslashes.

Unlike the _shell_ form, the _exec_ form does not invoke a command shell. This means that normal shell processing does not happen. For example, `RUN [ "echo", "$HOME" ]` will not do variable substitution on `$HOME`. If you want shell processing then either use the _shell_ form or execute a shell directly, for example: `RUN [ "sh", "-c", "echo $HOME" ]`. When using the exec form and executing a shell directly, as in the case for the shell form, it is the shell that is doing the environment variable expansion, not docker.


## `CMD`

```Dockerfile
CMD ["executable","param1","param2"]  # (exec form, preferred)
CMD ["param1","param2"]               # as default parameters to ENTRYPOINT
CMD command param1 param2             # shell form
```

The main purpose of a `CMD` is to provide defaults for an executing container. 

> [!Example]
> ```Dockerfile
> CMD echo "This is a test." | wc -   # shell form
> CMD ["/usr/bin/wc","--help"]        # exec form
> ```

> [!Important]
Do not confuse `RUN` with `CMD`. `RUN` actually runs a command and commits the result; `CMD` does not execute anything at build time, but specifies the intended command for the image.

> [!Warning]
> There can only be one `CMD` instruction in a `Dockerfile`. If you list more than one `CMD` then only the last `CMD` will take effect.

These defaults can include an executable, or they can omit the executable, in which case you must specify an `ENTRYPOINT` instruction as well. If `CMD` is used to provide default arguments for the `ENTRYPOINT` instruction, both the `CMD` and `ENTRYPOINT` instructions should be specified with the JSON array format.

> [!Warning]
If the user specifies arguments to `docker run` then they will override the default specified in `CMD`.

## `COPY`

```Dockerfile
COPY [--chown=<user>:<group>] [--chmod=<perms>] <src>... <dest>
```

The `COPY` instruction copies new files or directories from `<src>` and adds them to the filesystem of the container at the path `<dest>`.

## `FROM`

```Dockerfile
FROM [--platform=<platform>] <image>[:<tag>] [AS <name>]
```

The `FROM` instruction initializes a new build stage and sets the base Image for subsequent instructions. As such, **a valid `Dockerfile` must start with a `FROM` instruction**.

`FROM` can appear multiple times within a single `Dockerfile` to create multiple images or use one build stage as a dependency for another. Simply make a note of the last image ID output by the commit before each new `FROM` instruction. Each `FROM` instruction clears any state created by previous instructions.

Optionally a name can be given to a new build stage by adding `AS name` to the `FROM` instruction. The name can be used in subsequent `FROM` and `COPY --from=<name>` instructions to refer to the image built in this stage.

## `EXPOSE`

```Dockerfile
EXPOSE <port> [<port>/<protocol>...]
```

The `EXPOSE` instruction informs Docker that the container listens on the specified network ports at runtime. You can specify whether the port listens on TCP or UDP, and the default is TCP if the protocol is not specified.

> [!Example]
> ```Dockerfile
> EXPOSE 80       # means 80/tcp
> EXPOSE 80/udp
> ```

To expose both TCP and UDP, include two lines: with `80/tcp` and `80/udp`.

Regardless of the `EXPOSE` settings, you can override them at runtime by using the `-p` flag.

> [!Example]
> ```shell
> $ docker run -p 80:80/tcp -p 80:80/udp ...
> ```

## `RUN`

```Dockerfile
RUN <command>                           # shell form
RUN ["executable", "param1", "param2"]  # exec form
```

The `RUN` instruction will execute any commands **in a new layer** on top of the current image.

> [!Example]
> ```Dockerfile
> RUN yarn install --production          # shell form
> RUN ["/bin/bash", "-c", "echo hello"]  # exec form
> ```

> [!warning]
The cache for `RUN` instructions isn’t invalidated automatically during the next build. The cache for an instruction like `RUN apt-get dist-upgrade -y` will be reused during the next build. The cache for `RUN` instructions can be invalidated by using the `--no-cache` flag, for example `docker build --no-cache`.
> 
> The cache for `RUN` instructions can be invalidated by `ADD` and `COPY` instructions.

## `WORKDIR`

```Dockerfile
WORKDIR /path/to/workdir
```

The `WORKDIR` instruction sets the working directory for any `RUN`, `CMD`, `ENTRYPOINT`, `COPY` and `ADD` instructions that follow it in the `Dockerfile`. If the `WORKDIR` doesn’t exist, it will be created even if it’s not used in any subsequent `Dockerfile` instruction.

The `WORKDIR` instruction can be used multiple times in a `Dockerfile`. If a relative path is provided, it will be relative to the path of the previous `WORKDIR` instruction. or example:

```Dockerfile
WORKDIR /a
WORKDIR b
WORKDIR c
RUN pwd
```

The output of the final `pwd` command in this `Dockerfile` would be `/a/b/c`.

The `WORKDIR` instruction can resolve environment variables previously set using `ENV`. You can only use environment variables explicitly set in the `Dockerfile`. For example:

```Dockerfile
ENV DIRPATH=/path
WORKDIR $DIRPATH/$DIRNAME
RUN pwd
```

The output of the final `pwd` command in this `Dockerfile` would be `/path/$DIRNAME`

