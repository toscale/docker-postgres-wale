# Postgres docker container with wale

Based on https://github.com/docker-library/postgres with [WAL-E](https://github.com/wal-e/wal-e) installed.

Environment variables to pass to the container for WAL-E, all of these must be present or WAL-E is not configured.

```
WALE_WABS_ACCOUNT_NAME`
WALE_WABS_ACCESS_KEY`
WALE_WABS_PREFIX="wabs://<container>/<path>"
WALE_WABS_SAS_TOKEN
```

## Running

The master

```
docker run -it \
  --env "WALE_WABS_ACCOUNT_NAME=****" \
  --env "WALE_WABS_ACCESS_KEY=****" \
  --env "WALE_WABS_SAS_TOKEN=****" \
  --env "WALE_WABS_PREFIX=wabs://<container>/<path>" \
  --env "POSTGRES_AUTHORITY=master" \
  -v ./data/master:/var/lib/postgresql/data \
  toscale/docker-postgres-wale
```

The slave will run in `standby_mode`.

```
docker run -it \
  --env "WALE_WABS_ACCOUNT_NAME=****" \
  --env "WALE_WABS_ACCESS_KEY=****" \
  --env "WALE_WABS_SAS_TOKEN=****" \
  --env "WALE_WABS_PREFIX=wabs://<container>/<path>" \
  --env "POSTGRES_AUTHORITY=slave" \
  -v ./data/slave:/var/lib/postgresql/data \
  toscale/docker-postgres-wale
```

When bringing online `rm ./data/recovery.conf` and start the container with `POSTGRES_AUTHORITY=master`.
