# Docker & Docker Compose Usage Guide(Build)

This guide explains how to build and use the Docker image and Docker Compose setup for the canmv_k230 project.

## Prerequisites
- [Docker](https://docs.docker.com/get-docker/) installed
- [Docker Compose](https://docs.docker.com/compose/install/) installed (if using Compose)

## [Optional] Build the Docker Image Manually

From the project root directory, run:

```sh
docker build -f tools/docker/Dockerfile.txt -t canmv_k230_build .
```

- This builds the image using the provided Dockerfile.

## [Optional] Run a Container Manually

To start a container and mount your project directory:

```sh
# check your pwd path here assume you in repo home path instead of tools/docker
docker run --rm -it -v $(pwd):/workspace/canmv_k230_repo canmv_k230_build
```

- This opens a shell in the container with your code mounted at `/workspace`.

## 1. Using Docker Compose

A `docker-compose.yml` is provided for convenience. From the directory containing the compose file, run:

```sh
cd ./tools/docker
docker compose up --build
```

- This builds the image (if needed) and starts a container with your project mounted.

## 2. Building the Project Inside the Container

Once inside the container shell, you can use the standard build commands, for example:

```sh
# Initialize and sync repo (if needed)
repo init -u . -m canmv_k230_repo/tools/repo/default.xml --repo-url=https://github.com/canmv-k230/git-repo.git
repo sync

# Download toolchain (if needed)
make dl_toolchain

# List available configurations
make list_def

# Select a configuration (replace with your board's defconfig)
make k230_canmv_defconfig

# Build the project
make log
```

## 3. Stopping the Container

- For Docker Compose: Press `Ctrl+C` in the terminal, then run `docker-compose down` to clean up.
- For manual runs: Type `exit` or press `Ctrl+D`.

---

For more details, see `BUILD.md` in the project root.
