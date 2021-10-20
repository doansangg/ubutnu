-------------Lab 2-----------------
```
server {
		listen 80
		server_name book.com www.book.com;

		location / {
		    proxy_set_header Host $http_host;
		    proxy_pass http://localhost:3000;
		}

		location /api {
		    proxy_pass http://localhost:8000;
		}
}
```
-------------------------------------------
-------------Lab 2 config-----------------
```
version: "3"
services:
  client:
    image: client
    stdin_open: true
    ports: 
      - "3000:3000"
    networks:
      - mern-app
  server:
    image: server 
    ports:
      - "8000:8000"
    networks:
      - mern-app
    depends_on:
      - mongo
  mongo:
    image: mongo:3.6.19-xenial
    ports:
      - "27017:27017"
    networks:
      - mern-app
    volumes:
      - mongo-data:/data/db

networks:
  mern-app:
    driver: bridge
volumes:
  mongo-data:
    driver: local

-------------------------------------------
```
------------Lab 3-----------------

sudo nano /etc/nginx/nginx.conf

upstream backend {
    server localhost:3000;  
    server localhost:3001;
}

server {
    listen 80;
    server_name lb.com www.lb.com;

    location / {
        proxy_set_header Host $http_host;
        proxy_pass http://backend;
    }

    location /api {
        proxy_pass http://localhost:8000;
    }
}

-------------------------------------------
------------Lab 3_v2-----------------
version: "3"
services:
  client_1:
    image: client_1
    stdin_open: true
    ports: 
      - "3000:3000"
    networks:
      - mern-app
  client_2:
    image: client_2
    stdin_open: true
    ports: 
      - "3001:3000"
    networks:
      - mern-app
  server:
    image: server 
    ports:
      - "8000:8000"
    networks:
      - mern-app
    depends_on:
      - mongo
  mongo:
    image: mongo:3.6.19-xenial
    ports:
      - "27017:27017"
    networks:
      - mern-app
    volumes:
      - mongo-data:/data/db

networks:
  mern-app:
    driver: bridge
volumes:
  mongo-data:
    driver: local
-----------------------------
-------------------------------------------
