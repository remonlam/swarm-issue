# Swarm on ARM

## Networking
### Create TEST network
```shell
docker network create \
  --driver overlay \
  --subnet 10.100.1.0/24 \
  test
```

### Create NGINX network
```shell
docker network create \
  --driver overlay \
  --subnet 10.100.2.0/24 \
  nginx
```

## Services
## NGINX Service
```bash
docker service create \
  --name nginx \
  --detach=true \
  --network=nginx \
  --publish 80:80/tcp \
  containerstack/nginx-arm
  ```

  ## Test Service
```bash
docker service create \
  --name test \
  --replicas=2 \
  --detach=true \
  --network=nginx \
  --network=test \
  containerstack/nginx-arm
```

## Get NGINX VIP
```bash
docker service inspect nginx

...
"VirtualIPs": [
                {
                    "NetworkID": "qa7qtw9vxfdzooacg0c1kwjci",
                    "Addr": "10.255.0.7/16"
                },
                {
                    "NetworkID": "i87x9c6eftevj9jmax0g6k93v",
                    "Addr": "10.100.2.2/24"
                }
            ]
```
The VIP address of the NGINX service is 10.100.2.2

So from for example the TEST container I would expect a response when I do a "curl http://10.100.2.2" but all I see is connection refused...
