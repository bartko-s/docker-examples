# Docker Examples

Build 
```
docker-compose build
```

Run 
```
docker-compose up
```

Stop
```
docker-compose down
```

Where is docker volumes located
```
/var/lib/docker/volumes/
```

Connect to the running container with shell
```
docker exec -it "container id" bash

// or

docker-compose exec "service-name" bash

// or run as specific user

docker-compose exec --user "user" "service-name" bash
```
