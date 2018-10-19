# Debugging

- XDebug supported
- PhpStorm worked well
- `disable_xdebug`, `enable_xdebug`
- Docs: <https://ddev.readthedocs.io/en/latest/users/step-debugging/>


Notes:
- We use PhpStorm in our shop, and xdebug for it worked well, in terms of normal operation.  
- (Talk about in-container commands `disable_xdebug` and `enable_xdebug`)
- Getting drush scripts to recognize was a bit of a pain, and I'm not going to go into it now, since I'm not sure which parts of my solution are signal and which are noise, but I can discuss it after if anyone is interested.

Extended notes:
**PHP Storm Configuration:**

1. Ensure you have a server set up for your project:
   - In PHP storm, go to `Settings` > `Languages & Frameworks` > `PHP` > `Servers`
   - Ensure a server is listed with the same name/host as the DDEV site you have set up.
   - Ensure `Use path mapping` is checked.
   - Set the path for your repository root to `/var/www/html`
     - Set any other paths accordingly (eg `/vendor`, `/public_html`, etc)
1. Run the following from within DDEV SSH:
   ```bash
   export PHP_IDE_CONFIG="serverName=Some_Name"
   ```
   - Replace `Some_Name` with the name of the server declared in PHP Storm
     (`Settings` > `Languages & Frameworks` > `PHP` > `Servers`)

** PHP Storm Troubleshooting: **

You will likely run into debug errors when running `drush` or `composer`
if you have debugging turned on in PHP storm:

For example:
 - `Remote file path '/home/.composer/vendor/drush/drush/drush' is not
   mapped to any file path in project`
 - `Remote file path '/usr/local/bin/composer' is not mapped to any file
   path in project`

To alleviate this:
 1. Click the `Click to set up path mappings` link provided in the debug window,
    (or go to `Settings` > `Languages & Frameworks` > `PHP` > `Servers`)
 1. Find the project instance of the error in question (eg: For `Drush`
    find the `vendor/drush` folder)
 1. On the right side in the `Absolute path on the server` column, enter the
    path that xdebug was looking for, relative to the local folder.
    - eg: For `Drush`, the error was complaining about not finding:
      `/home/.composer/vendor/drush/drush/drush`, therefore on the
      `vendor/drush` local folder, put in `/home/.composer/vendor/drush`
      in the absolute column and press `enter` then `Accept`
1. Now xdebug should work for `drush`, `composer`, etc

↓

# Other common commands

- `ddev list`: shows all running containers
- `ddev stop`: stops the current (or a named) container
- `ddev rm`: removes the current (or a named) container.

Notes:
- In general, for other commonly used commands, See the "Using the CLI" documentation.
A few specific items:

`ddev list` will show a list of all containers running, including other projects
where you have executed `ddev start`.
`ddev stop` or `ddev remove` can be used to stop those containers or remove them.
Note that removing containers will **not** destroy the associated MySQL data
unless you explicitly request it with a flag.

↓

# Other common commands

- `ddev import-db`: For importing an external db dumpfile
- `ddev exec`: Execute an in-container command from outside of it.
- `ddev sequelpro`: GUI access to the internal db (phpmyadmin also available).

<small>More: <https://ddev.readthedocs.io/en/latest/users/cli-usage></small>

↓

# Pain points

- Containers mysteriously hanging (resource issues?)
- CTRL+C during container upgrade process
- Cycling multiple containers at once (router is shared).

Notes:
We have run into a couple of pain points, as the tech is still in its infancy.  Things like breaking the update script in the middle of installation with ctrl+c, or containers mysteriously hanging, or trying to spin up one container while powering down another.

The most egregious problems had to be solved either by blowing away the container entirely at the docker level, and/or restarting the docker daemon, and rebuilding our container.  As these are local dev setups, this was not really an issue.

Some of these were ddev bugs, as it's still relatively new.  As new as it is, though, it's built on Docker, which is not, and it exposes a great deal, so proficient Docker hands would doubtless have found it much easier to debug this.

↓

# Postmortem

- What is DDEV
- Spinning up a basic container
- Container internals
- Customizing via config.yml
- Customizing via docker override

DDEV Quicksprint?:
<https://github.com/drud/quicksprint>

Questions / Comments / Thanks!

Notes:

So, a reminder of what we've discussed here.  What is DDEV and why you should care, basic usage, and a bit on the container internals, as well as some basic and more advanced customizations.

Just a note: In researching this, I also ran across the DDEV quicksprint repo, which they state is a "basic toolkit to get lots of people started in the same codebase in a short time" - tooling apparently meant for sprints.  I haven't evaluated it yet, but I will be.

Again, I'm not an expert in Docker, but that was sort of the point, and why I felt compelled to talk about this.  Hope it helps!