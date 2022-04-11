# Docker

`docker run busybox echo 'Hello World'`

`docker build -t kubia .`
`docker images`
`docker run --name=kubia-container -p 8080:8080 -d kubia`
`curl localhost:8080`
`docker ps`
`docker inspect kubia-container`
`docker exec -it kubia-container bash`
`ps aux`
`ps aux | grep app.js`
`ls /`
`docker stop kubia-container`
`docker rm kubia-container`
`docker tag kubia nthssss/kubia`
`docker images | head`
`docker login`
`docker push nthssss/kubia`
`docker run -p 8080:8080 -d nthssss/kubia`
