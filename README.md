# piman-catalog

This repository hosts the official application templates for the **piStore** homeserver orchestrator (`piman`). 

It provides pre-configured Docker Compose files and metadata tailored for ARM64 (Raspberry Pi) and standard Linux host environments.

---

## Repository Structure

```
piman-catalog/
├── README.md               # This documentation
├── .github/
│   └── workflows/
│       └── validate.yml    # CI checking compose file syntax
├── state/
│   └── deployments.yaml    # Cluster-wide default deployments configuration mapping
└── catalog/
    ├── <app-name>/
    │   ├── metadata.yaml   # Title, description, categories, required ports
    │   └── docker-compose.yml
```

---

## How to Synchronize in piStore

To sync this catalog, configure the repository URL in the **piStore Web UI** under **Settings & Sync**, or run the CLI sync command:

```bash
piman sync --repo https://github.com/sameerchandra/piman-catalog.git
```

This pulls the repository into `~/.pistore/catalog_cache/` so the Web UI and CLI status/deployment engine can parse it dynamically.

---

## Adding a New Application Template

We welcome open-source contributions! To add a new app:

1. Create a subdirectory under `catalog/<app-name>/` (use lowercase names and hyphens, e.g. `catalog/home-assistant`).
2. Add a `metadata.yaml` conforming to the following structure:
   ```yaml
   title: "Home Assistant"
   description: "Open source home automation that puts local control and privacy first."
   version: "2024.5.0"
   categories:
     - Smart Home
     - IoT
   icon: "🏠"
   required_ports:
     - "8123:8123"
   min_ram_mb: 1024
   ```
3. Add a `docker-compose.yml` file. Please ensure:
   - Volume paths are kept self-contained using relative directories (e.g. `./data/<app-name>/config:/config`).
   - Use multi-arch compatible images (such as `lscr.io/linuxserver/*` or official images with multi-platform tags).
   - Hardcoded environment variables (e.g. database credentials) are set directly to default values inside the compose file so that the template launches out of the box.

---

## Automated Validation

Every pull request will run syntax checking using `docker compose config` against all compose templates. Please run it locally before opening a pull request:

```bash
docker compose -f catalog/<app-name>/docker-compose.yml config --quiet
```
