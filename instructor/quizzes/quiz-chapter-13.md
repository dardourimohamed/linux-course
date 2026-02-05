# Chapter 13 Quiz: Docker Containers

## Quiz Questions

### Multiple Choice

**1. What is a Docker container?**
- A) A virtual machine
- B) A lightweight, portable application environment
- C) A package manager
- D) A file archive

**Answer:** B

**2. What is a Dockerfile?**
- A) A Docker configuration file
- B) A recipe for building a Docker image
- C) A list of Docker commands
- D) A container runtime

**Answer:** B

**3. Which command runs a container?**
- A) docker build
- B) docker run
- C) docker create
- D) docker start

**Answer:** B

**4. What is Docker Hub?**
- A) A network monitoring tool
- B) A registry of Docker images
- C) A container orchestration tool
- D) A Docker documentation site

**Answer:** B

**5. What is the main difference between containers and virtual machines?**
- A) Containers include a full OS, VMs don't
- B) VMs include a full OS, containers share the host kernel
- C) Containers are slower than VMs
- D) There is no difference

**Answer:** B

### True/False

**6. True or False: Docker images are read-only templates for containers.**

**Answer:** True

**7. True or False: Docker Compose is used to manage multi-container applications.**

**Answer:** True

**8. True or False: Containers are ephemeral by default - data is lost when container stops.**

**Answer:** True

**9. True or False: The docker exec command runs a command inside a running container.**

**Answer:** True

**10. True or False: Volumes are used to persist data beyond container lifecycle.**

**Answer:** True

### Short Answer

**11. What are the main Docker commands for managing images and containers?**

**Answer:**
**Image Management:**
- `docker pull image` - Download image from registry
- `docker images` - List local images
- `docker build -t name .` - Build image from Dockerfile
- `docker rmi image` - Delete image

**Container Management:**
- `docker run image` - Create and start container
- `docker ps` - List running containers
- `docker ps -a` - List all containers
- `docker stop container` - Gracefully stop container
- `docker start container` - Start stopped container
- `docker rm container` - Delete container
- `docker logs container` - View container logs
- `docker exec container command` - Run command in container

**12. Explain the difference between a Docker image and a Docker container.**

**Answer:**
**Docker Image:**
- Read-only template for creating containers
- Contains application code, dependencies, runtime
- Built from Dockerfile or pulled from registry
- Stored in layers (each Docker instruction = one layer)
- Can be shared and distributed

**Docker Container:**
- Running instance of an image
- Created with `docker run`
- Has writable layer on top of image
- Ephemeral - changes lost when deleted (unless using volumes)
- Multiple containers can run from same image

**Analogy**: Image is like a class (template), Container is like an object (instance).

**13. What are Docker volumes and why are they important?**

**Answer:**
**Docker volumes** are persistent storage that exists outside the container lifecycle. They're important because:

1. **Data persistence**: Containers are ephemeral - volumes keep data when container stops/deletes

2. **Shared storage**: Multiple containers can access same volume

3. **Host mounting**: Bind mounts connect host directories to containers
   ```bash
   docker run -v /host/path:/container/path image
   ```

4. **Named volumes**: Managed by Docker, easier to reference
   ```bash
   docker volume create my-data
   docker run -v my-data:/data image
   ```

5. **Backups**: Easier to backup volumes than containers

6. **Configuration**: Separate data from application code

**Without volumes**: Database loses all data when container restarts. With volumes: Data persists across container lifecycle.

### Discussion Question

**14. Docker has revolutionized application deployment by enabling "build once, run anywhere." Explain what this means and discuss how containers solve the classic "it works on my machine" problem. Compare containerized deployment with traditional methods (installing directly on servers, virtual machines), and discuss the trade-offs of each approach.**

**Sample Answer:**

**"Build Once, Run Anywhere"**

This means a containerized application works identically across all environments - developer laptop, testing, staging, production - because the container includes everything needed: application, dependencies, runtime, configuration.

**Solving "Works on My Machine":**

**Problem without containers:**
- Different OS versions
- Different library versions
- Different configurations
- Missing dependencies
- Environment variables differ

**Solution with containers:**
- Same image everywhere
- All dependencies packaged
- Configuration baked in
- Isolated from host differences

**Comparison of Deployment Methods:**

**1. Direct Installation on Servers:**
```
Server → Install dependencies → Install app → Configure → Run
```
**Pros:**
- No overhead
- Full hardware access
- Familiar to sysadmins

**Cons:**
- Environment-specific issues
- Difficult to reproduce bugs
- Updates can break things
- Each server configured differently
- Scale-out is manual

**2. Virtual Machines:**
```
Hypervisor → Guest OS → Dependencies → App → Run
```
**Pros:**
- Isolated from host
- Can run different OS
- Good for multiple apps on one server

**Cons:**
- Full OS overhead (GBs of RAM)
- Slow startup (minutes)
- Large disk footprint
- Resource-heavy

**3. Containers (Docker):**
```
Docker Engine → Image → App → Run
```
**Pros:**
- Lightweight (MBs of RAM)
- Fast startup (seconds)
- Portable (any Linux host)
- Versioned and reproducible
- Easy to scale (orchestration)
- Consistent environments

**Cons:**
- Less isolation than VMs
- Shared kernel (Linux only)
- Learning curve
- Security considerations (running as root)

**Trade-offs:**

| Aspect | Direct Install | VM | Container |
|--------|---------------|-----|-----------|
| **Performance** | Best | Good | Good |
| **Isolation** | None | Full | Process-level |
| **Portability** | Poor | Medium | Excellent |
| **Startup Time** | N/A | Minutes | Seconds |
| **Resource Usage** | Minimal | High | Low |
| **Scalability** | Manual | Manual | Auto (K8s) |
| **Reproducibility** | Poor | Good | Excellent |
| **Complexity** | Low | Medium | Medium |

**When to Use Each:**
- **Direct Install**: Simple utilities, system tools, maximum performance needed
- **VMs**: Running different OS, strong isolation needed, legacy apps
- **Containers**: Microservices, web apps, CI/CD, cloud deployments

**Real-World Impact:**
Containers enable:
- Microservices architecture
- CI/CD pipelines
- Cloud-native applications
- DevOps practices
- Faster development cycles

**Conclusion**: Containers strike a balance between the isolation of VMs and the efficiency of direct installation. They've become the de facto standard for modern application deployment because they solve the environment consistency problem while remaining lightweight and portable.

## Answer Key

| Question | Answer | Points |
|----------|--------|--------|
| 1 | B | 1 |
| 2 | B | 1 |
| 3 | B | 1 |
| 4 | B | 1 |
| 5 | B | 1 |
| 6 | True | 1 |
| 7 | True | 1 |
| 8 | True | 1 |
| 9 | True | 1 |
| 10 | True | 1 |
| 11 | Main commands listed | 4 |
| 12 | Image vs container explained | 3 |
| 13 | Volumes explained | 3 |
| 14 | Deployment methods compared | 5 |

**Total Points:** 27

**Passing Score:** 18/27 (67%)

---

## Instructor Notes

### Common Misconceptions to Address

1. **"Containers are just lightweight VMs"** - They share the host kernel, no full OS overhead.

2. **"Docker is only for production"** - Development environments benefit hugely from consistency.

3. **"Containers are secure by default"** - Running as root is common but dangerous. Security requires attention.

4. **"You need to learn orchestration immediately"** - Start with basic docker commands, k8s can wait.

### Teaching Tips

- **Live container demo**: Run nginx during class, show it working immediately
- **Dockerfile walkthrough**: Build a custom image step by step
- **Volume demonstration**: Show data persistence across container restarts
- **Docker Compose demo**: Run a multi-container app (web + database)
- **Compare with VMs**: Show resource usage differences with docker stats

### Extension Activities

- Containerize a simple web application
- Create a multi-container app with Docker Compose
- Explore Docker Hub and find useful images
- Set up a development environment with Docker
- Research container security best practices
- Compare Docker with other container runtimes (Podman, containerd)
