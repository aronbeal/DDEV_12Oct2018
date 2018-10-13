# Output: 

- web container
- db container
- dba container
- router

Notes: 
What has this given us:
- web container with php internals, including a global drush and drupal console
- db container, referencable internally only (by default)
- dba container, for outside administration of the db
- a router to connect them all.  Note that this router is shared between all ddev projects.

↓


# Output (continued): 

- .ddev folder
- .ddev/config.yml
- .ddev/docker-compose.yml

Notes:
Additional items created, in response to `ddev config` and `ddev start`: discuss.

- `config.yml` in a bit
- `docker-compose.yml` is dynamically rebuilt - don't touch it.
