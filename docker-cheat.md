docker rmi $(docker images -q)

docker rmi $(docker images |grep 'imagename')
docker rmi $(docker images | grep "^<none>" | awk "{print $3}")

docker run -it --entrypoint /bin/bash $IMAGE_NAME -s
 
docker inspect $CONTAINER_ID
