# docker_lab2
lab2
# ITI - Docker Lab Two🐋
## Task 1: Run a container using nginx image, and mount a directory from your host into the Docker container.
example: /home/samy/nginx:/home/nginx (bind mount)

### Steps
#### 1. Create Bind Mount Directory
```bash
mkdir main_dir

```

#### 2. Run a container using nginx image
```bash
docker run -d --name main_dir-v /root/main_dir:/user/share/nginx/html nginx
docker exec -it main_dir bash
```

#### 3. Echo any content to show when curl ip-address
```bash
cd /user/share/nginx/html
echo "Hello i salma" > index.html
exit
docker inspect -f '{{.NetworkSettings.IPAddress}}' main_dir  ->172.17.0.7
curl 172.17.0.7
```

---
## Task 2: Create 2 docker network (net-1 & net-2)

### Steps
#### 1. Create 2 docker network (net-1 & net-2)
```bash
docker network create mainNetwork1
docker network create mainNetwork2
```

#### 2. Run 2 new containers using nginx:alpine image, and attach the net-1 to them.
```bash
docker run -d --name nginx_mainNetwork1 --net mainNetwork1 nginx:alpine
docker run -d --name nginx_mainNetwork2 --net mainNetwork2 nginx:alpine
```

#### 3. Run another 1 new containers using nginx:alpine image, and attach the net-2 to them.
```bash
docker run -d --name nginx_mainNetwork3 --net mainNetwork2 nginx:alpine
```

#### 4. Inspect the 3 containers to know their IPs and write them aside.
```bash
docker inspect -f '{{.NetworkSettings.Networks.network_1.IPAddress}}' nginx_network1
172.18.0.2

docker inspect -f '{{.NetworkSettings.Networks.network_1.IPAddress}}' nginx_network2
172.18.0.3

docker inspect -f '{{.NetworkSettings.Networks.network_2.IPAddress}}' nginx_network3
172.20.0.2
 docker inspect nginx_mainNetwork1 | grep IPAddress
```

#### 5. Enter a container in the net-1 network and try to ping a container in the net-2 network (What do you notice?)
```bash
docker exec -it nginx_mainNetwork1 sh 
Use ping or curl
ping 172.20.0.2
ping both in same network dosent work
```

#### 6. Enter a container in the net-1 network and try to ping the other container in the same network (What do you notice?)
```bash
docker exec -it nginx_network1 sh 
curl 172.18.0.2
ping 172.18.0.3
ping both in same network work
```

## Task 3: Explain the difference between Docker volumes and Bind Mount.
Docker volumes and bind mounts serve similar purposes in allowing containers to access files and directories outside of their own filesystems, but they differ in how they achieve this and their intended use cases.
