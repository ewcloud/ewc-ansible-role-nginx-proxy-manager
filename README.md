# Nginx Proxy Manager Ansible Role

This repository contains a configuration template 
(i.e. an [Ansible Role](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)) 
to customize your environment in the
[European Weather Cloud (EWC)](https://europeanweather.cloud/).
The template is designed to:
* Configure a pre-existing Ubuntu instance to run [Nginx Proxy Manager](https://nginxproxymanager.com),
  a fully-featured tool that helps to lower the entry barrier for users who are interested in learning
  about and working with [Nginx](https://nginx.org/en) servers.

## Copyright and License
> 💡 No dependencies are distributed as part of this repository.

See the LICENSE file for licensing information as it pertains to
files in this repository.

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

Create an Ansible Playbook file to load your customizations: 

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

## Final Environment
> ⚠️ Versions listed here refer only to those available for Ubuntu 22 packages
as of August 8th, 2025. As new security patches/features are published by their
authors, and newer Ubuntu image versions are introduced into the EWC, the 
effective versions installed in your environment might be higher.

Third-party components used in the resulting environment.

### Ubuntu 22 Environment

The following components will be included in the resulting environment:

| Component | Version | License | Home URL |
|------|---------|---------|--------------|
| docker-ce | >=28.3.3 | Apache-2.0 | https://github.com/docker-archive/docker-ce |
| docker-ce-cli | >=28.3.3 | Apache-2.0 | https://github.com/docker/cli |
| containerd.io | >=1.7.27  | Apache-2.0 | https://github.com/containerd/containerd |
| docker-compose-plugin | >=2.39.1 |  Apache-2.0 | https://github.com/docker/compose |
| nginx-proxy-manager | >=2.12.6 |  MIT | https://hub.docker.com/r/jc21/nginx-proxy-manager |

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
