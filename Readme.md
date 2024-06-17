# Simple GoLang server

## Local code build and test

```sh
# Build Go binary for the service
go build -o bin/app src/main.go

# run web server locally
./bin/app

# open second terminal session check (or in new browser tab):
curl localhost:8080
```

## local Docker authentication to DockerHub

```sh
docker login -u <hub-user>

password: <generated_token_on_DockerHub>
```

## Docker build/tag, run

```sh
docker build -t <hub-user>/<repo-name>:<tag> .

# local image check
docker images | grep <hub-user>/<repo-name>:<tag>

# run container locally
docker run -p 8060:8080 <hub-user>/<repo-name>:<tag>

# check running container
docker ps | grep <hub-user>/<repo-name>:<tag>

# check with curl or browser page
curl localhost:8060

```

## Check image vulnerabilities with Scout

```sh
docker scout quickview <hub-user>/<repo-name>:<tag>
docker scout cves <hub-user>/<repo-name>:<tag>
```

## Docker push or pull images from DockerHub repository

```sh
# image repository (<repo-name>) must be created at DockerHub here
# https://hub.docker.com/repositories/<hub-user>

# push tagged image to DockerHub
docker push <hub-user>/<repo-name>:<tag>

# docker pull image from DockerHub repo
docker pull <hub-user>/<repo-name>:<tag>
# or 
docker run <hub-user>/<repo-name>:<tag>
```

### Docker clean up

```sh
# delete all containers including its volumes
docker rm -vf $(docker ps -aq)

# delete all the images
docker rmi -f $(docker images -aq)

# WARNING! This will remove:
#   - all stopped containers
#   - all networks not used by at least one container
#   - all anonymous volumes not used by at least one container
#   - all images without at least one container associated to them
#   - all build cache
docker system prune -a --volumes
```
