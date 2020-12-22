[![Ansible Galaxy](https://img.shields.io/badge/galaxy-nginxinc.nginx__config-5bbdbf.svg)](https://galaxy.ansible.com/nginxinc/nginx_config)
[![Molecule CI/CD](https://github.com/nginxinc/ansible-role-nginx-config/workflows/Molecule%20CI/CD/badge.svg)](https://github.com/nginxinc/ansible-role-nginx-config/actions)
[![License](https://img.shields.io/badge/License-Apache--2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

# 👾 *Help make the NGINX config Ansible role better by participating in our [survey](https://forms.office.com/Pages/ResponsePage.aspx?id=L_093Ttq0UCb4L-DJ9gcUKLQ7uTJaE1PitM_37KR881UM0NCWkY5UlE5MUYyWU1aTUcxV0NRUllJSC4u)!* 👾

# Ansible NGINX Configuration Role

This role configures NGINX Open Source and NGINX Plus on your target host.

**Note:** This role is still in active development. There may be unidentified issues and the role variables may change as development continues.

## Requirements

### Ansible

*   This role is developed and tested with [maintained](https://docs.ansible.com/ansible/latest/reference_appendices/release_and_maintenance.html#release-status) versions of Ansible. Backwards compatibility is not guaranteed.
*   Instructions on how to install Ansible can be found in the [Ansible website](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html).

### Molecule

*   Molecule `3.x` is used to test the various functionalities of the role.
*   Instructions on how to install Molecule can be found in the [Molecule website](https://molecule.readthedocs.io/en/latest/installation.html).

## Installation

### Ansible Galaxy

Use `ansible-galaxy install nginxinc.nginx_config` to install the latest stable release of the role on your system.

### Git

Use `git clone https://github.com/nginxinc/ansible-role-nginx-config.git` to pull the latest edge commit of the role from GitHub.

## Platforms

The NGINX config Ansible role supports all platforms supported by [NGINX Open Source](https://nginx.org/en/linux_packages.html#mainline) and [NGINX Plus](https://www.nginx.com/products/technical-specs/):

### NGINX Open Source

```yaml
Alpine:
  - 3.9
  - 3.10
  - 3.11
  - 3.12
CentOS:
  - 6
  - 7.4+
  - 8
Debian:
  - stretch
  - buster
RedHat:
  - 6
  - 7.4+
  - 8
SUSE/SLES:
  - 12
  - 15
Ubuntu:
  - xenial
  - bionic
  - eoan
  - focal
```

### NGINX Plus

```yaml
Alpine:
  - 3.9
  - 3.10
  - 3.11
Amazon Linux:
  - 2018.03
Amazon Linux 2:
  - any
CentOS:
  - 6.5+
  - 7.4+
  - 8
Debian:
  - stretch
  - buster
FreeBSD:
  - 11.2+
  - 12
Oracle Linux:
  - 6.5+
  - 7.4+
RedHat:
  - 6.5+
  - 7.4+
  - 8
SUSE/SLES:
  - 12
  - 15
Ubuntu:
  - xenial
  - bionic
  - eoan
  - focal
```

## Role Variables

This role has multiple variables. The descriptions and defaults for all these variables can be found in the **[`defaults/main/`](https://github.com/nginxinc/ansible-role-nginx-config/blob/main/defaults/main/)** folder in the following files:

|Name|Description|
|----|-----------|
|**[`main.yml`](https://github.com/nginxinc/ansible-role-nginx-config/blob/main/defaults/main/main.yml)**|NGINX simple config variables|
|**[`selinux.yml`](https://github.com/nginxinc/ansible-role-nginx-config/blob/main/defaults/main/selinux.yml)**|Set up SELinux to allow the necessary connections to your NGINX setup|
|**[`template.yml`](https://github.com/nginxinc/ansible-role-nginx-config/blob/main/defaults/main/template.yml)**|NGINX config template variables|
|**[`upload.yml`](https://github.com/nginxinc/ansible-role-nginx-config/blob/main/defaults/main/upload.yml)**|NGINX config/HTML/SSL upload variables|

## Example Playbooks

Working functional playbook examples can be found in the **[`molecule/`](https://github.com/nginxinc/ansible-role-nginx-config/blob/main/molecule/)** folder in the following files:

|Name|Description|
|----|-----------|
|**[`cleanup_module/converge.yml`](https://github.com/nginxinc/ansible-role-nginx-config/blob/main/molecule/cleanup_module/converge.yml)**|Cleanup an NGINX config and configure NGINX supported modules|
|**[`default/converge.yml`](https://github.com/nginxinc/ansible-role-nginx-config/blob/main/molecule/default/converge.yml)**|Use the NGINX config templating variables to create an NGINX config|
|**[`plus/converge.yml`](https://github.com/nginxinc/ansible-role-nginx-config/blob/main/molecule/plus/converge.yml)**|Use the NGINX config templating variables to create an NGINX Plus config|
|**[`stable_push/converge.yml`](https://github.com/nginxinc/ansible-role-nginx-config/blob/main/molecule/stable_push/converge.yml)**|Install NGINX using the stable branch and push a preexisting config from your system to your NGINX instance|

Do note that if you install this repository via Ansible Galaxy, you will have to replace the role variable in the sample playbooks from `ansible-role-nginx-config` to `nginxinc.nginx_config`.

## Other NGINX Ansible Collections and Roles

You can find the Ansible NGINX Core collection of roles to install and configure NGINX Open Source, NGINX Plus, and NGINX App Protect [here](https://github.com/nginxinc/ansible-collection-nginx).

You can find the Ansible NGINX role to install NGINX [here](https://github.com/nginxinc/ansible-role-nginx).

You can find the Ansible NGINX App Protect role to install and configure NGINX App Protect [here](https://github.com/nginxinc/ansible-role-nginx-app-protect).

You can find the Ansible NGINX Controller collection of roles to install and configure NGINX Controller [here](https://github.com/nginxinc/ansible-collection-nginx_controller).

You can find the Ansible NGINX Unit role to install NGINX Unit [here](https://github.com/nginxinc/ansible-role-nginx-unit).

## License

[Apache License, Version 2.0](https://github.com/nginxinc/ansible-role-nginx-config/blob/main/LICENSE)

## Author Information

[Alessandro Fael Garcia](https://github.com/alessfg)

&copy; [F5 Networks, Inc.](https://www.f5.com/) 2020
