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
../scripts/checkout
../scripts/update
```

```sh
# Install icu4c 64
brew reinstall https://raw.githubusercontent.com/Homebrew/homebrew-core/44895fce117ab92a44d733315b702c48dbb3898d/Formula/icu4c.rb
docker-compose up -d
symfony serve
```

### Get remote data

Edit `.env.local`:

```sh
REMOTE_HOST=ereolen.dk
REMOTE_DB_DUMP_CMD='drush --root=/data/www/ereolen_dk/htdocs --uri=ereolen.dk sql-dump --structure-tables-list="cache,cache_*,history,search_*,sessions,watchdog"'
REMOTE_PATH='/data/www/ereolen_dk/htdocs/sites/default/files'
REMOTE_EXCLUDE=(ting styles advagg_*)
LOCAL_PATH='web/sites/default/files'
```

Get database:

```sh
itkdev-docker-compose sync:db
```

## eReolen Go!

```sh
cd ereolengo
../scripts/checkout
../scripts/update
```
