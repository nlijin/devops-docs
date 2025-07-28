
# 🐳 Docker Cheat Sheet: From Basics to Advanced

---

## 🧩 1. What is Docker?

- A platform that allows you to **package**, **distribute**, and **run applications** in **containers**.
- **Container** = Lightweight, isolated environment with app + dependencies.
- **Analogy:** Like a shipping container for your software – same app runs anywhere.

---

## 📦 2. Core Docker Concepts

| Term          | Description                                                                 |
|---------------|-----------------------------------------------------------------------------|
| Image         | Blueprint for containers; read-only snapshot of app + dependencies.         |
| Container     | Running instance of an image.                                               |
| Dockerfile    | Script with step-by-step instructions to build a Docker image.              |
| Docker Hub    | Public registry to push/pull images.                                        |
| Volume        | Persistent storage for data across container restarts.                      |
| Network       | Docker's way to allow containers to talk to each other and the outside.     |

---

## 🛠 3. Essential Docker Commands

### 📄 Images & Containers
```bash
docker build -t app-name .               
docker images                            
docker rmi <image_id>                    

docker run -d -p 3000:3000 app-name      
docker ps                                
docker stop <container_id>               
docker rm <container_id>                 
```

### 🔄 Volumes
```bash
docker run -v /host/path:/container/path image-name
docker volume create mydata
docker volume ls
```

### 🔍 Inspect & Logs
```bash
docker inspect <container_name>
docker logs <container_name>
```

---

## 🧱 4. Sample Dockerfile (Node.js)

```dockerfile
FROM node:18

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000
CMD ["npm", "start"]
```

---

## 🚀 5. Docker Compose (Multi-container apps)

### docker-compose.yml example:
```yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "3000:3000"
  mongo:
    image: mongo
    ports:
      - "27017:27017"
```

```bash
docker-compose up        
docker-compose down      
```

---

## ⚙️ 6. Advanced Docker: Multi-Stage Builds

### 🔥 Why Multi-Stage?
- Reduce final image size.
- Avoid shipping unnecessary build tools or source code.

### 🧪 Example: React App Build
```dockerfile
FROM node:18 AS builder
WORKDIR /app
COPY . .
RUN npm install && npm run build

FROM nginx:alpine
COPY --from=builder /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### ✅ Benefits:
- Final image contains only optimized static files.
- Smaller, faster, more secure production image.

---

## 🧰 7. Advanced Tools & Concepts

| Feature              | Description                                                             |
|----------------------|-------------------------------------------------------------------------|
| Dockerfile .dockerignore | Prevents unnecessary files from being copied                         |
| Layers               | Each Dockerfile command creates a layer; cache is reused for speed.     |
| ENTRYPOINT vs CMD    | ENTRYPOINT is fixed; CMD is overrideable. Often used together.          |
| Healthchecks         | Define container health logic inside Dockerfile.                        |

```dockerfile
HEALTHCHECK --interval=30s CMD curl -f http://localhost:3000 || exit 1
```

---

## 🧪 8. Debugging Containers

```bash
docker exec -it <container_id> sh     
docker logs <container_id>            
docker inspect <container_id>         
```

---

## 🔒 9. Docker Best Practices

✅ Keep images small (multi-stage builds)  
✅ Use `.dockerignore`  
✅ Use official base images  
✅ Keep Dockerfiles readable  
✅ Pin dependency versions (e.g., `node:18`)  
✅ Don’t store secrets inside images  
