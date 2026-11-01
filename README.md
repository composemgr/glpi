## 👋 Welcome to glpi 🚀

GLPI is a free software asset and IT inventory management application

## 📋 Description

GLPI is a free software asset and IT inventory management application

## 🚀 Services

- **app**: diouxx/glpi:latest

### Infrastructure Components

- **glpi-db**: MariaDB database

## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/glpi/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/glpi" ~/.local/srv/docker/glpi
cd ~/.local/srv/docker/glpi
cp default.env .env
docker compose --env-file .env up -d
```

### Option 3: Using composemgr
```bash
composemgr install glpi
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
BASE_HOST_NAME=glpi.example.com
DB_USER_NAME=glpi
DB_USER_PASS=replace-with-a-random-password
DB_CREATE_DATABASE_NAME=glpi
```

See `docker-compose.yaml` for complete list of configurable options.

`diouxx/glpi` does not read database credentials from the environment; it
provisions them in its web installer. On first visit to
`http://172.17.0.1:64188`, choose "Install" and enter the database host
`glpi-db`, port `3306`, and the database, user, and password values created
here. Those credentials are then written to the mounted config directory and
persist across restarts.

### Environment Files

`default.env.sample` holds the full set of variables and is copied to `.env`
before starting the stack:

```bash
cp default.env .env
docker compose --env-file .env up -d
```

`app.env.sample` holds the app-specific overrides and is copied to `app.env`:

```bash
cp app.env.sample app.env
```

`composemgr up` applies `app.env` and `default.env` automatically. Raw
`docker compose` commands need `--env-file .env` to pick up the same values.

### Reverse Proxy

Three standalone compose files ship in this repo:

| File | Use |
|------|-----|
| `docker-compose.yaml` | Default, published on `172.17.0.1:64188` |
| `docker-compose.traefik.yaml` | Behind an existing external `traefik` network |
| `docker-compose.tunnel.yaml` | Behind an existing external `cloudflare` tunnel network |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

## 🌐 Access

- **Web Interface**: http://172.17.0.1:64188

## 📂 Volumes

- `./volumes/data/glpi` - GLPI application files at `/var/www/html/glpi`, including config and uploads
- `./volumes/db/mariadb/glpi` - Database data

## 🔐 Security

- Change all default passwords before deploying to production
- Set `DB_ADMIN_PASS` and `DB_USER_PASS` to strong random values before the
  first start; MariaDB refuses to initialize with empty passwords
- Configure HTTPS using a reverse proxy (nginx, traefik, caddy)
- Regularly update Docker images for security patches
- Backup your data regularly

## 🔍 Logging

```shell
docker compose --env-file .env logs -f app
```

## 🛠️ Management

```bash
# Start services
docker compose --env-file .env up -d

# Stop services
docker compose --env-file .env down

# Update to latest images
docker compose --env-file .env pull && docker compose --env-file .env up -d

# View logs
docker compose --env-file .env logs -f

# Restart services
docker compose --env-file .env restart
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
