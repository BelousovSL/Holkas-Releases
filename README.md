<!-- Canonical source: Holkas/docs/releases-readme.md. Published to BelousovSL/Holkas-Releases. -->

# Holkas

> Public downloads and installation instructions for Holkas.

[![Latest release](https://img.shields.io/github/v/release/BelousovSL/Holkas-Releases?display_name=tag&sort=semver)](https://github.com/BelousovSL/Holkas-Releases/releases/latest)
[![Docker image](https://img.shields.io/docker/v/belousov2501/holkas?sort=semver&label=Docker)](https://hub.docker.com/r/belousov2501/holkas)
[![Docker pulls](https://img.shields.io/docker/pulls/belousov2501/holkas)](https://hub.docker.com/r/belousov2501/holkas)

Holkas is a migration workspace for customized Parus applications. It helps inspect, compare, and migrate application metadata together with related database objects.

> [!IMPORTANT]
> Holkas is under active development. Review every generated plan before applying it to an important database, and keep a tested database backup.

## Downloads

| Package | Use case | Download |
| --- | --- | --- |
| Holkas Web | Browser interface and API for Windows x64 | [`holkas-web-win-x64-<version>.zip`](https://github.com/BelousovSL/Holkas-Releases/releases/latest) |
| Holkas CLI | Command-line client for Windows x64 | [`holkas-cli-win-x64-<version>.zip`](https://github.com/BelousovSL/Holkas-Releases/releases/latest) |
| Docker image | Linux server or Docker Desktop | [`belousov2501/holkas`](https://hub.docker.com/r/belousov2501/holkas) |

Windows archives are self-contained; a separate .NET installation is not required. They are not currently code-signed, so verify the published SHA-256 checksum if Windows displays a SmartScreen warning.

## Quick start with Docker

```bash
docker volume create holkas-data

docker run -d \
  --name holkas \
  --restart unless-stopped \
  -p 8080:8080 \
  -v holkas-data:/data \
  -e HolkasWeb__BootstrapAdminPassword='choose-a-strong-password' \
  belousov2501/holkas:latest
```

Open <http://localhost:8080> and sign in as `admin` with the password supplied above.

The `holkas-data` volume contains the state database, artifacts, logs, and cryptographic keys. Keep it when updating or recreating the container. For repeatable deployments, pin a numbered image tag such as `0.1.0` instead of `latest`.

### Docker Compose

```yaml
services:
  holkas:
    image: belousov2501/holkas:latest
    container_name: holkas
    restart: unless-stopped
    ports:
      - "8080:8080"
    environment:
      HolkasWeb__BootstrapAdminPassword: ${HOLKAS_ADMIN_PASSWORD}
    volumes:
      - holkas-data:/data

volumes:
  holkas-data:
```

Put `HOLKAS_ADMIN_PASSWORD=choose-a-strong-password` in a local `.env` file, protect it, and run `docker compose up -d`.

## Install Holkas Web on Windows

1. Download `holkas-web-win-x64-<version>.zip` and `SHA256SUMS` from the [latest release](https://github.com/BelousovSL/Holkas-Releases/releases/latest).
2. Optionally compare the archive's SHA-256 hash with `SHA256SUMS`.
3. Extract the archive and open PowerShell in its `holkas-web-win-x64` directory.
4. Start Holkas:

```powershell
$env:HolkasWeb__Mode = "Standalone"
$env:HolkasWeb__BootstrapAdminPassword = "choose-a-strong-password"
$env:HolkasWeb__DataPath = "$PWD\data"
$env:HolkasWeb__StatePath = "$PWD\data\holkasweb.db"
$env:ASPNETCORE_URLS = "http://localhost:5000"
.\Holkas.Web.exe
```

Open <http://localhost:5000> and sign in as `admin`. Keep the PowerShell window open while Holkas is running; press `Ctrl+C` to stop it.

The bootstrap password is only used to create the first administrator. Change the password in the Web interface after signing in.

## Install the CLI on Windows

Download and extract `holkas-cli-win-x64-<version>.zip`, then check the server:

```powershell
.\holkas.exe status --server http://localhost:5000
```

For authenticated commands, create a script token in **Полномочия → Мой агент** and configure the current PowerShell session:

```powershell
$env:HOLKAS_SERVER = "http://localhost:5000"
$env:HOLKAS_API_KEY = "hpat_..."
.\holkas.exe connections list
```

Run `holkas --help` for the complete command reference.

## Updating and backups

- **Docker:** pull the new image and recreate the container with the same `holkas-data` volume.
- **Windows:** stop Holkas, back up the `data` directory, extract the new release separately, and point it to the existing data directory.
- Back up persistent data before every upgrade.
- Do not expose Holkas directly to the public internet; use a TLS reverse proxy and network restrictions for remote access.

## Verify a Windows download

Replace the filename with the version you downloaded:

```powershell
(Get-FileHash .\holkas-web-win-x64-0.1.0.zip -Algorithm SHA256).Hash.ToLower()
Get-Content .\SHA256SUMS
```

The calculated value must match the corresponding entry in `SHA256SUMS`.

## Release contents

Every successful release publishes:

- versioned Windows Web and CLI archives;
- a `SHA256SUMS` checksum file;
- Docker tags `<version>` and `latest`.

Browse all versions on the [Releases page](https://github.com/BelousovSL/Holkas-Releases/releases) or [Docker Hub](https://hub.docker.com/r/belousov2501/holkas/tags).
