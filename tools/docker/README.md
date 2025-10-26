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
mkdir build_k230_image
cd build_k230_image
repo init -u https://github.com/canmv-k230/manifest -b master --repo-url=https://github.com/canmv-k230/git-repo.git
```

## 3. Build the project


### 3.1a Customizing the Manifest File

Base on what you need to build, the default manifest file may not be suitable. You can copy a custom manifest file from the host to the container and use it during repo sync:(here we use `default_local_dev_repo.xml` it will use the this repo `dev` branch to sync code)

```sh
# cp ../canmv_k230_repo/tools/repo/default_local_docker.xml .repo/manifests/
cp ../canmv_k230_repo/tools/repo/default_local_dev_repo.xml .repo/manifests/
repo sync -m default_local_dev_repo.xml

# Troubleshooting:
# If you see errors about 'HEAD' or missing branches, ensure you have run 'git init' and created a branch (e.g., master or main) in this directory before running 'repo init'.
```

### 3.1b symbilic link

If you use the symbolic link method, you can use current version of code without repo sync:

```sh
# cp ../canmv_k230_repo/tools/repo/default_local_docker.xml .repo/manifests/
cp ../canmv_k230_repo/tools/repo/default_local_file.xml .repo/manifests/
mkdir src
ln -s /workspace/canmv_k230_repo src/canmv
## create symbilic link from /workspace/camv_k230_reo to build_k230_image/src/canmv

repo sync -m default_local_file.xml

# Troubleshooting:
# If you see errors about 'HEAD' or missing branches, ensure you have run 'git init' and created a branch (e.g., master or main) in this directory before running 'repo init'.
```

### 3.2 Build Commands

```sh
# Download toolchain (if needed)
make dl_toolchain

# List available configurations
make list_def

# Select a configuration (replace with your board's defconfig)
# see build_k230_image/configs for available defconfig files or add your own
# e.g. k230_canmv_01studio_defconfig
make k230_canmv_defconfig

# Build the project
make log
```

## 4. Stopping the Container

- For Docker Compose: Press `Ctrl+C` in the terminal, then run `docker-compose down` to clean up.
- For manual runs: Type `exit` or press `Ctrl+D`.

---

For more details, see `BUILD.md` in the project root.
