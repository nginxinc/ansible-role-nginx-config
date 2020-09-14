# Changelog

## 0.2.0 (September 13, 2020)

BREAKING CHANGES:

*   The process to configure modules has changed. Instead of manually setting the modules you want to install to `true` or `false`, you will now have to use either:
    *   A newly introduced top level list variable, `nginx_config_modules`.
    *   A newly introduced list variable within your main NGINX config template, `nginx_config_main_template.modules`.

Make sure you only use one variable or the other, since they will overwrite each other. This change will simplify adding future supported modules to this role, and allows you to include any external modules you may wish in your NGINX config.

FEATURES:

*   A new variable has been introduced:
    *   `nginx_debug_tasks` -- Print task related information to give you a better insight into the current progress of the role.
*   Improved tasks naming conventions.
*   Add Alpine `3.12` to the list of supported platforms.
*   Remove Alpine `3.8` from the list of supported platforms .

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
