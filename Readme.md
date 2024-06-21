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
# DockerHub image naming
Image_Name=<hub-user>/<repo-name>:<tag>

docker build -t $Image_Name .

# local image check
docker images | grep $Image_Name

# run container locally
docker run -d -p 8080:8080 $Image_Name

# check running container
docker ps | grep $Image_Name

# check with curl or browser page
curl localhost:8080

```

## Check image vulnerabilities with Scout

```sh
docker scout quickview $Image_Name
docker scout cves $Image_Name
```

## Docker push or pull images from DockerHub repository

```sh
# image repository (<repo-name>) must be created at DockerHub here
# https://hub.docker.com/repositories/<hub-user>

# push tagged image to DockerHub
docker push $Image_Name

# docker pull image from DockerHub repo
docker pull $Image_Name
# or 
docker run $Image_Name
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
## Deploy service to local k8s

```sh
# switch to needed context ( docker-desktop example )
kubectl config use-context docker-desktop

# deploy 3 pods and expose service on :30001 port
kubectl apply -f ./k8s/deploy.yaml

# service healthcheck with curl or open UI on default browser tab
curl -ILfSs http://localhost:30001
```