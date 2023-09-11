
build 現有 container 鏡像

```sh
docker commit docker_name new_image_name:tag
```


```sh
docker run --name {容器名稱} -p 11434:1433 {鏡像名稱}
```


```sh
docker tag SOURCE_IMAGE:TAG {harbor ip}/roger/REPOSITORY[:TAG]
docker push {harbor ip}/roger/REPOSITORY:TAG
```