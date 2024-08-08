# Docker

This is my first learning space to learn about docker, and specifically using **docker desktop** for projects at home.

## Day 1 `08/06/2024`

* Went over the `What is a container?` segment in the docker desktop learning senter`
* It's neat that I can see my docker container through the terminal `docker container ls`, I wonder if I can kick it off locally too?
  * `docker run <Container ID>` In docker desktop i sat that it created a new container named "naughty_easey", I'm guessing that this is a randomly generated name. I also see that docker desktop named the images since theyre name and image id are different. Also when I ran my terminal thread was caught on it, so I `ctrl+c` in order to get out, and it also exited the docker container, so it isn't up and running any more.

***

## Day 2 `08/07/2024`

* What is a container?
* How do I run a container?
* Run Docker Hub images
* Multi-container applications
* Persist your data between containers
* Access your local folder from a container
  * Bind Mount Applications
* Co-pilot explanations.

I've been going over a lot of docker information. Copilot gave me a bunch of information. I mostly didn't know about npm, how it manages dependencies and what installing serve/running it does.
I got a quick run down of that and asked it bunch of questions about connections. In short you should be using you DockerFule to set environment variables that will be used in the helm-chart or docker compose to be passed down to the container. For example doing:

``` docker
ARG PORT=XXX
ENV PORT=$PORT
EXPOSE $PORT
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "$PORT"]
```

Then in the docker build command add `--build-arg PORT=3000` to set the environment variable.

Also you should be using Secrets in K8 for anything sensitive.

Learning about docker compose I asked the question to copilot `"Why are the port numbers different between containers, wouldn't they need one open on both containers to connect to each other?"`

I then got the answer:
___
Great question! The port numbers you see in the `docker-compose.yaml` file are actually **port mappings**. They map a network port on the host (your machine) to a network port in the container. The format is `<host-port>:<container-port>`. 

In the context of your `docker-compose.yaml` file, for the `todo-app` service, the ports `3000` and `35729` on your machine are mapped to the same ports inside the `todo-app` container. Similarly, for the `todo-database` service, port `27017` on your machine is mapped to the same port inside the `todo-database` container.

These mappings allow you to access the services running inside the containers from your machine. For example, if your `todo-app` is a web server listening on port `3000`, you could open a web browser on your machine and navigate to `localhost:3000` to access it.

However, **containers on the same Docker network can communicate with each other without needing to map ports**. In your `docker-compose.yaml` file, the `todo-app` service has a `depends_on` field with a value of `- todo-database`, which means Docker will ensure the `todo-database` service is started before the `todo-app` service. Once both services are running, they can communicate with each other using the service names as hostnames and the internal port numbers. For example, the `todo-app` could connect to the `todo-database` using `todo-database:27017`.
___

I then went ahead and ran `docker compose up -d` to run this application. NOTE: `-d` is for detached mode.

There was also the command `docker compose watch` which was supposed to allow you to automatically update and preview your running service as you edit and save the code. The issue was that I wasn't able to see this functionality.

It might have been that I need to reload my firefox screen. I was used to .vue forcing a reload on the screen whenever there was a change.

### Publishing an Image 

When publishing an image you need to rename/retag it.

``` bash
docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```

Example:

``` bash
docker tag docker/welcome-to-docker YOUR-USERNAME/welcome-to-docker
```

Then go to `docker desktop`, go to your image and select `Push to Hub` in the drop-down.