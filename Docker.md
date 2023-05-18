# Docker

Install Docker

```
curl https://get.docker.com | sh \
  && sudo systemctl --now enable docker
```

Install Docker compose

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

Install Portainer CE with Docker



Upload một image lên Docker Hub

```
docker login
docker image ls
docker tag <spark:spark-docker> <truong96/spark-k8s:tagname>
docker push <truong96/spark-k8s:tagname>
```
