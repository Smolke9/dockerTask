
# 🐳 Docker Task 2 - Suraj Balaji Molke

> Batch: 7th Feb AWS | Author: Suraj Molke
> FortuneCloud Tech Shivaji Nagar Pune

This project demonstrates various Docker concepts such as Docker volumes, networks, Dockerfile usage, and pushing images to Docker Hub.

---

## 📁 Project Contents

- Docker Volume Example with Nginx
- Custom Docker Network with Nginx & Apache
- Dockerfile for Node.js App
- Dockerfile for Python App & Docker Hub Push
- Screenshots included for verification

---

## 🔸 1. Docker Volumes

Docker volumes are used to store persistent data outside of a container's writable layer.

### 📦 Types of Docker Volumes

| Type             | Description                                                                 |
|------------------|-----------------------------------------------------------------------------|
| Named Volumes    | Created and managed by Docker.                                              |
| Anonymous Volumes| Automatically created. Not easy to refer later.                             |
| Bind Mounts      | Maps host files/directories. Full access to host file system.               |
| tmpfs            | In-memory temporary data, deleted when container stops.                     |

### ✅ Volume Demo Steps

```bash
docker volume create mydata

docker run -d --name mynginx -v mydata:/usr/share/nginx/html -p 8080:80 nginx

echo "Hello, Docker Volumes!" > index.html

sudo cp index.html /var/lib/docker/volumes/mydata/_data/

curl http://localhost:8080
```

---

## 🔸 2. Docker Networks

Docker networks enable containers to talk to each other.

| Feature          | Host Network           | Bridge Network        |
|------------------|------------------------|------------------------|
| Isolation        | Less isolated          | More isolated          |
| Port Mapping     | Not needed             | Required               |
| Use Case         | High-performance apps  | General app usage      |

### ✅ Custom Network Example

```bash
docker network create --driver bridge my_network

docker run -d --name apache-server --network my_network httpd

docker run -d --name nginx-server --network my_network nginx

docker exec -it nginx-server bash
curl http://apache-server
```

---

## 🔸 3. Dockerfile for Node.js App

### 📄 Dockerfile

```dockerfile
FROM node:18
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "index.js"]
```

### 📄 docker-compose.yml

```yaml
version: '3.8'
services:
  nodeapp:
    build: .
    container_name: node-container
    ports:
      - "3000:3000"
    volumes:
      - node_data:/usr/src/app/data
    networks:
      - node_network

volumes:
  node_data:

networks:
  node_network:
    driver: bridge
```

Run:

```bash
docker-compose up --build -d
```

---

## 🔸 4. Dockerfile for Python App + Docker Hub Push

### 📄 app.py

```python
print("Hello from Suraj’s Python Docker app!")
```

### 📄 requirements.txt

```
requests
```

### 📄 Dockerfile

```dockerfile
FROM python:3.10-slim
WORKDIR /app
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["python", "app.py"]
```

### 🔼 Push to Docker Hub

```bash
docker build -t surajmolke/pythonimage .
docker login
docker push surajmolke/pythonimage
```

---

## 🖼️ Screenshots

Place your screenshots in a folder named `/screenshots` and reference them like this:

```markdown
![Volume Demo](screenshots/1.png)
![Custom Network](screenshots/2.png)
![Node App](screenshots/3.png)
![Python Image on Docker Hub](screenshots/4.png)
```

---


## 👨‍💻 Author

**Suraj Balaji Molke**  
Batch: 7th Feb AWS  
GitHub: [github.com/surajmolke](https://github.com/surajmolke)

---
