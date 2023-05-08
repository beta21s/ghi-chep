# Nginx

### Nginx Proxy Pass

#### Jupyter Notebook

```bash
server {
        listen 80;
        server_name notebook.vlute.edu.vn;
        location / {
                proxy_pass http://172.20.200.151:8848;
                
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection "upgrade";
                proxy_read_timeout 86400;
        }

        client_max_body_size 50M;
        error_log /var/log/nginx/error.log;

        location ~* /(api/kernels/[^/]+/(channels|iopub|shell|stdin)|terminals/websocket)/? {
                proxy_pass http://172.20.200.151:8848;

                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header Host $host;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_http_version 1.1;
                
                proxy_set_header Upgrade "websocket";
                proxy_set_header Connection "upgrade";
                proxy_read_timeout 86400;
        }
}

```

#### VMware vSphere

```
server
{
	listen 80;
	server_name vcenter.vlute.edu.vn;
	rewrite ^(.*) https://vcenter.vlute.edu.vn$1 permanent;
}

server
{
	listen 443 ssl;
	server_name vcenter.vlute.edu.vn;
	ssl_certificate /etc/letsencrypt/live/vcenter.vlute.edu.vn/fullchain.pem;
	ssl_certificate_key /etc/letsencrypt/live/vcenter.vlute.edu.vn/privkey.pem;

	location /
	{
		proxy_set_header Host "172.20.71.71";
		proxy_set_header Origin "172.20.71.71";
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-Server $host;
		proxy_set_header X-Forwarded-Proto $scheme;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header Authorization "";
		proxy_pass_header X-XSRF-TOKEN;
		proxy_ssl_verify off;
		proxy_pass https://172.20.71.71/;
		proxy_http_version 1.1;
		proxy_set_header Upgrade $http_upgrade;
		proxy_set_header Connection "Upgrade";
		proxy_buffering off;
		proxy_send_timeout 300;
		proxy_read_timeout 300;
		send_timeout 300;
		client_max_body_size 1000m;
		proxy_redirect https://172.20.71.71/ https://vcenter.vlute.edu.vn/;
	}

	location /websso/SAML2
	{
		sub_filter "172.20.71.71" "vcenter.vlute.edu.vn";
		proxy_set_header Host 172.20.71.71;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_ssl_verify off;
		proxy_pass https://172.20.71.71;
		proxy_http_version 1.1;
		proxy_set_header Upgrade $http_upgrade;
		proxy_set_header Connection "upgrade";
		proxy_buffering off;
		client_max_body_size 0;
		proxy_read_timeout 36000s;
		proxy_ssl_session_reuse on;
		proxy_redirect https://172.20.71.71/ https://vcenter.vlute.edu.vn/;
	}

	# wss://vsphere.company.dev/ui/app-fabric/fabric
	location /ui/app-fabric/fabric
	{
		proxy_pass https://172.20.71.71/ui/app-fabric/fabric;
		proxy_http_version 1.1;
		proxy_set_header Upgrade $http_upgrade;
		proxy_set_header Connection "upgrade";
		proxy_set_header Host $host;
	}

	# wss://vsphere.company.dev/ui/webconsole/authd
	location /ui/webconsole/authd
	{
		proxy_pass https://172.20.71.71/ui/webconsole/authd;
		proxy_http_version 1.1;
		proxy_set_header Upgrade $http_upgrade;
		proxy_set_header Connection "upgrade";
		proxy_set_header Host $host;
	}

}
```

#### VMware ESXI

```
server {
	listen 80;
	server_name cloud.vlute.edu.vn;
	rewrite ^(.*) https://cloud.vlute.edu.vn$1 permanent;
}

server {
	listen 443 ssl;
    server_name cloud.vlute.edu.vn;
    ssl_certificate /etc/letsencrypt/live/cloud.vlute.edu.vn/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/cloud.vlute.edu.vn/privkey.pem;

	if ($host != "cloud.vlute.edu.vn") {
 		return 404;
	}

    location / {
		allow all;
		proxy_set_header Host $host;
		proxy_set_header Connection "upgrade";
		proxy_set_header Connection $http_connection;
		proxy_pass https://172.20.70.70;
		proxy_http_version 1.1;
		proxy_set_header Upgrade $http_upgrade;
	}

    location /icpc {
		allow all;
		proxy_set_header Host $host;
		proxy_set_header Connection "upgrade";
		proxy_set_header Connection $http_connection;
		proxy_pass https://172.20.70.71;
		proxy_http_version 1.1;
		proxy_set_header Upgrade $http_upgrade;
	}

}
```
