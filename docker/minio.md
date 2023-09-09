# MinIO

```bash
mkdir -p ~/minio/data

docker run \
   -p 9000:9000 \
   -p 9090:9090 \
   --name minio \
   --restart=always \
   -v ~/minio/data:/data \
   -e "MINIO_ROOT_USER=truongtpa" \
   -e "MINIO_ROOT_PASSWORD=truongtpa" \
   quay.io/minio/minio server /data --console-address ":9090"
```
