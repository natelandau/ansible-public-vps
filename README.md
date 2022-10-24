# Ansible natenate-org

Ansible roles to manage the natenate.org server

## 1. Ready your Ansible control host

Install the required roles by running the following command.

```bash
ansible-galaxy install -r requirements.yml
```

## 2. Review variables

Ensure that all the variables in `default_variables.yml` are set up correctly.

## 3. Run the playbook

To run the entire playbook use the following command.

```bash
ansible-playbook --ask-vault-pass main.yml

# To specify a password
ansible-playbook --ask-vault-pass main.yml

# Or, if you have a password file
ansible-playbook --vault-password-file [filename] main.yml
```

# Configuring Services

## Secrets Management

All secrets used for containers are contained in variables with `docker.env.j2`. This file is owned by root.

## Networking

Traefik operates on a non-dafault network named `traefik_proxy`.

Add new containers which will be accessible to Traefik with the following:

```yaml
services:
    service:
        networks:
            - traefik_proxy
        ...

networks:
    traefik_proxy:
        name: traefik_proxy
        external: true
```

# Troubleshooting

### List all domains with certificates in Traefik Acme file

```bash
sudo cat ~/services/traefik/acme/acme.json | jq '.letsencrypt.Certificates | .[].domain.main'
```
