# Ansible Playbook for Web Services

This playbook configures a small VPS running publicly available services.

**Architecture:**

-   [Authelia](https://www.authelia.com/) - Authentication and authorization
-   [Cloudflare](https://www.cloudflare.com/) - DNS proxy and security
-   [CrowdSec](https://www.crowdsec.net/) - Collaborative Security
-   [Docker](https://www.docker.com/) - Service orchestration
-   [Plausible](https://plausible.io/) - Self-hosted web analytics
-   [Traefik](https://doc.traefik.io/traefik/) - Reverse Proxy

## Ansible Usage

### 1. Manage variables

-   Non-secret variables are contained in `default_variables.yml`
-   Secrets are encrypted with `ansible-vault` and contained in `vault.yml`

### 2. Run the playbook

To run the entire playbook use the following command.

```bash
ansible-playbook --ask-vault-pass main.yml

# To specify a password
ansible-playbook --ask-vault-pass main.yml

# Or, if you have a password file
ansible-playbook --vault-password-file [filename] main.yml
```

Specify specific tags by appending `--tags "tag1,tag2"` to the command above. Or, you can skip specific tags by using `--skip-tags "tag1,tag2"`

### Available Tags

| Tag             | Description                                             |
| --------------- | ------------------------------------------------------- |
| `docker`        | Install and configure Docker                            |
| `update`        | Updates packages                                        |
| `cloudflare_ip` | Updates shell script syncing IP address with Cloudflare |
| `proxy`         | Configures Traefik and Authelia                         |
| `crowdsec`      | Installs and configures Crowdsec                        |
| `plausible`     | Installs and configures Plausible analytics             |

# Configuring Services

## Networking

Traefik operates on a non-dafault network named `traefik_proxy`.

Add new containers which will be accessible to Traefik with the following:

```yaml
# Add the network to the docker-compose file
networks:
    traefik_proxy:
        name: traefik_proxy
        external: true

# Add the network to a service and allow Docker to create manage the IP Address
services:
    service:
        networks:
            - traefik_proxy
        ...

# Add the network to a service and specify an IP address for the service
services:
    service:
        networks:
            traefik_proxy:
                ipv4_address: 192.168.90.3
        ...
```

# Contributing

## Set up: Once per computer

1. Install prerequisites with Python (Managing Python installations is out of the scope of this README)
    ```bash
    python3 -m pip install --user pre-commit
    python3 -m pip install --user commitizen
    python3 -m pip install --user ansible
    python3 -m pip install --user ansible-lint
    python3 -m pip install --user yamllint
    ```
2. Install the pre-commit hooks with `pre-commit install --install-hooks`

## Developing

-   This project follows the [Conventional Commits](https://www.conventionalcommits.org/) standard to automate [Semantic Versioning](https://semver.org/) and [Keep A Changelog](https://keepachangelog.com/) with [Commitizen](https://github.com/commitizen-tools/commitizen).
-   Commit changes using `cz c`
-   Run `cz bump` to bump the package's version, update the `CHANGELOG.md`, and create a git tag.
