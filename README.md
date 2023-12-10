## App Deploy Ansible Playbook

### Code Structure

```
.
├── inventory
│   ├── group_vars
│   │   ├── dev.yml
│   │   └── staging.yml
│   ├── dev.yml
│   └── staging.yml
├── roles
│   └── deploy_app
│       ├── defaults
│       │   └── main.yml
│       ├── tasks
│       │   ├── deploy.yml
│       │   └── main.yml
│       ├── templates
│       │   └── compose.yaml.tmpl
│       └── vars
│           └── main.yml
├── deploy_app.yml
└── README.md
```

### Requirements
- Python 3.8 or higher on the Ansible host
- ansible_core 2.15.6 or higher on the Ansible host
- Docker Swarm nodes on the Ansible targets (To have multiple replicas)

### Playbook Functions
- Prepares the Swarm OS to run the services
- Checks if the host has enough resources to run the services (Using service requirements settings and resource reservations)
- Deploys the Docker Compose configuration file
- Starts the service up
- Checks the service availability

### Usage
- New services can be added and individually configured in the [roles/deploy_app/vars/main.yml](https://github.com/CSimpiFoN/finnet-ansible/tree/main/roles/deploy_app/vars/main.yml) file
- All default environment related variables are set in [roles/deploy_app/vars/main.yml](https://github.com/CSimpiFoN/finnet-ansible/tree/main/roles/deploy_app/vars/main.yml) file, and they can be overwritten in the respective [group_var](https://github.com/CSimpiFoN/finnet-ansible/tree/main/inventory/group_vars) files
- New hosts can be added to environment hosts files [dev](https://github.com/CSimpiFoN/finnet-ansible/tree/main/inventory/dev.yml) or [staging](https://github.com/CSimpiFoN/finnet-ansible/tree/main/inventory/staging.yml) or other groups can be created
- New environments can be added by adding a [new host group](https://github.com/CSimpiFoN/finnet-ansible/tree/main/inventory) and corresponding [group_vars](https://github.com/CSimpiFoN/finnet-ansible/tree/main/inventory/group_vars) file

Run the playbook against all environments:
```
ansible-playbook -i inventory deploy_app.yml
```
Or limit the run to certain environment (dev/stg)
```
ansible-playbook -i inventory/{environment}.yml deploy_app.yml
```

### Design Considerations
- The service configuration settings are passed as environmental variables to the containers
- The Docker Compose configuration file is generated from [template](https://github.com/CSimpiFoN/finnet-ansible/tree/main/roles/deploy_app/templates/compose.yaml.tmpl) using the configuration taken from the role
- The template only manages currently used functions, is another section has to be added, the template should be amended

### Production Considerations
- Never configure sensitive data like username/password from plain text setting
  - Use Ansible Vault to encrypt the values (Still going to be stored plain text on the Swarm host and in the container's environment variables)
  - Best to use an external secret store, like HashiCorp Vault
