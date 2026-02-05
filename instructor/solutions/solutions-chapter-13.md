# Chapter 13 Solutions: Docker Containers

## Exercise Solutions

### Exercise 1: Run a Web Server

**Task:** Run nginx in a container.

**Solution:**

```bash
# Pull the nginx image
docker pull nginx
```

**Expected Output:**
```
Using default tag: latest
latest: Pulling from library/nginx
a1b2c3d4e5f6: Pull complete
...
Digest: sha256:abc123...
Status: Downloaded newer image for nginx:latest
```

```bash
# Run nginx in detached mode on port 8080
docker run -d --name my-nginx -p 8080:80 nginx
```

**Expected Output:**
```
a1b2c3d4e5f67890abc123def45678901234567890abcdef123456
```

```bash
# Verify it's running with curl
curl http://localhost:8080
```

**Expected Output:**
```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>
...
</body>
</html>
```

```bash
# View container logs
docker logs my-nginx
```

**Expected Output:**
```
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
...
2025/02/07 10:30:00 [notice] 1#1: start worker process 1
```

```bash
# Stop the container
docker stop my-nginx
```

```bash
# Remove the container
docker rm my-nginx
```

**Explanation:**
- `docker pull`: Download image from registry
- `docker run -d`: Run in detached mode (background)
- `--name`: Give container a name
- `-p 8080:80`: Map host port 8080 to container port 80
- `docker logs`: Show container output
- `docker stop`: Gracefully stop container
- `docker rm`: Delete stopped container

---

### Exercise 2: Build a Custom Image

**Task:** Create a Dockerfile and build image.

**Solution:**

```bash
# Create project directory
mkdir my-web
cd my-web
```

```bash
# Create custom HTML page
cat > index.html << 'EOF'
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>My Docker Web Server</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 50px auto;
            padding: 20px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }
    </style>
</head>
<body>
    <h1>Welcome to My Docker Container!</h1>
    <p>This page is served from an nginx container.</p>
    <p>Built on: $(date)</p>
</body>
</html>
EOF
```

```bash
# Create Dockerfile
cat > Dockerfile << 'EOF'
# Use nginx as base image
FROM nginx:alpine

# Copy custom HTML to nginx html directory
COPY index.html /usr/share/nginx/html/index.html

# Expose port 80
EXPOSE 80
EOF
```

```bash
# Build the image
docker build -t my-web .
```

**Expected Output:**
```
[+] Building 45.2s (10/10) FINISHED
 => [internal] load .dockerignore                   0.0s
 => => transferring context: 2B                     0.0s
 => [1/4] FROM docker.io/library/nginx:alpine      15.3s
 => [2/4] COPY index.html /usr/share/nginx/html/    0.1s
 => [3/4] EXPOSE 80                                  0.0s
 => [4/4] CMD ["nginx" "-g" "daemon off;"]          0.0s
 => exporting to image                               0.1s
 => => naming to docker.io/library/my-web
```

```bash
# Run the container
docker run -d --name my-web-container -p 8081:80 my-web
```

```bash
# Verify your custom page loads
curl http://localhost:8081
```

**Expected Output:**
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>My Docker Web Server</title>
...
    <h1>Welcome to My Docker Container!</h1>
    <p>This page is served from an nginx container.</p>
...
```

```bash
# Cleanup
docker stop my-web-container
docker rm my-web-container
```

**Explanation:**
- `FROM`: Base image to use
- `COPY`: Copy files from host to image
- `EXPOSE`: Document which port container listens on
- `docker build -t name`: Build image with tag
- `nginx:alpine`: Minimal nginx image (smaller size)

---

### Exercise 3: Multi-Container App

**Task:** Use Docker Compose for web app with database.

**Solution:**

```bash
# Create project directory
mkdir my-compose-app
cd my-compose-app
```

```bash
# Create docker-compose.yml
cat > docker-compose.yml << 'EOF'
version: '3.8'

services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
    depends_on:
      - db

  db:
    image: postgres:15-alpine
    environment:
      - POSTGRES_PASSWORD=mysecretpassword
      - POSTGRES_DB=mydb
    volumes:
      - db-data:/var/lib/postgresql/data

volumes:
  db-data:
EOF
```

```bash
# Create HTML content
mkdir html
echo "<h1>Hello from Docker Compose!</h1>" > html/index.html
```

```bash
# Start all services
docker compose up -d
```

**Expected Output:**
```
[+] Running 3/3
 ✔ Network my-compose-app_default     Created
 ✔ Container my-compose-app-db-1      Started
 ✔ Container my-compose-app-web-1     Started
```

```bash
# View running services
docker compose ps
```

**Expected Output:**
```
NAME                    COMMAND                  SERVICE
my-compose-app-db-1     "docker-entrypoint.s…"   db
my-compose-app-web-1    "/docker-entrypoint.…"   web
```

```bash
# View logs
docker compose logs web
```

```bash
# Verify web server
curl http://localhost:8080
```

```bash
# Stop all services
docker compose down
```

**Expected Output:**
```
[+] Stopping 2/2
 ✔ Container my-compose-app-web-1     Stopped
 ✔ Container my-compose-app-db-1      Stopped
 ✔ Container my-compose-app_web-1     Removed
 ✔ Container my-compose-app_db-1      Removed
```

**Explanation:**
- Docker Compose defines multi-container apps in YAML
- `services`: Define each container
- `volumes`: Persistent data storage
- `depends_on`: Start dependencies first
- `docker compose up -d`: Start all services in background
- `docker compose down`: Stop and remove all services

---

### Exercise 4: Persistent Data

**Task:** Use volumes for data persistence.

**Solution:**

```bash
# Create a named volume
docker volume create my-data
```

```bash
# Verify volume exists
docker volume ls
```

**Expected Output:**
```
DRIVER    VOLUME NAME
local     my-data
```

```bash
# Run a container that writes to the volume
docker run -d --name writer -v my-data:/data nginx:alpine sh -c "echo 'Persistent data' > /data/data.txt && sleep 3600"
```

```bash
# Verify data was written
docker run --rm -v my-data:/data alpine cat /data/data.txt
```

**Expected Output:**
```
Persistent data
```

```bash
# Remove the writer container
docker rm -f writer
```

```bash
# Run a new container with the same volume
docker run --rm -v my-data:/data alpine cat /data/data.txt
```

**Expected Output:**
```
Persistent data
```

```bash
# Data persists! Cleanup
docker volume rm my-data
```

**Explanation:**
- Volumes exist outside container lifecycle
- Data survives container deletion
- Multiple containers can share the same volume
- Named volumes are easier to manage than bind mounts

---

### Exercise 5: Image Inspection

**Task:** Inspect Docker image details.

**Solution:**

```bash
# Pull Python image
docker pull python:3.11
```

```bash
# Use docker inspect
docker inspect python:3.11
```

**Expected Output:**
```
[
    {
        "Id": "sha256:abc123...",
        "RepoTags": ["python:3.11"],
        "Created": "2025-01-15T00:00:00Z",
        "Size": 923456789,
        "Architecture": "amd64",
        "Os": "linux",
        "Layers": [
            "sha256:def456...",
            "sha256:ghi789...",
            ...
        ],
        "Env": [
            "PATH=/usr/local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin",
            "LANG=C.UTF-8",
            "PYTHON_VERSION=3.11.1",
            "PYTHON_PIP_VERSION=23.2.1"
        ],
        "Cmd": [
            "python3"
        ],
        "ExposedPorts": {
            "8080/tcp": {}
        }
    }
]
```

```bash
# View specific fields
docker inspect -f '{{.Config.Env}}' python:3.11
```

**Expected Output:**
```
[PATH=/usr/local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin LANG=C.UTF-8 PYTHON_VERSION=3.11.1 PYTHON_PIP_VERSION=23.2.1]
```

```bash
# View image size
docker inspect -f '{{.Size}}' python:3.11
```

**Expected Output:**
```
923456789
```

```bash
# Run container and explore filesystem
docker run -it python:3.11 sh
```

**Inside container:**
```bash
# Explore file system
ls /
which python
python --version
exit
```

**Expected Output:**
```
bin   dev  etc   lib   media  mnt   opt   proc  root  run   sbin  srv  sys  tmp  usr   var
/usr/local/bin/python
Python 3.11.1
```

```bash
# Compare with alpine version
docker pull python:3.11-alpine
docker inspect -f '{{.Size}}' python:3.11-alpine
```

**Expected Output:**
```
450123456    # Much smaller!
```

**Explanation:**
- `docker inspect`: Show detailed image metadata
- Image consists of layers (each Dockerfile instruction = one layer)
- Alpine-based images are much smaller
- Environment variables, exposed ports, and default command are all in image config

---

## Advanced Docker Examples

### Multi-Stage Build

```dockerfile
# Build stage
FROM golang:1.21 AS builder
WORKDIR /app
COPY . .
RUN go build -o app

# Runtime stage (much smaller)
FROM alpine:latest
WORKDIR /root/
COPY --from=builder /app/app .
CMD ["./app"]
```

### Health Check

```dockerfile
FROM nginx:alpine
HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost/ || exit 1
```

### Resource Limits

```bash
docker run -m 512m --cpus=1.0 nginx
```

---

## Docker Compose Examples

### WordPress with MySQL

```yaml
version: '3.8'
services:
  wordpress:
    image: wordpress
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_PASSWORD: examplepass
    volumes:
      - wordpress_data:/var/www/html

  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: examplepass
      MYSQL_DATABASE: wordpress
    volumes:
      - db_data:/var/lib/mysql

volumes:
  wordpress_data:
  db_data:
```

---

## Instructor Notes

### Teaching Tips

1. **Live container demo**: Run nginx during class, show immediate results
2. **Dockerfile walkthrough**: Build custom image step by step
3. **Volume demonstration**: Show data persistence across container restarts
4. **Docker Compose demo**: Run multi-container app (web + database)
5. **Resource comparison**: Show `docker stats` to see container resource usage

### Common Student Issues

| Issue | Solution |
|-------|----------|
| Can't connect to container | Check port mapping: `-p host:container` |
| Image not found | Run `docker pull` first or check image name |
| Permission denied | Add user to docker group or use sudo |
| Container exits immediately | Check logs with `docker logs`, might need `-it` flag |
| Data not persisting | Need to use volumes or bind mounts |

### Extension Activities

- Containerize a simple web application
- Create a multi-tier app with Docker Compose (web, api, database)
- Set up a development environment with Docker
- Explore Docker security best practices
- Compare Docker with Podman (alternative runtime)
- Create a CI/CD pipeline with Docker
- Research container orchestration with Kubernetes
