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
 'database' => 'db',
 'username' => 'db',
 'password' => 'db',
 'host' => 'mariadb',
 'port' => '',
 'driver' => 'mysql',
 'prefix' => '',
];
```

```sh
../scripts/checkout
../scripts/update
```

## eReolen Go!

```sh
cd ereolengo
../scripts/checkout
../scripts/update
```
