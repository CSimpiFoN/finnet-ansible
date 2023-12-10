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