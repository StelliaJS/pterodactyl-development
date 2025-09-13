# Pterodactyl Development Environment

This repository provides a `docker`-based local development environment for Pterodactyl.

**⚠️ This environment is for local development only — it is not suitable for production.**

> This is the official Pterodactyl development setup that the owner of this repo [`@Biraru`](https://github.com/Biraru) uses. It has been tested on an Ubuntu 24.04 VM running on Hyper-V. Some details might be missing, so contributions via PRs or Issues are welcome to improve documentation and functionality.

---

## Prerequisites

Make sure the following tools are installed on your system:

* [Docker](https://docker.io)
* [mkcert](https://github.com/FiloSottile/mkcert)

---

## Setup

Clone this repository and run the setup script to configure dependencies, certificates, and host file routing:

```bash
git clone https://github.com/BiraruStudios/development.git
cd development
./setup.sh
```

### What Gets Created

* **Traefik** – reverse proxy
* **Panel & Wings** – main Pterodactyl services
* **MySQL & Redis** – database and cache
* **Minio** – S3-compatible storage

---

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

This completes the local setup and gets everything running.