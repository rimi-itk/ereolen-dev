# ereolen-docker

```sh
./scripts/install-sites
```

To remove `docker-compose`, run

```sh
./scripts/undocker-sites
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

```sh
../scripts/checkout develop develop
```

```sh
docker-compose up -d
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

## Development

### With `mutagen`

```sh
brew install mutagen-io/mutagen/mutagen
cd ereolen
mutagen project start
```

Run `mutagen project terminate` to stop docker containers.


### With the [Symfony binary](https://symfony.com/download)

We need PHP 7.0 and want to keep this old shit to itself. Hence, we install it
inside an isolated Homebrew area
(cf. https://github.com/Homebrew/brew/blob/master/docs/Installation.md#multiple-installations):

Run `scripts/php7.0-install` to install PHP 7.0.

Start the show:

```sh
cd ereolen
symfony serve
```

#### Utility scripts

Must be run from inside a site folder (e.g. `ereolen/web`):

```sh
scripts/drush
scripts/phpcs
scripts/phpcbf
```

## eReolen Go!

```sh
cd ereolengo
../scripts/checkout
../scripts/update
```
