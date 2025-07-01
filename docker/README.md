# docker

## debug
- view stats `docker stats`
- view top 10 CPU processes `ps aux --sort=-pcpu | head -n 10`
- view process threads `ps -fly -T -p <PID>`

## run netstat on all containers

Prints local container IP, name and executes netstat:

```
for id in $(docker ps | awk 'NR>1 {printf("%s ",$1)}'); do echo $id; docker inspect $id | jq '.[] | .NetworkSettings.IPAddress,.Name'; docker exec $id netstat -anp; echo ""; done
```

