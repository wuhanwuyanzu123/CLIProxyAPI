# Server Deployment

This repository includes the deployment files used by the 88 server.

## Files

- `docker-compose.server.yml` starts the `cli-proxy-api` service and maps the same ports used on the 88 server.
- `Dockerfile.server` can be used to build a server image when you do not want to pull `eceasy/cli-proxy-api:latest`.
- `config.server.example.yaml` is a sanitized example. Do not commit a real `config.yaml`.
- `auths/` is intentionally ignored except for `.gitkeep`; copy OAuth/account files to the target server manually.

## First Deploy On A New Server

```bash
git clone https://github.com/wuhanwuyanzu123/CLIProxyAPI.git /opt/CLIProxyAPI
cd /opt/CLIProxyAPI

cp config.server.example.yaml config.yaml
mkdir -p auths logs static
```

Edit `config.yaml`:

- Replace `CHANGE_ME_BCRYPT_HASH` with the management password hash.
- Replace `CHANGE_ME_API_KEY` with the API key you want clients to use.
- Put provider auth JSON files under `auths/`.

Start the service:

```bash
docker compose -f docker-compose.server.yml up -d
docker compose -f docker-compose.server.yml logs -f --tail=100
```

Management page:

```text
http://SERVER_IP:8317/management.html
```

## Update Existing Server

```bash
cd /opt/CLIProxyAPI
git pull --ff-only
docker compose -f docker-compose.server.yml pull
docker compose -f docker-compose.server.yml up -d
```

If you build locally instead of pulling the image:

```bash
docker build -f Dockerfile.server -t eceasy/cli-proxy-api:latest .
docker compose -f docker-compose.server.yml up -d
```
