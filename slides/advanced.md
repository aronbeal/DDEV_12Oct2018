# Advanced customization

- Extra services (container additions)
- Adding host files
- Other?

`docker-compose.override.yml`

Notes:
- Sometimes hooks not enough
- Mounting files from the host
- Extra services, container additions
- This is why we prefer ddev at the moment
- Example: Learning SQL versus learning ORM
- Most of it is just adding a new docker service declaration file, or a docker-compose override file, which is a docker, not a ddev construct.

↓

# Adding host files
## Example: composer authentication
```yml
# docker-compose.override.yml
version: '3.6'

services:
  web:
    volumes:
    - ~/.composer/auth.json:/home/.composer/auth.json
```

Notes:
It is useful to be able to run composer commands from within
DDEV, but you have to supply your credentials to Bitbucket in order
to gain access to restricted repositories.  If you have already
authenticated in the parent (see note below), then this authentication can be 
re-used in any container by mounting the `auth.json` file in your
composer root into the new container.

To do so, add (or modify)the following file: `[repo root]/.ddev/docker-compose.override.yml` 
in your project (*Note: this file may already exist for other reasons*):

