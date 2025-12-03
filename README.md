# Nginx Proxy Manager Ansible Role

This repository contains a configuration template 
(i.e. an [Ansible Role](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)) 
to customize your environment in the
[European Weather Cloud (EWC)](https://europeanweather.cloud/).
The template is designed to:
* Configure a pre-existing instance with Ubuntu versions 24 or 22 to run [Nginx Proxy Manager](https://nginxproxymanager.com),
  a fully-featured tool that helps to lower the entry barrier for users who are interested in learning
  about and working with [Nginx](https://nginx.org/en) servers.

## Copyright and License
Copyright © EUMETSAT 2025.

The provided code and instructions are licensed under the MIT license. 
They are intended to automate the setup of an environment that includes 
third-party software components.
The usage and distribution terms of the resulting environment are 
subject to the individual licenses of those third-party libraries.

Users are responsible for reviewing and complying with the licenses of
all third-party components included in the environment.

Contact [EUMETSAT](http://www.eumetsat.int) for details on the usage and distribution terms.

## Authentication

Before proceeding, if you lack OpenStack Application Credentials or do not know
how to make them available to Ansible in your development environment, make sure
to check out the 
[EWC documentation](https://confluence.ecmwf.int/display/EWCLOUDKB/EWC+-+How+to+request+Openstack+Application+Credentials).

## Usage

The step-by-step described below assume your local file system follows the 
example structure below, with `ewc-ansible-role-nginx-proxy-manager` being a clone of this
repository:
```
.
├── roles
│   └── ewc-ansible-role-nginx-proxy-manager
├── inventory.yml
└── playbook.yml
```

### 1. Specify the target host and SSH credentials
Create an inventory file to specify address/credentials that Ansible should use
to reach the virtual machine you wish to configure:
```yaml
# inventory.yml
---
ewcloud:
  hosts:
    nginx_proxy_manager:
      ansible_python_interpreter: /usr/bin/python3
      ansible_host: <add the IPV4 address of the target host>
      ansible_ssh_private_key_file: <add the path to local SSH RSA private key file>
      ansible_user: <add the username which owns the SSH RSA private key >
```
### 2. Customize the template

Edit input values for the template [variables](./vars/main.yml) as needed (see
[Inputs](#inputs) section for details).
Then, proceed to create an Ansible Playbook file to load your customizations: 

```yaml
# playbook.yml
---
- name: Install NGINX Proxy Manager
  hosts: nginx_proxy_manager
  become: true
  become_user: root
  become_method: ansible.builtin.sudo

  roles:
    - ewc-ansible-role-nginx-proxy-manager
```

### 3. Apply the template

You can apply changes on the target host by running:
```bash
ansible-playbook -i inventory.yml playbook.yml
```

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|----------|
| npm_admin_ui_port | port number at which the Nginx Proxy Manager admin UI is served. Example: `8080` | `number` | n/a | yes |
| os_security_group_name | OpenStack security group containing all firewall rules required for Nginx Proxy Manager operation. Example: `nginx-proxy-manager` | `string` | n/a | yes |

## Dependencies
> 💡 Upon execution, a SBOM (SPDX format) is auto-generated and stored in the VM's file system root directory (see `/sbom.json`).
The following third-party components will be included in the resulting environment:

| Component | Home URL |
|------|---------|
| docker-ce |  https://github.com/moby/moby |
| docker-ce-cli | https://github.com/docker/cli |
| containerd.io | https://github.com/containerd/containerd |
| docker-compose-plugin | https://github.com/docker/compose |
| nginx-proxy-manager |  https://hub.docker.com/r/jc21/nginx-proxy-manager |

## Changelog
All notable changes (i.e. fixes, features and breaking changes) are documented 
in the [CHANGELOG.md](./CHANGELOG.md).

## Contributing

Thanks for taking the time to join our community and start contributing!
Please make sure to:
* Familiarize yourself with our [Code of Conduct](./CODE_OF_CONDUCT.md) before 
contributing.
* See [CONTRIBUTING.md](./CONTRIBUTING.md) for instructions on how to request 
or submit changes.

## Authors

[European Weather Cloud](http://support.europeanweather.cloud/) 
<[support@europeanweather.cloud](mailto:support@europeanweather.cloud)>
