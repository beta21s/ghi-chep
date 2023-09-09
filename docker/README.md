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

```bash
docker volume create portainerdata
```

{% code overflow="wrap" %}
```bash
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainerdata:/data portainer/portainer-ce:latest
```
{% endcode %}

Upload một image lên Docker Hub

```
docker login
docker image ls
docker tag <spark:spark-docker> <truong96/spark-k8s:tagname>
docker push <truong96/spark-k8s:tagname>
```
