# Research 4: Git & Docker Introductory Scope

## Sources Analyzed
- GitLab University curriculum
- Codecademy Git course
- Docker official 101 tutorial
- Docker Curriculum
- University Git learning resources

## Git: Essential Commands for Beginners

### Minimal Viable Command Set

Based on course analysis, beginners need these 10 commands:

| Command | Purpose | Frequency |
|---------|---------|-----------|
| `git status` | Check current state | Every operation |
| `git add` | Stage changes | Every commit |
| `git commit` | Save changes | Every save point |
| `git log` | View history | Review |
| `git diff` | See changes | Before commit |
| `git clone` | Copy repository | First time |
| `git pull` | Get updates | Collaboration |
| `git push` | Send changes | Collaboration |
| `git branch` | List branches | Collaboration |
| `git checkout` | Switch branches/restore | Collaboration |

### Teaching Sequence

1. **Concept** — What is version control? Why use it?
2. **Local workflow** — init, add, commit, status, log
3. **Branching basics** — branch, checkout (optional for intro)
4. **Remote basics** — clone, pull, push
5. **Common scenarios** — Undo changes, resolve conflicts

### Git in Linux Context

**Integration points:**
- Use CLI Git (reinforces command line skills)
- `.gitignore` teaches Linux file patterns
- Diff output reinforces reading CLI output
- SSH keys for Git teach authentication

**Time allocation:** 1 session (90 minutes)
- 30min: Concepts and local workflow
- 60min: Hands-on exercises with real files

### Recommended Exercise

"Track your shell scripts" — Students create a repo for scripts they write during the course, teaching Git as they go.

---

## Docker: Basic Introduction Scope

### Essential Concepts

| Concept | Description | Analogy |
|---------|-------------|---------|
| **Image** | Template for containers | Like a VM template |
| **Container** | Running instance | Like a lightweight VM |
| **Dockerfile** | Recipe for an image | Like a build script |
| **Registry** | Where images are stored | Like an app store |
| **Volume** | Persistent storage | Like a shared folder |

### Minimal Viable Command Set

| Command | Purpose |
|---------|---------|
| `docker run` | Start a container |
| `docker ps` | List running containers |
| `docker images` | List images |
| `docker stop` | Stop a container |
| `docker rm` | Remove a container |
| `docker pull` | Download an image |

### Teaching Sequence

1. **Concepts** — Containers vs VMs, why Docker matters
2. **Images** — Pull, run basic containers (nginx, hello-world)
3. **Basic operations** — ps, stop, rm
4. **Volumes (optional)** — Persist data
5. **Dockerfile (optional)** — Build own image

### Docker in Linux Context

**Integration points:**
- Reinforces CLI skills
- Shows Linux's dev power
- Introduces modern dev workflows
- Practical: run services locally

**Time allocation:** 1 session (90 minutes)
- 45min: Concepts and basic commands
- 45min: Run a web server container

### Recommended Exercise

"Run your first containerized app" — Pull and run an nginx container, serve a simple HTML page.

### What to Skip (Intro Level)

- Docker Compose (too advanced for first exposure)
- Networking specifics
- Multi-stage builds
- Container orchestration (Kubernetes)
- Production deployment

## Sources

- GitLab University: https://university.gitlab.com/courses/introduction-to-git
- Codecademy Git: https://www.codecademy.com/learn/learn-git
- Docker 101: https://www.docker.com/101-tutorial/
- Docker Curriculum: https://docker-curriculum.com/
- Docker for Beginners: https://training.play-with-docker.com/beginner-linux/
