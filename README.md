## Deploy static web application

1. Setup source code: `yarn run create-react-app <source_name>`
2. Create Dockerfile

```
FROM node:latest AS build          #

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm run build

FROM nginx:latest

COPY --from=build /app/build /var/www/html

COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 80

CMD ["/usr/sbin/nginx", "-g", "daemon off;"]

```

3. Create `nginx.conf` file

```
http {
  include mime.types;

  limit_req_zone          $binary_remote_addr zone=mylimit:10m rate=10r/s;

  server {
    listen 80;
    listen [::]:80;
    server_name _;
    root /proxy;
    limit_req zone=mylimit burst=70 nodelay;

    location / {
            root   /var/www/html;
            index  index.html index.htm;
            try_files $uri /index.html;
    }
  }
}

events {}
```

4. ssh server and clone source
5. Installing:

- nginx: https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-20-04#step-5-setting-up-server-blocks-recommended
- docker: https://docs.docker.com/engine/install/ubuntu/

6. Build image: `docker build -t <image_name>:<tag> .`
7. Check images: `docker images`
8. Run docker container from image: `docker run -d -p 80:80 --name <container_name> <image_name>`
9. Open browser: `IP_address:<port>`
