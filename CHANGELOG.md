# Changelog

## 0.1.1 (Unreleased)

ENHANCEMENTS:

*   Update Ansible to `2.9.13` and Ansible Lint to `4.3.4`.
*   Explicitly defined `mode` in relevant tasks.
*   Improve configuration templating capabilities:
    *   Allow setting `access_log`/`access_log_location` to `off`.
    *   Add IP restriction for web servers

BUG FIXES:

*   An empty `nginx_config_cleanup_files` will no longer cause `nginx_config_cleanup` related tasks to fail.

## 0.1.0 (August 19, 2020)

Initial release of the NGINX Config role. Contains all NGINX Config related features previously available on the [NGINX Ansible role](https://github.com/nginxinc/ansible-role-nginx).
