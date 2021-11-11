# eReolen dev

Development environment for eReolen and eReolen GO based on [ddev](https://ddev.com/).

## Installation

```sh
# Install ddev
brew tap drud/ddev && brew install ddev
```

See <https://ddev.readthedocs.io/en/stable/users/performance/> for details on
performance.

```sh
./scripts/install-sites
```

## eReolen

```sh
cd ereolen
../scripts/git/checkout develop develop
ddev start
```

## Get remote data

Edit `.env.local`:

```dotenv
REMOTE_HOST=ereolen.dk
REMOTE_DB_DUMP_CMD='drush --root=/data/www/ereolen_dk/htdocs --uri=ereolen.dk sql-dump --structure-tables-list="cache,cache_*,history,search_*,sessions,watchdog"'
REMOTE_PATH='/data/www/ereolen_dk/htdocs/sites/default/files'
REMOTE_EXCLUDE=(advagg_* css ctools js languages resize styles ting)
LOCAL_PATH='web/sites/default/files'
```

Run

```sh
ddev itkdev:pull:db # to load remote database
ddev itkdev:pull:files # to fetch remote files
```

## Building themes

Almost the same procedure as documented in [Building
themes](https://github.com/ereolen/base#building-themes):

```sh
ddev yarn --cwd sites/all/themes/orwell install
ddev yarn --cwd sites/all/themes/orwell build

ddev yarn --cwd sites/all/themes/wille install
ddev yarn --cwd sites/all/themes/wille build
```
