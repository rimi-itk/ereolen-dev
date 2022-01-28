# ereolen-dev

eReolen development with [Symfony binary](https://symfony.com/download).

Before starting development do what [Prerequisites and initial installation of
stuff](#prerequisites-and-initial-installation-of-stuff) tells you what to do.


## Development

```sh
cd ereolen
docker run --rm --interactive --tty --volume "$PWD":/app --volume "${COMPOSER_HOME:-$HOME/.composer}":/tmp itkdev/php7.0-fpm composer install
docker-compose up --detach
```

Pull remote database and files (cf. [Get remote data](#get-remote-data):

```sh
itkdev-docker-compose sync
```

```sh
# Make PHP 7.0 available
symfony local:php:refresh
# Start the development web server
symfony local:server:start
```

In a new shell (in the same directory):

```sh
cd web
# Export site url for use in Drush
export DRUSH_OPTIONS_URI=$(~/.symfony/bin/symfony run printenv SYMFONY_PROJECT_DEFAULT_ROUTE_URL)
symfony php ../vendor/bin/drush user:login
```

## Prerequisites and initial installation of stuff

We need PHP 7.0:

```sh
brew tap shivammathur/php
brew install shivammathur/php/php@7.0
```

And [ImageMagick](https://imagemagick.org/):

```sh
brew install imagemagick
```

Install sites:

```sh
./scripts/install-sites
```

## eReolen

```sh
cd ereolen
```

Edit `web/sites/default/settings.local.php` and insert:

```php
<?php

$databases['default']['default'] = [
 'database' => getenv('DATABASE_DATABASE') ?: 'db',
 'username' => getenv('DATABASE_USERNAME') ?: 'db',
 'password' => getenv('DATABASE_PASSWORD') ?: 'db',
 'host' => getenv('DATABASE_HOST') ?: 'mariadb',
 'port' => getenv('DATABASE_PORT') ?: '',
 'driver' => getenv('DATABASE_DRIVER') ?: 'mysql',
 'prefix' => '',
];
```

Check out the development branches:

```sh
../scripts/checkout develop develop
```

```sh
docker-compose up --detach
# Make PHP 7.0 available
symfony local:php:refresh
symfony local:server:start
```

```sh
../scripts/update
```

### Get remote data

Edit `.env.local`:

```sh
REMOTE_HOST=ereolen.dk
REMOTE_DB_DUMP_CMD='drush --root=/data/www/ereolen_dk/htdocs --uri=ereolen.dk sql-dump --structure-tables-list="cache,cache_*,history,search_*,sessions,watchdog"'
REMOTE_PATH='/data/www/ereolen_dk/htdocs/sites/default/files'
REMOTE_EXCLUDE=(advagg_* css ctools js languages resize styles ting)
LOCAL_PATH='web/sites/default/files'
```

Install `itkdev-docker-compose` from
[https://github.com/aakb/itkdev-docker](https://github.com/aakb/itkdev-docker)
and run

```sh
itkdev-docker-compose sync:db
```

## Building themes

Same procedure as documented in [Building
themes](https://github.com/ereolen/base#building-themes), but with a slightly
different path (`/app/web/` rather than `/app/`):

```sh
docker-compose run --rm node yarn --cwd /app/web/sites/all/themes/orwell install
docker-compose run --rm node yarn --cwd /app/web/sites/all/themes/orwell build
```

```sh
docker-compose run --rm node yarn --cwd /app/web/sites/all/themes/wille install
docker-compose run --rm node yarn --cwd /app/web/sites/all/themes/wille build
```

## Development

### With the [Symfony binary](https://symfony.com/download)

We need PHP 7.0:


```sh
brew tap shivammathur/php
brew install shivammathur/php/php@7.0
```

Start the show:

```sh
cd ereolen
docker-compose up --detach
# Make PHP 7.0 available
symfony local:php:refresh
symfony local:server:start
```

#### Utility scripts

Must be run from inside a site folder (e.g. `ereolen/web`):

```sh
../../scripts/drush
../../scripts/phpcs
../../scripts/phpcbf
../../scripts/symfony
```

## eReolen Go!

The same story, but in the `ereolengo` folder.
