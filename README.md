[![Ansible Galaxy](https://img.shields.io/badge/galaxy-nginxinc.nginx__config-5bbdbf.svg)](https://galaxy.ansible.com/nginxinc/nginx_config)
[![Molecule CI/CD](https://github.com/nginxinc/ansible-role-nginx-config/workflows/Molecule%20CI/CD/badge.svg)](https://github.com/nginxinc/ansible-role-nginx-config/actions)
[![License](https://img.shields.io/badge/License-Apache--2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Project Status: Active – The project has reached a stable, usable state and is being actively developed.](https://www.repostatus.org/badges/latest/active.svg)](https://www.repostatus.org/#active)
[![Community Support](https://badgen.net/badge/support/community/cyan?icon=awesome)](/SUPPORT.md)
[![Contributor Covenant](https://img.shields.io/badge/Contributor%20Covenant-2.1-4baaaa.svg)](/CODE_OF_CONDUCT.md)

# 👾 *Help make the NGINX config Ansible role better by participating in our [survey](https://forms.office.com/Pages/ResponsePage.aspx?id=L_093Ttq0UCb4L-DJ9gcUKLQ7uTJaE1PitM_37KR881UM0NCWkY5UlE5MUYyWU1aTUcxV0NRUllJSC4u)!* 👾

# Ansible NGINX Configuration Role

This role configures NGINX Open Source and NGINX Plus on your target host.

> [!IMPORTANT]
> This role is still in active development. There may be unidentified issues and the role variables may change as development continues.

## Role Requirements

### Ansible

If you want to use this role, you will need to use a supported version of Ansible core and Jinja2 as well as a few Ansible collections.

For ease of use, you can install and/or upgrade Ansible core, Jinja2, and the aforementioned Ansible collections by running the following four commands on your Ansible host:

```bash
pip install --upgrade -r https://raw.githubusercontent.com/nginxinc/ansible-role-nginx-config/main/.github/workflows/requirements/requirements_ansible.txt
curl -O https://raw.githubusercontent.com/nginxinc/ansible-role-nginx-config/main/.github/workflows/requirements/requirements_collections.yml
ansible-galaxy install --force -r requirements_collections.yml
rm -f requirements_collections.yml
```

This will also ensure you are deploying/running this role with a fully tested version of the aforementioned packages/collections.

#### Ansible core

- This role is developed and tested with [maintained](https://docs.ansible.com/ansible/devel/reference_appendices/release_and_maintenance.html) versions of Ansible core and Python.
- When using Ansible core, you will also need to install the following Ansible collections:

  ```yaml
  ---
  collections:
    - name: ansible.posix
      version: 1.5.4
    - name: community.general
      version: 9.0.1
    - name: community.docker # Only required if you plan to use Molecule (see below)
      version: 3.10.3
  ```

- Instructions on how to install Ansible core can be found in the [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#upgrading-ansible-from-version-2-9-and-older-to-version-2-10-or-later) docs.
- Instructions on how to install Ansible collections can be found in the [Ansible collections](https://docs.ansible.com/ansible/latest/collections_guide/collections_installing.html) guide.

> [!TIP]
> You can alternatively install the [Ansible community distribution](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#selecting-an-ansible-package-and-version-to-install) (what is still known Ansible -- instead of Ansible core) if you don't want to manage individual collections.

### Jinja2

- This role uses Jinja2 templates. Ansible core installs Jinja2 by default, but depending on your install and/or upgrade path, you might be running an outdated version of Jinja2. The minimum version of Jinja2 required for the role to properly function is `3.1`.
- Instructions on how to install Jinja2 can be found in the [Jinja2 website](https://jinja.palletsprojects.com/en/3.1.x/intro/#installation).

### Testing suite (Optional)

If you want to contribute to this role, you will also need to install Ansible Lint and Molecule.

#### Ansible Lint (Optional)

- Ansible Lint is used to lint the role for both Ansible best practices and potential Ansible/YAML issues.
- Instructions on how to install Ansible Lint can be found in the [Ansible Lint website](https://ansible.readthedocs.io/projects/lint/installing/).
- Once installed, using Ansible Lint is as easy as running:

  ```bash
  ansible-lint
  ```

- For ease of use, you can install and/or upgrade Ansible Lint by running the following command on your Ansible host:

  ```bash
  pip install -r https://raw.githubusercontent.com/nginxinc/ansible-role-nginx-config/main/.github/workflows/requirements/requirements_ansible_lint.txt
  ```

#### Molecule (Optional)

- Molecule is used to test the various functionalities of the role.
- Instructions on how to install Molecule can be found in the [Molecule website](https://molecule.readthedocs.io/en/latest/installation.html). *You will also need to install the Molecule plugins package and the Docker Python SDK.*
- To run the NGINX Plus/App Protect config Molecule tests, you must copy your NGINX Plus/App Protect license to the role's [`files/license`](https://github.com/nginxinc/ansible-role-nginx-config/blob/main/files/license/) directory.

  You can alternatively add your NGINX Plus/App Protect repository certificate and key to the local environment. Run the following commands to export these files as base64-encoded variables and execute the Molecule tests:

  ```bash
  export NGINX_CRT=$( cat <path to your certificate file> | base64 )
  export NGINX_KEY=$( cat <path to your key file> | base64 )
  molecule test -s plus
  ```

- For ease of use, you can install and/or upgrade Molecule, the Molecule plugins package, and the Docker Python SDK by running the following command on your Ansible host:

  ```bash
  pip install --upgrade -r https://raw.githubusercontent.com/nginxinc/ansible-role-nginx-config/main/.github/workflows/requirements/requirements_molecule.txt
  ```

## Role Installation

This role can be installed via either Ansible Galaxy (the Ansible community marketplace) or by cloning this repo. Once installed, you will need to include the role in your Ansible playbook using [the `roles` keyword, the `import_role` module, or the `include_role` module](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html#using-roles).

### Ansible Galaxy

To install the latest stable release of the role on your system, use:

```bash
ansible-galaxy install nginxinc.nginx_config
```

Alternatively, if you have already installed the role, you can update the role to the latest release by using:

```bash
ansible-galaxy install -f nginxinc.nginx_config
```

To use the role, include the following task in your playbook:

```yaml
- name: Configure NGINX
  ansible.builtin.include_role:
    name: nginxinc.nginx_config
```

### Git

To pull the latest edge commit of the role from GitHub, use:

```bash
git clone https://github.com/nginxinc/ansible-role-nginx-config.git
```

To use the role, include the following task in your playbook:

```yaml
- name: Configure NGINX
  ansible.builtin.include_role:
    name: <path/to/repo> # e.g. <roles/ansible-role-nginx-config> if you clone the repo inside your project's roles directory
```

## Platforms

The NGINX config Ansible role supports all platforms supported by [NGINX Open Source](https://nginx.org/en/linux_packages.html#mainline) and [NGINX Plus](https://www.nginx.com/products/technical-specs/).

> [!NOTE]
> You should be able to use this role to configure any NGINX installation -- wherever/however it's been installed -- at your own risk. Any potential bugs with the role involving unsupported installation methods/platforms will be addressed in a best effort manner and might be outright dismissed.*

## Role Variables

This role has multiple variables. The descriptions and defaults for all these variables can be found in the **[`defaults/main/`](/defaults/main/)** directory in the following files:

| Name | Description |
| ---- | ----------- |
| **[`main.yml`](/defaults/main/main.yml)** | NGINX simple config variables |
| **[`selinux.yml`](/defaults/main/selinux.yml)** | Set up SELinux to allow the necessary connections to your NGINX setup |
| **[`template.yml`](/defaults/main/template.yml)** | NGINX config template variables |
| **[`upload.yml`](/defaults/main/upload.yml)** | NGINX config/HTML/SSL upload variables |

## Example Playbooks

Working functional playbook examples can be found in the **[`molecule/`](/molecule/)** directory in the following files:

| Name | Description |
| ---- | ----------- |
| **[`api/converge.yml`](/molecule/api/converge.yml)** | Configure the NGINX Plus API and live metrics dashboard |
| **[`cleanup_config/converge.yml`](/molecule/cleanup_config/converge.yml)** | Cleanup an NGINX config |
| **[`complete/converge.yml`](/molecule/complete/converge.yml)** | Test all NGINX directives are correctly templated |
| **[`complete_plus/converge.yml`](/molecule/complete_plus/converge.yml)** | Test all NGINX Plus specific directives are correctly templated |
| **[`default/converge.yml`](/molecule/default/converge.yml)** | Configure NGINX with a config as close as possible to the default config |
| **[`push_config/converge.yml`](/molecule/push_config/converge.yml)** | Push a preexisting NGINX config from your system to your NGINX instance |
| **[`reverse_proxy/converge.yml`](/molecule/reverse_proxy/converge.yml)** | Configure NGINX as a reverse proxy between two web servers |
| **[`stub_status/converge.yml`](/molecule/stub_status/converge.yml)** | Configure the NGINX Open Source stub status metrics |
| **[`web_server/converge.yml`](/molecule/web_server/converge.yml)** | Configure NGINX as a web server |

> [!NOTE]
> If you install this repository via Ansible Galaxy, you will need to replace the `include_role` variable in the example playbooks from `ansible-role-nginx-config` to `nginxinc.nginx_config`.

## Other NGINX Ansible Collections and Roles

You can find the Ansible NGINX Core collection of roles to install and configure NGINX Open Source, NGINX Plus, and NGINX App Protect [here](https://github.com/nginxinc/ansible-collection-nginx).

You can find the Ansible NGINX role to install NGINX OSS and NGINX Plus [here](https://github.com/nginxinc/ansible-role-nginx).

You can find the Ansible NGINX App Protect role to install and configure NGINX App Protect WAF and NGINX App Protect DoS [here](https://github.com/nginxinc/ansible-role-nginx-app-protect).

You can find the Ansible NGINX Unit role to install NGINX Unit [here](https://github.com/nginxinc/ansible-role-nginx-unit).

## License

[Apache License, Version 2.0](/LICENSE)

## Author Information

[Alessandro Fael Garcia](https://github.com/alessfg)

&copy; [F5, Inc.](https://www.f5.com/) 2020 - 2024
