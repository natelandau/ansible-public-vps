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

# Troubleshooting

### List all domains with certificates in Traefik Acme file

```bash
sudo cat ~/services/traefik/acme/acme.json | jq '.letsencrypt.Certificates | .[].domain.main'
```
