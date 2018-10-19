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
- .ddev/config.yaml
- .ddev/docker-compose.yaml

Notes:
Additional items created, in response to `ddev config` and `ddev start`: discuss.

- `config.yaml` in a bit
- `docker-compose.yml` is dynamically rebuilt - don't touch it.

↓

# Output (continued):

- `ddev_drush_settings.php`

```php
// Use the array format for D7+
$databases['default']['default'] = array(
'database' => "db",
'username' => "db",
'password' => "db",
'host' => "127.0.0.1",
'driver' => "mysql",
'port' => 32788,
'prefix' => "",
);
```

Notes:
- You'll also see a new file in your project, containing config settings for ddev.  This should be now referenced by default in the bottom of your settings.php.
- Within that file, you'll also see the database credentials ddev establishes, and that drupal will now use to connect to the DDEV db container that was spun up as part of this project.
- "db", in this case, is the docker service name, visible in the docker-compose file.  For our purposes, we've never had to modify any of this; the default stack was fine.