# contao-docker

## Migration

### Step one
- Navigate to the `docker/migration` directory
- Run `docker-compose -f step-1.yml up -d`
- Open the [contao manager](http://localhost:8080/contao-manager.phar.php) in a browser
- Enter the desired username/password and click on `Create Account`
- Wait for the system check to finish, then click on `Setup`
- Choose `Contao 4.9 (Long Term Support)` as `Version` and `Minimal Installation (Core only)` as `Initial Setup`, click `Finish`
- Wait until `composer` installed all dependencies
- Copy the generated files to the host with `docker cp -a -L contao:/var/www/html/contao ./`
- Complete step one with `docker-compose -f step-1.yml down`

### Step two
- Copy `.env.example` to `.env`
- Enter the desired mariadb config in `.env`
- Copy the old `.sql` dump to the `initdb` directory
- Copy the old `files` and `templates` directories to the `contao` directory
- Run `docker-compose -f step-2.yml up -d`
- Open the [contao manager](http://localhost:8080/contao-manager.phar.php) in a browser
- Login with the credentials from step one
- Click on `Update Database` and `Accept license`
- Enter a new password (Can be the same as for the cantao manager)
- Set host to `mariadb` and the rest with the mariadb credentials from `.env`, then click `Save settings`
- Contao will check for needed database migrations, when finished click `Update database`
- If there are still actions selected with a checkmark click `Update database` again, repeat until there are no checkmarks selected
- Navigate to the [cantao backend](http://localhost:8080/contao) and login with your credentials
- Install any needed packages in the [contao manager](http://localhost:8080/contao-manager.phar.php)
- Open [phpmyadmin](http://localhost:8081) in a browser and export a `.sql` dump of the `mariadb` database
- Complete the migration with `docker-compose -f step-2.yml down`

You now have the migrated `contao` directory and the `.sql` dump

## Change db adress for sidecar deployment:
- Open a shell in the `fpm` container
- Install nano
    - `apk add nano`
- Set `database_host` to `127.0.0.1`
    - `nano config/parameters.yml`
- Clear cache
    - `php vendor/bin/contao-console cache:clear --no-warmup`
- Rebuild cache
    - `php vendor/bin/contao-console cache:warmup`
- Fix permissions
    - `chown -R 1000:1000 var/cache`