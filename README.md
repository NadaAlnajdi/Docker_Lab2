# Docker_Lab2

# ITI - Docker Lab 🐋

## Task 1: Run a container using nginx image, and mount a directory from your host into the Docker container.

### Steps
#### 1. Create Bind Mount Directory
```bash
mkdir nginx_bindMount
cd nginx_bindMount
pwd
```
#### 2. Run a container using nginx image
```bash
docker run -d --name nginx_bindMount -v /root/nginx_bindMount:/user/share/nginx/html nginx
docker exec -it nginx_bindMount bash
```
#### 3. Echo any content to show when curl ip-address

```bash
cd /user/share/nginx/html
echo "Hello from Bind Mount" > index.html
exit
docker inspect -f 172.17.0.2 nginx_bindMount 
curl 172.17.0.2
```

---

## Task 2: Create 2 docker network (net-1 & net-2)

### Steps
#### 1. Create 2 docker network (net-1 & net-2)
```bash
docker network create network_1
docker network create network_2
```
#### 2. Run 2 new containers using nginx:alpine image, and attach the net-1 to them.

```bash
docker run -d --name nginx_net1 --net network_1 nginx:alpine
docker run -d --name nginx_net2 --network network_1 nginx:alpine
```
#### 3.Run another 1 new containers using nginx:alpine image, and attach the net-2 to them.
```bash
docker run -d --name nginx_net3 --net network_2 nginx:alpine
```
#### 4. Inspect the 3 containers to know their IPs and write them aside.
```bash
docker inspect -f '{{.NetworkSettings.Networks.network_1.IPAddress}}' nginx_net1


docker inspect -f '{{.NetworkSettings.Networks.network_1.IPAddress}}' nginx_net2
172.18.0.3

docker inspect -f '{{.NetworkSettings.Networks.network_2.IPAddress}}' nginx_net3
172.20.0.2

```
#### 5. Enter a container in the net-1 network and try to ping a container in the net-2 network (What do you notice?)
```bash
docker exec -it nginx_net1 sh 
ping 172.20.0.2
ping fails, because the two containers exist in different networks so we can see any response in terminal.
```
#### 6. Enter a container in the net-1 network and try to ping the other container in the same network (What do you notice?)
```bash
docker exec -it nginx_net1 sh 
curl 172.18.0.2
ping 172.18.0.3
ping successful and return response
```

---
## Task 3:Explain the difference between Docker volumes and Bind Mount.


#### 1.Docker Volumes
```bash
● It is stored within a directory on the Docker host. ● Managed by Docker and are isolated from the core functionality of the host machine. ● Docker volumes can be named and managed independently of containers ● Can be mounted into multiple containers simultaneously. ● Deleting a container does not delete the volume. ● Docker provides commands (docker volume create, docker volume ls, etc.) to manage volumes.

```
#### 2.Docker Bind Mounts
```bash
● Bind mounts have limited functionality compared to volumes. ● Bind mounts are linked to a specific directory or file on the Docker host filesystem. ● The file or directory is referenced by its full path on the host machine. ● When you use a bind mount, a file or directory on the host is mounted into the container, and allowing the container to access the host's filesystem. ● It is created on demand if it does not yet exist. ● You can’t use Docker CLI commands to directly manage bind mounts. ● Available since the early days of Docker.
```


