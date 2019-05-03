# Detailed Design

## PKI initialization

### Requirements for core services

#### Consul

Required configuration keys (put in a `.json`file in the `config-dir`):

- addresses:https
- ports: https
- key_file
- cert_file
- ca_file

Reference: https://www.consul.io/docs/agent/options.html#configuration_files

#### Cassandra

Configure in `cassandra.yaml`:

```yaml
client_encryption_options:
  enabled: true
  optional: false
  keystore: /path/to/keystore.jks
  keystore_password: changeit
  protocol: TLS
  algorithm: SunX509
  store_type: JKS
  cipher_suites: [TLS_RSA_WITH_AES_256_CBC_SHA]
```

#### Postgres

Postgres configuration options (postgresql.conf).

- ssl = on
- ssl_cert_file (default: $PGDATA/server.crt)
- ssl_key_file (default: $PGDATA/server.key)
- ssl_ca_file (default: $PGDATA/root.crt)

These parameters can be set with the -c command line switch (i.e. -c ssl=on -c ssl_cert_file=blah).

For Docker, PGDATA defaults to /var/lib/postgresql/data.

#### Kong

Required configuration keys:

- admin_ssl = true
- admin_ssl_cert
- admin_ssl_key
- pg_ssl = true
- pg_ssl_verify = true
- cassandra_ssl = true
- cassandra_ssl_verify = true

These keys are set by specifying environment variables with the prefix `KONG`.  For example, `$KONG_ADMIN_SSL` sets the admin_ssl parameter.

Reference: https://docs.konghq.com/1.0.x/configuration/#admin_ssl

#### Vault

Configuration is specified by -config argument to specify name of a directory.  Dockerized vault defaults to /vault/config.  Can drop a .json file in here and it will be processed.  For Docker, the environment variable `VAULT_LOCAL_CONFIG` is copied to `local.json` in this folder without having to mount a volume.

- tls_cert_file
- tls_key_file
- tls_perfer_server_cipher_suites = true

Reference: https://www.vaultproject.io/docs/configuration/listener/tcp.html

### Per-service drop locations

PKI collateral must be placed in designated locations for use by bootstrap components.  These locations vary depending on on the runtime used.

#### Docker PKI drop locations

pki-init service must write the PKI to the following locations (note: dockerized version currently uses Kong-Cassandra):

| Target service | Drop Location                          | Description                           |
| -------------- | -------------------------------------- | ------------------------------------- |
| consul         | /consul/config/tls.json                | Set TLS configuration keys            |
| consul         | /run/edgex/secrets/consul/server.key   | TLS private key                       |
| consul         | /run/edgex/secrets/consul/server.crt   | Server cert & intermediate cert       |
| consul         | /run/edgex/secrets/consul/ca.pem       | CA certificate                        |
| kong           | /run/edgex/secrets/kong/server.key     | Kong admin private key                |
| kong           | /run/edgex/secrets/kong/server.crt     | Kong admin server & intermediate cert |
| postgres       | /run/edgex/secrets/postgres/server.key | TLS private key                       |
| postgres       | /run/edgex/secrets/postgres/server.crt | Server cert & intermediate cert       |
| postgres       | /run/edgex/secrets/postgres/ca.pem     | CA certificate                        |
| vault          | /vault/config/tls.json                 | Set TLS configuration keys            |
| vault          | /run/edgex/secrets/vault/server.key    | Vault private key                     |
| vault          | /run/edgex/secrets/vault/server.crt    | Vault server & intermediate cert      |

Container mounts:

- pki-init
  - pki-init-config @ /usr/local/etc/edgex/pki (PKI_CACHE)
  - consul-config @ /consul/config
  - vault-config @ /vault/config
  - /run/edgex/secrets (bind to host)
- consul
  - consul-config @ /consul/config
  - /run/edgex/secrets/consul (bind to host)
- kong
  - /run/edgex/secrets/kong (bind to host)
- postgres
  - /run/edgex/secrets/postgres (bind to host)
- vault
  - vault-config @ /vault/config
  - /run/edgex/secrets/vault (bind to host)



#### Snap PKI drop locations

PKI setup logic should write the following locations:

| Target service | Drop Location                                         | Description                           |
| -------------- | ----------------------------------------------------- | ------------------------------------- |
| consul         | $SNAP_DATA/consul/config/tls.json                     | Set TLS configuration keys            |
| consul         | $XDG_RUNTIME_DIR/edgex/secrets/consul/server.key      | Consul private key                    |
| consul         | $XDG_RUNTIME_DIR/edgex/secrets/consul/server.crt      | Server cert & intermediate cert       |
| consul         | $XDG_RUNTIME_DIR/edgex/secrets/consul/ca.pem          | CA certificate                        |
| kong           | $XDG_RUNTIME_DIR/edgex/secrets/kong/server.key        | Kong admin private key                |
| kong           | $XDG_RUNTIME_DIR/edgex/secrets/kong/server.pem        | Kong admin server & intermediate cert |
| cassandra      | $XDG_RUNTIME_DIR/edgex/secrets/cassandra/server.key   | Cassndra private key                  |
| cassandra      | $XDG_RUNTIME_DIR/edgex/secrets/cassandra/server.crt   | Cassandra leaf certificate            |
| cassandra      | $XDG_RUNTIME_DIR/edgex/secrets/cassandra/chain.crt    | Cassandra cert chain                  |
| cassandra      | $XDG_RUNTIME_DIR/edgex/secrets/cassandra/keystore.jks | Java keystore contains above          |
| vault          | $SNAP_DATA/vault/config/tls.json                      | Set TLS configuration keys            |
| vault          | $XDG_RUNTIME_DIR/edgex/secrets/consul/server.key      | Vault private key                     |
| vault          | $XDG_RUNTIME_DIR/edgex/secrets/consul/server.crt      | Vault server & intermediate cert      |

The Cassandra keystore.jks must be post-processed by shell script that runs Java keytool in order to put PKI-generated assets into an acceptable format.

`loginctl enable-linger (username)` is needed to ensure the `$XDG_RUNTIME_DIR` is created, where `(username)` is `root` when EdgeX is launched with `sudo snap start edgexfoundry`

### Modes of operation

PKI setup always ends with deploying the PKI to the affected microservices.  There are three different ways to get a PKI that can be deployed:

1. Importing an externally defined PKI hierarchy and deploying it to microservices at startup.
2. Generating a fresh one from scratch and deploying it on every boot.
3. Doing a one-time PKI generation and caching the leaf (and optional CA) certificates for future deployment.

PKI_CACHE is specified by environment variable but defaults to /etc/edgex/pki.

#### --import

There is no active code that performs this function; importing a PKI is done out-of-band.

Docker: Importing PKI is done by customizing edgex-docker with pre-configured PKI, which will be imported to pki-init-config volume automatically.
Snap: TBD

#### --generate

Use pkisetup utility to generate certificate hierarchy.  This should be done to $XDG_RUNTIME_DIR and should post-arrange the files to conform to deployment strategy.  If using Cassandra, make a JKS at this time.

#### --cache

Copies the PKI into a persistent location ($PKI_CACHE)

- Docker: pki-init-config volume
- Snap: $SNAP_DATA/pki-init

#### --cacheca

Copies the PKI CA private keys into a persistent location ($PKI_CACHE/ca).  Normally CA private keys should be destroyed after PKI is generated.

- Docker: pki-init-config volume
- Snap: $SNAP_DATA/pki-init

#### --deploy (Docker)

Recursively copy $PKI_CACHE/config to /
Recursively copy $PKI_CACHE/secrets to /run/edgex/secrets/

#### --deploy (Snap)

Recursively copy $PKI_CACHE/config to $SNAP_DATA
Recursively copy $PKI_CACHE/secrets to $XDG_RUNTIME_DIR