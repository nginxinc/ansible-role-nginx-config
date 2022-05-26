# Changelog

## 0.5.2 (Unreleased)

BUG FIXES:

Improve the NGINX main config defaults to bring them closer to the standard NGINX defaults on a fresh NGINX install.

## 0.5.1 (April 6, 2022)

FEATURES:

Rename all modules to use the fully qualified collection name (FQCN) per Ansible guidelines.

ENHANCEMENTS:

* Bump the Ansible `community.general` collection to `4.7.0` and `community.docker` collection to `2.3.0`.
* Add labels to loops in `tasks/config/template-config.yml` to reduce amount of output data.
* Implement `gunzip`, `map`, `mirror`, `realip` and `split_clients` modules into `http` templates.
* Streamline configuring SELinux.
* Update Dependabot to trigger updates at the same time across all NGINX core roles at the same time and to avoid triggering release drafter on GitHub actions dependency updates.

BUG FIXES:

Ansible check mode runs will no longer fail if NGINX has not yet been installed.

## 0.5.0 (February 17, 2022)

BREAKING CHANGES:

* Remove parameters deprecated in release `0.4.0`. To recap, these are `nginx_config_main_upload_*`, `nginx_config_upload_html_*`, and `nginx_config_stream_upload_*`. Use `nginx_config_upload` instead.
* Refactor all the `stream` Jinja2 templates!:
  * Each NGINX module is now contained within its own templating file. Macros are then used, in turn, to import each respective module template into a top level template file.
  * This avoids confusing and unnecessary code duplication, as well as hard to maintain code.
  * You will notice that the overall structure of your NGINX config now follows a very simple dictionary structure where each top level key corresponds to an NGINX module. Top level lists are used when dealing with `servers`:

    ```yaml
    core:
      root: /usr/share/nginx/html
    proxy:
      set_header: []
    servers:
      - core: {}
        proxy: {}
    ```

  * Check [`defaults/main/template.yml`](https://github.com/nginxinc/ansible-role-nginx-config/blob/main/defaults/main/template.yml) and [`molecule/default/converge.yml`](https://github.com/nginxinc/ansible-role-nginx-config/blob/main/molecule/default/converge.yml) for examples!
  * These changes follow in the footsteps of the `http` Jinja2 refactor introduced in the `0.4.0` release. If you want more information on how to port your `stream` configurations, the release notes/changelog for `0.4.0` are a good place to start.
* Replace `conf_file_name` and `conf_file_location` with `deployment_location` inside `nginx_config_stream_template`.
* Replace `html_file_name` and `html_file_location` with `deployment_location` inside `nginx_config_html_demo_template`.

FEATURES:

* Add `backup` variable to template and upload parameters. Set to `false` if you don't want to keep backups of your previous NGINX config files.
* Automatically create a NGINX `client_body_temp_path` directory if your NGINX config uses the directive.

ENHANCEMENTS:

Bump the Ansible `community.general` collection to `4.4.0` and `community.docker` collection to `2.1.1`.

BUG FIXES:

* Fix a bug when using a single `custom_directives` entry and the http template.
* Fix check mode issue when running with SELinux enabled. Role no longer reports a change in check mode when setting the host to permissive mode.
* Fix typo in the REST API template.
* Fix incorrect REST API and status log variable names in [`defaults/main/template.yml`](https://github.com/nginxinc/ansible-role-nginx-config/blob/main/defaults/main/template.yml).
* Fix bugged conditional check in the `http/ssl.j2` Jinja2 template.

## 0.4.2 (October 28, 2021)

BUG FIXES:

* Dictionaries are a sequence per Jinja2 contrary to Python's defaults (dictionaries are not a sequence in Python). The template conditionals assumed the latter.
* NAP DoS monitor directive would fail if some variables were commented out.
* NGINX listen `so_keepalive` parameter was not working as intended when setting specific values.
* Make sure all template objects are properly transformed into strings before doing Jinja2 operations.
* Remove unnecessary parentheses.

## 0.4.1 (October 25, 2021)

BUG FIXES:

* Fix issue where your `deployment_location` directory would not be properly created due to an outdated variable.
* Remove duplicated brace in `http/auth.j2`.

## 0.4.0 (October 19, 2021)

This is a very **big** release which fundamentally refactors the whole NGINX configuration templating engine. Almost all of the templates have undergone some breaking changes. Please take extra caution when upgrading your environment to this release and make sure you test any required changes before using the role in any potential production environments.

Efforts have been made to thoroughly test all these changes and make sure they work as intended, but due to the magnitude of the refactoring work, there will be some bugs that have escaped our tests. If you find any, please open an issue or PR through the usual channels.

DEPRECATION WARNINGS:

The `nginx_config_main_upload_*`, `nginx_config_upload_html_*`, and `nginx_config_stream_upload_*` parameters have been deprecated in favor of a newly introduced parameter, `nginx_config_upload` (previously `nginx_config_snippet_upload_*`). The new parameter provides greater flexibility in configuring your upload settings in addition to simplifying the upload Ansible tasks. The deprecated parameters will be removed in the next major release (0.5.0), due December 2021.

BREAKING CHANGES:

General updates:

* Rename upload related variables:
  * Rename the `nginx_config_snippet_upload_*` parameters to `nginx_config_upload_*` (check `defaults/main/upload.yml` for an example).
  * Rename the `nginx_config_html_upload_*` parameters to `nginx_config_upload_html_*`.
  * Rename the `nginx_config_ssl_upload_*` parameters to `nginx_config_upload_ssl_*`.
* Tweak the `nginx_config_html_upload` and `nginx_config_ssl_upload` parameters to use a list instead of a single `src` and `dest` value (check `defaults/main/upload.yml` for an example).

Template engine updates:

* Refactor all the `http` Jinja2 templates!:
  * Each NGINX module is now contained within its own templating file. Macros are then used, in turn, to import each respective module template into a top level template file.
  * This avoids confusing and unnecessary code duplication, as well as hard to maintain code.
  * You will notice that the overall structure of your NGINX config now follows a very simple dictionary structure where each top level key corresponds to an NGINX module. Top level lists are used when dealing with `servers` and `locations`:

    ```yaml
    core:
      root: /usr/share/nginx/html
    proxy:
      set_header: []
    servers:
      - core: {}
        proxy: {}
        locations:
          - core: {}
            proxy: {}
    ```

  * Check [`defaults/main/template.yml`](https://github.com/nginxinc/ansible-role-nginx-config/blob/main/defaults/main/template.yml) and [`molecule/default/converge.yml`](https://github.com/nginxinc/ansible-role-nginx-config/blob/main/molecule/default/converge.yml) for examples!
* Refactor the base config templates to simplify the creation of templates as well as development and maintenance moving forward:
  * Modify `servers`, `servers.listen`, `server.locations`, `upstream` and `upstream.servers` from nested dictionaries in the `http` and `stream` configuration templates to lists.
  * Remove/merge the `web_server` and `reverse_proxy` nested dictionary keys from the HTTP templates. These often lead to confusing and unnecessary code duplication and hard to maintain code. To update your templates, remove both keys and adjust your spacing accordingly.
  * Replaced `conf_file_name` and `conf_file_location` with a single variable, `deployment_location`.
  * All config related parameters now live under the `config` key in both the core/main and HTTP templates.
  * Modify the `nginx_config_html_demo_template` variable from a nested dictionary to a list.
* Refactor the `nginx_config_main_template` to now include all the respective `core` and `events` directives. The following variables have changed:
  * `http_enable` no longer exists, neither does `http_settings`. You can still use `http.include` to include files within the `http` context.
  * `stream_enable` no longer exists, neither does `stream_settings`. You can still use `stream.include` to include files within the `stream` context.
* Refactor the `upstream` HTTP config template into its own separate file. All the `upstream` module directives are now included. The following variables have changed:
  * `port` is no longer supported. Instead, include the port as part of your `address`.
  * `lb_method` is no longer supported. Instead, you will have to specifically set the method you want to use.
  * `zone_name` and `zone_size` have been modified into a dictionary.
  * `sticky_cookie` is no longer supported as is. You will now have to configure the respective `sticky_cookie` values.
  * The `health_check` parameter within the `server` dictionary is no longer supported. Instead, manually set `max_fails` and `fail_timeout`.
* Refactor various individual variables into the `core` HTTP config template. All the `core` module directives are now included. The following variables are now included in the `core` dictionary:
  * `alias`, `client_max_body_size`, `error_log`, `error_page`, `include`, `index`, `keepalive_timeout`, `listen`, `root`, `send_file`, `server_name`, `server_names_hash_bucket_size`, `server_names_hash_max_size`, `server_tokens`, `tcp_nodelay`, `tcp_nopush`, `try_files`
  * `listen.port` is now `listen.address`, and `listen.opts` no longer exists (there are now individual keys for each `listen` parameter).
* Refactor the `ssl` HTTP config template into its own separate file. All the `ssl` module directives are now included. Almost all variables have changed:
  * All `ssl` variables still live within an `ssl` dictionary, but the names have changed to reflect the official NGINX directive names.
  * `ssl` configs are now supported within both the `http` and `server` contexts.
* Refactor both the `app_protect_waf` and `app_protect_dos` modules into a single file:
  * The `app_protect` dictionary now has the `app_protect_waf` key.
  * `app_protect_global` directives are now found inside the `app_protect_waf` dictionary too.
* Refactor the `proxy` HTTP config template into its own separate file. All the `proxy` module directives are now included. All variables have changed:
  * All `proxy_*` related variables now live under the `proxy` dictionary key. You can specify the `proxy` dictionary key inside the `http`, `server`, and `location` contexts.
  * Removed the `nginx_config_main_template.http_settings.cache` dictionary variable. Use `nginx_config_http_template.*.proxy.cache_path` instead.
  * Removed the `location.websocket` variable. Use `location.proxy.set_header` instead:

    ```yaml
    proxy:
      set_header:
        - field: Upgrade
          value: $http_upgrade
        - field: Connection
          value: Upgrade
    ```

* Combine the `grpc_global` directives with the `grpc` directives.
* Refactor the `auth` HTTP config template into its own separate `auth` modules file. All the various `auth` related module directives including all `auth_jwt` directives are now available. All variables have changed:
  * All the various `auth` variables now live within their respective `auth` dictionaries.
  * `auth` configs are now supported within the `http`, `server`, and `location` contexts.
* Refactor the `autoindex` HTTP config template into its own separate file `modules` file and added missing `autoindex` module directives. All variables have changed:
  * The `autoindex` directives now live within the `autoindex` dictionary.
  * The `autoindex` dictionary now lives in the HTTP template config instead of the Main template config.
* Refactor the `add_headers` dictionary into a `headers` dictionary that now includes all the `headers` HTTP config directives:
  * The `add_headers` directive now lives within the `headers` dictionary.
* Refactor the `keyval` directives into its own template config that now includes all the `keyval` HTTP module directives:
  * Both `keyval` directives now live within the `keyval` dictionary.
  * The `keyval` dictionary now lives in the HTTP template config instead of the Main template config.
* Refactor `server.health_check_plus` into its own dictionary that now includes all the `health_check` module directives (check [`defaults/main/template.yml`](https://github.com/nginxinc/ansible-role-nginx-config/blob/main/defaults/main/template.yml) for examples).
* Refactor the `limit_req` directive into its own dictionary:
  * The `limit_req` directives now live within the `limit_req` dictionary.
  * The `limit_req` dictionary now lives in the HTTP template config instead of the Main template config.
* Refactor the `access_log` and `log_format` directives into a `log` dictionary that now includes all the `log` module directives:
  * An `access` and `format` directive now lives within the `log` dictionary.
  * The `log` dictionary HTTP context now lives in the HTTP template config instead of the Main template config.
* Refactor the `return` and `rewrite` directives into their own dictionary that now includes all the `rewrite` HTTP module directives:
  * The `rewrites` directive has transitioned from a list of one liners

    ```yaml
    rewrites:
      - (.*).html(.*) $1$2
    ```

    to

    ```yaml
    rewrites:
      - regex: (.*).html(.*)
        replacement: $1$2
    ```

  * The `return` directive has transitioned from a slightly complex dictionary structure (wherein the `location` variable didn't necessarily have any effect)

    ```yaml
    returns:
      return301:
        location: ^~ /old-path
        code: 301
        value: http://$host/new-path
    ```

    to a slightly less complicated structure

    ```yaml
    return:  # 200 -- Alternatively you could also include a code here instead of fleshing out the dictionary.
      code: 200
      text: nginx
    ```

* Refactor the `sub_filter` directives into their own `sub_filter` dictionary that includes all the `sub_filter` HTTP module directives:
  * The only major difference is that one liners under the `sub_filters` dictionary key have changed from

    ```yaml
    sub_filters:
      - sub_filter 'server_hostname' '$hostname';
    ```

    to

    ```yaml
    sub_filters:
      - string: server_hostname
        replacement: $hostname
    ```

  * Removed the `server.http_demo_conf` dictionary. Use `server.sub_filters` instead:

    ```yaml
    sub_filter:
      sub_filters:
        - string: server_hostname
          replacement: $hostname
        - string: server_address
          replacement: $server_addr:$server_port
        - string: server_url
          replacement: $request_uri
        - string: remote_addr
          replacement: '$remote_addr:$remote_port'
        - string: server_date
          replacement: $time_local
        - string: client_browser
          replacement: $http_user_agent
        - string: request_id
          replacement: $request_id
        - string: nginx_version
          replacement: $nginx_version
        - string: document_root
          replacement: $document_root
        - string: proxied_for_ip
          replacement: $http_x_forwarded_for
    ```

  * The `sub_filter` dictionary HTTP context now lives in the HTTP template config instead of the Main template config.
* Rename some NGINX template config parameters to align with NGINX directive names:
  * Rename `html_file_location` to `root`.
  * Rename `html_file_name` to `index`.
* NGINX App Protect 3.2 supports multiple log destinations per scope. Changing the `security_log` variable from a dictionary to a list of objects in order to support this.
* NGINX App Protect 3.5 supports a new timeout directive which allows the user to configure the period of time between reconnect retries of the module to the web application firewall (WAF) engine. Added this as a supported directive.

FEATURES:

* Replace Ansible community distribution with Ansible base and add the necessary extra collections as a dependency requirement. For reference, these are:

    ```yaml
    ---
    collections:
      - name: community.general
        version: 3.8.0
      - name: ansible.posix
        version: 1.3.0
      - name: community.docker  # This collection is only used as part of the Molecule testing suite
        version: 1.10.0
    ```

* Explicitly list Jinja2 `2.11.3` as a requirement, as well as detail the minimum supported version (`2.11.x`).
* Implement Release Drafter.
* Add support for configuring NGINX App Protect DoS (Denial of Service) module and directives.
* Add support for configuring the NGINX Rest API module and the NGINX stub status module.

ENHANCEMENTS:

* Move the `gzip` HTTP config template into the `modules` file. It's a small module and did not warrant being in its own individual file.
* Update Ansible Lint to `5.1.3`, Molecule to `3.4.0`, yamllint to `1.26.3` and Docker Python SDK to `5.0.2`.
* Consolidate Molecule testing scenarios to address changes introduced in Ansible Lint `5.*`.
* Specify GitHub actions Ubuntu release.
* Minor GitHub template tweaks, including the creation of a SECURITY doc.
* Replace Molecule tests using Debian stretch with Debian buster (stretch has reached its EoL), and update list of supported platforms.
* Replace Ansible base with Ansible core. Ansible core will be the "core" Ansible release moving forward from Ansible `2.11`.
* Update GitHub actions to add a workflow dispatch option.
* Update GitHub actions `if` conditionals to use the `contains` function instead of checking for exact names.
* Remove Debian Buster from the `plus` Molecule scenario since it often fails in the GitHub Actions CI/CD pipeline.
* Replace "yes"/"no" boolean values with "true"/"false" to comply with YAML spec `1.2`.
* Ensure the default values for the `nginx.conf` template match the default values found on a fresh NGINX installation.
* Change Dependabot frequency from daily to weekly.
* Minor touch-up of GitHub actions workflows.

BUG FIXES:

* Add `state` parameter to package module in Molecule verification tests.
* In NGINX App Protect environments on SELinux enforced systems, the `nginx -t` handler fails when run from a directory that the NGINX process' user does not have access to.
* Fix missing GRPC boolean check in GRPC template.
* Fix `nginx_config_cleanup_paths` not working as intended.
* Fix issue with the `app_protect.j2` template that was causing the default values for `nginx.conf` to fail.

## 0.3.3 (January 28, 2021)

FEATURES:

* Add support for configuration snippets (use the `nginx_config_snippet_upload_*` parameters to configure it).
* Add support for Dependabot.

ENHANCEMENTS:

* Add support for NGINX GRPC directives.
* Add support for NGINX GZIP directives.
* Add support for upstream server `backup` parameter in http and stream template.
* Only run GitHub actions Galaxy CI/CD workflow when a new release is published.
* Update list of supported platforms.
* Update Ansible base to `2.10.5` and Ansible to `2.10.6`.

BUG FIXES:

* Address inconsistent types within Jinja templates.
* Add Jinja2 checks to all config template parameters to ensure that they are only included when appropriately defined.
* Fix edge case where `proxy_pass` is still required when using `grpc_pass`.

## 0.3.2 (January 11, 2021)

ENHANCEMENTS:

* The GitHub actions Molecule CI/CD workflow is no longer run on a new release (this is not necessary since it already runs on every push).
* The GitHub actions Molecule CI/CD workflow should now correctly avoid running 'Plus' related tests on external PRs.
* The `cleanup-config.yml` playbook has been slightly refactored and simplified.
* Update Ansible base to `2.10.4`, Ansible to `2.10.5`, Molecule to `3.2.2` and Docker Python SDK to `4.4.1`.
* Update copyright notice.

## 0.3.1 (December 22, 2020)

ENHANCEMENTS:

* Update Molecule to `3.2.1` and Docker Python SDK to `4.4.0`.
* Replace TravisCI with GitHub actions.

BUG FIXES:

* Switch to explicit boolean values in `sub_filter` defaults for `last_modified` and `since` in `nginx_config_main_template`. `"on"` and `"off"` values are treated as true instead of true/false when surrounded by double quotes. By always resorting to true/false we avoid unaccounted edge cases.
* Fix issue whereas SELinux state would not be correctly set back to `enforcing` when `nginx_config_selinux: true`.

## 0.3.0 (November 17, 2020)

BREAKING CHANGES:

* The default port of the status and REST API config is now `8080` and matches the CI Molecule test which already uses it. You can set `nginx_config_status_port` to another value if desired.
* The allow/deny directives for `nginx_config_status` and `nginx_config_rest_api` now take a list instead of a single value.
* The default `nginx_config_*_log` values have changed to `nginx_config_*_access_log` and no longer have a default value of `off`. Set the respective variables to `false` to preserve the previous behaviour.

ENHANCEMENTS:

* Add survey to README.
* Improve README structure and use tables where relevant.
* Update Ansible (now Ansible base) to `2.10.3`, Ansible (now Ansible Community Distribution) to `2.10.3`, Ansible Lint to `4.3.7`, Molecule to `3.1.5`, and yamllint to `1.25.0`.
* Improve templating of stub status and REST API config.

BUG FIXES:

* Prevent TravisCI from trying to build (and failing) NGINX Plus images on external PRs.
* Fix naming for SELinux facts dictionary.
* Correctly import `app_protect` global directives in template.
* Role now runs correctly when using Ansible's check mode.
* Fix issue with access log in stub status and REST API config template not being properly parsed.

## 0.2.0 (September 24, 2020)

BREAKING CHANGES:

The process to configure modules has changed. Instead of manually setting the modules you want to install to `true` or `false`, you will now have to use either:

* A newly introduced top level list variable, `nginx_config_modules`.
* A newly introduced list variable within your main NGINX config template, `nginx_config_main_template.modules`.

Make sure you only use one variable or the other, since they will overwrite each other. This change will simplify adding future supported modules to this role, and allows you to include any external modules you may wish in your NGINX config.

FEATURES:

* Support for all NGINX App Protect directives has been added. You can find details on the supported directives on `defaults/main/template.yml`. This is the first module to be included using J2 macros. Expect to slowly see a refactor of various modules to use macros where possible.
* Add Alpine `3.12` to the list of supported platforms.
* Remove Alpine `3.8` from the list of supported platforms .
* Add NGINX Plus tests to TravisCI

ENHANCEMENTS:

* Added handlers to check for NGINX syntax validity and fail if any errors are detected.
* Switch to using `ansible_facts` wherever possible.
* Improved tasks naming conventions.
* Update Ansible to `2.9.13` and Ansible Lint to `4.3.5`.
* Explicitly defined `mode` in relevant tasks.
* Improve configuration templating capabilities:
  * Allow setting `access_log`/`access_log_location` to `off`.
  * Add IP restriction for web servers

BUG FIXES:

An empty `nginx_config_cleanup_files` will no longer cause `nginx_config_cleanup` related tasks to fail.

## 0.1.0 (August 19, 2020)

Initial release of the NGINX Config role. Contains all NGINX Config related features previously available on the [NGINX Ansible role](https://github.com/nginxinc/ansible-role-nginx).
