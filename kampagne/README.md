# eReolen-kampagne

## Installation

```sh
cat > web/sites/default/settings.local.php <<'EOF'
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

// And memcached:
$conf['memcache_servers'] = array(
  (getenv('MEMCACHED_HOST') ?: 'memcached').':'.(getenv('MEMCACHED_PORT') ?: 11211) => 'default',
);
EOF
```

```sh
docker-compose up -d
```

```sh
itkdev-docker-compose sync:db
```

```sh
./scripts/install
```

## Updating

```sh
./scripts/update
```

```sh
echo http://$(docker-compose port nginx 80)
docker-compose run --rm drush --root=/app/web --uri=http://$(docker-compose port nginx 80) user-login
```
