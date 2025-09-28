# Pterodactyl Development Environment

This repository provides a `docker`-based local development environment for Pterodactyl.

**⚠️ This environment is for local development only — it is not suitable for production.**

> This is the official Pterodactyl development setup that the owner of this repo [`@Biraru`](https://github.com/Biraru) uses. It has been tested on an Ubuntu 24.04 VM running on Hyper-V. Some details might be missing, so contributions via PRs or Issues are welcome to improve documentation and functionality.

---

## Prerequisites

Make sure the following tools are installed on your system:

* [Docker](https://docker.io) - [Installation link for Debian](https://docs.docker.com/engine/install/debian/#install-using-the-repository)
* [mkcert](https://github.com/FiloSottile/mkcert) - [Installation link for Debian](https://github.com/FiloSottile/mkcert?tab=readme-ov-file#linux) - [Installation of HomeBrew](https://brew.sh/) - [Set command](https://docs.brew.sh/Homebrew-on-Linux)

---

## Setup

Clone this repository and run the setup script to configure dependencies, certificates, and host file routing:

```bash
git clone https://github.com/StelliaJS/pterodactyl-development
cd pterodactyl-development/
./setup.sh
```

### What Gets Created

* **Traefik** – reverse proxy
* **Panel & Wings** – main Pterodactyl services
* **MySQL & Redis** – database and cache
* **Minio** – S3-compatible storage

---

## Add Docker group
```bash
sudo groupadd docker
```

```bash
sudo usermod -aG docker $USER
```

## Using the Environment

Start the environment with:

```bash
./beak up -d
```

> `./beak` is a convenience wrapper for common Docker Compose commands.

You can access the containers via SSH:

```bash
./beak app    # SSH into the Panel container
./beak wings  # SSH into the Wings container
```

The web interfaces are available at:

* **Panel:** `https://pterodactyl.test`
* **Wings:** `https://wings.pterodactyl.test`

### Initial Setup

If you haven't configured a database or environment yet:

* **Panel:** SSH into the Panel container and run:

```bash
setup-pterodactyl
```

* **Wings:** SSH into the Wings container and run:

```bash
setup-wings
```

## Update local Windows `hosts` file:

File: `C:\Windows\System32\drivers\etc\hosts`
```
127.0.0.1 pterodactyl.test
127.0.0.1 wings.pterodactyl.test
127.0.0.1 minio.pterodactyl.test
127.0.0.1 s3.minio.pterodactyl.test
```
This completes the local setup and gets everything running.
