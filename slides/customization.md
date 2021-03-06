> `.ddev/config.yml`

```yml
APIVersion: v1.2.0
name: some-dir
type: drupal8
docroot: web
php_version: "7.1"
webserver_type: nginx-fpm
router_http_port: "80"
router_https_port: "443"
xdebug_enabled: false
additional_hostnames: []
additional_fqdns: []
provider: default


# This config.yaml was created with ddev version v1.2.0 
# webimage: drud/ddev-webserver:v1.2.0
# dbimage: drud/ddev-dbserver:v1.2.0
# dbaimage: drud/phpmyadmin:v1.2.0
# However we do not recommend explicitly wiring these images into the
# config.yaml as they may break future versions of ddev.
# You can update this config.yaml using 'ddev config'.

# Key features of ddev's config.yaml:

# name: <projectname> # Name of the project, automatically provides
#   http://projectname.ddev.local and https://projectname.ddev.local

# type: <projecttype>  # drupal6/7/8, backdrop, typo3, wordpress, php

# docroot: <relative_path> # Relative path to the directory containing index.php.

# php_version: "7.1"  # PHP version to use, "5.6", "7.0", "7.1", "7.2"

# You can explicitly specify the webimage, dbimage, dbaimage lines but this
# is not recommended, as the images are often closely tied to ddev's' behavior,
# so this can break upgrades.

# webimage: <docker_image>  # nginx/php docker image.
# dbimage: <docker_image>  # mariadb docker image.
# dbaimage: <docker_image>

# router_http_port: <port>  # Port to be used for http (defaults to port 80)
# router_https_port: <port> # Port for https (defaults to 443)

# xdebug_enabled: false  # Set to true to enable xdebug and "ddev start" or "ddev restart"

# webserver_type: nginx-fpm  # Can be set to apache-fpm or apache-cgi as well

# additional_hostnames:
#  - somename
#  - someothername
# would provide http and https URLs for "somename.ddev.local"
# and "someothername.ddev.local".

# additional_fqdns:
#  - example.com
#  - sub1.example.com
# would provide http and https URLs for "example.com" and "sub1.example.com"
# Please take care with this because it can cause great confusion.

# upload_dir: custom/upload/dir
# would set the destination path for ddev import-files to custom/upload/dir.

# provider: default # Currently either "default" or "pantheon"
#
# Many ddev commands can be extended to run tasks after the ddev command is
# executed.
# See https://ddev.readthedocs.io/en/latest/users/extending-commands/ for more
# information on the commands that can be extended and the tasks you can define
# for them. Example:
#hooks:
# post-import-db:
#   - exec: drush cr
#   - exec: drush updb
```

An example of the `config.yml` file generated

Notes:

- ApiVersion and updating
- Project name needs hyphens, additional hostnames property
- nginx most common, but apache support now available, at least for *ix systems.
- xdebug_enabled is the default when starting
- 'provider' we don't use. Integration with hosting provider...

Image customization possible, but we haven't yet.

Common overrides in a bit.

→

# Customization: 
## Subdomain

```yml
APIVersion: v1.2.0
name: some-dir
type: drupal7
docroot: my_custom_webroot
php_version: "5.6"
webserver_type: nginx-fpm
router_http_port: "80"
router_https_port: "443"
xdebug_enabled: false
additional_hostnames:
- local.some-dir
```
Will be available at `local.some-dir.ddev.local`, in addition to other domains

Notes:
- Some of our sites are only accessible using a subdomain.
- Attn: customized docroot
- Attn: customized php version
- Attn: additional_hostnames
  - Will be available at local.some-dir.ddev.com

→

# Customization (hook example): 
## Post-start hook

```yml
# config.yaml hooks section
hooks:
  post-start:
  # Clear cache on container start.
  # Not very exciting, but it demonstrates the 
  # principle.
  - exec: bash -c "cd /var/www/html/public_html; drush cr"
```

Full list of hooks at <https://ddev.readthedocs.io/en/latest/users/extending-commands/#supported-command-hooks>

Notes:
- Simplified demonstration
- Note the "bash -c" bit.
