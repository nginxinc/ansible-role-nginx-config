Ansible NGINX Role
==================

This role configures NGINX Open Source and NGINX Plus on your target host.

**Note:** This role is still in active development. There may be unidentified issues and the role variables may change as development continues.

Requirements
------------

**Ansible**

This role was developed and tested with [maintained](https://docs.ansible.com/ansible/latest/reference_appendices/release_and_maintenance.html#release-status) versions of Ansible. Backwards compatibility is not guaranteed.

Instructions on how to install Ansible can be found in the [Ansible website](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html).

**Molecule**

Molecule is used to test the various functionailities of the role. Instructions on how to install Molecule can be found in the [Molecule website](https://molecule.readthedocs.io/en/latest/installation.html).

Installation
------------

**Ansible Galaxy**

Use `ansible-galaxy install nginxinc.nginx_config` to install the latest stable release of the role on your system.

**Git**

Use `git clone https://github.com/nginxinc/ansible-role-nginx-config.git` to pull the latest edge commit of the role from GitHub.

Platforms
---------

The NGINX Ansible role supports all platforms supported by [NGINX Open Source](https://nginx.org/en/linux_packages.html#mainline) and [NGINX Plus](https://www.nginx.com/products/technical-specs/):

**NGINX Open Source**

```yaml
Alpine:
  - 3.9
  - 3.10
  - 3.11
  - 3.12
CentOS:
  - 6
  - 7
  - 8
Debian:
  - stretch
  - buster
FreeBSD:
  - 11.2+
  - 12
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
  - focal
```

**NGINX Plus**

```yaml
Alpine:
  - 3.9
  - 3.10
  - 3.11
  - 3.12
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
  - focal
```

Role Variables
--------------

This role has multiple variables. The descriptions and defaults for all these variables can be found in the **`defaults/main`** directory in the following files:

-   **[defaults/main/main.yml](https://github.com/nginxinc/ansible-role-nginx-config/blob/master/defaults/main/main.yml):** NGINX simple config variables
-   **[defaults/main/template.yml](https://github.com/nginxinc/ansible-role-nginx-config/blob/master/defaults/main/template.yml):** NGINX config template variables
-   **[defaults/main/upload.yml](https://github.com/nginxinc/ansible-role-nginx-config/blob/master/defaults/main/upload.yml):** NGINX config/HTML/SSL upload variables

Example Playbooks
-----------------

Working functional playbook examples can be found in the **`molecule/common`** directory in the following files:

-   **[molecule/common/playbooks/cleanup_module_converge.yml](https://github.com/nginxinc/ansible-role-nginx-config/blob/master/molecule/common/playbooks/cleanup_module_converge.yml):** Cleanup an NGINX config and configure NGINX supported modules
-   **[molecule/common/playbooks/default_converge.yml](https://github.com/nginxinc/ansible-role-nginx-config/blob/master/molecule/common/playbooks/default_converge.yml):** Use the NGINX config templating variables to create an NGINX config
-   **[molecule/common/playbooks/stable_push_converge.yml](https://github.com/nginxinc/ansible-role-nginx-config/blob/master/molecule/common/playbooks/stable_push_converge.yml):** Install NGINX using the stable branch and push a preexisting config from your system to your NGINX instance

Do note that if you install this repository via Ansible Galaxy, you will have to replace the role variable in the sample playbooks from `ansible-role-nginx-config` to `nginxinc.nginx_config`.

Other NGINX Roles
-----------------

You can find an Ansible collection of roles to help you install and configure NGINX Controller [here](https://github.com/nginxinc/ansible-collection-nginx_controller)

You can find an Ansible role to help you install and configure NGINX App Protect [here](https://github.com/nginxinc/ansible-role-nginx-app-protect)

License
-------

[Apache License, Version 2.0](https://github.com/nginxinc/ansible-role-nginx-config/blob/master/LICENSE)

Author Information
------------------

[Alessandro Fael Garcia](https://github.com/alessfg)

&copy; [NGINX, Inc.](https://www.nginx.com/) 2020
