
## remove all docker images with filter 

docker rmi $(docker images -q)
docker rmi $(docker images |grep 'imagename')
docker rmi $(docker images | grep "^<none>" | awk "{print $3}")

## docker run with custom entrypoint

docker run -it --entrypoint /bin/bash $IMAGE_NAME -s
 
 ## docker container details
 
docker inspect $CONTAINER_ID
