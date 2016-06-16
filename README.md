<center>![Furan](https://s3.amazonaws.com/dsc-misc/furan.jpg)</center>

Furan builds and pushes Docker images from the specified GitHub repository
to the specified target.

**Why use Furan? What advantages are there?**

- Furan is optimized for build speed. It can be configured to run all builds within a RAM disk and streams directly from GitHub to the local Docker daemon without temporary files.

- Furan is deployed as an essentially stateless API application, allowing it to be painlessly scaled out as needed.

- Furan can be triggered on demand or can be hooked into GitHub events.

- Furan supports pushing to Docker registries as well as pre-squashing and deploying
directly to S3.

Dependencies
------------

- Cassandra 2.x (ScyllaDB 1.x)
- Kafka 0.9.x
- Docker 1.6+

API
---

The native API for Furan is based on [gRPC](http://www.grpc.io) and supports
a number of RPC calls. See the [protobuf definition](https://github.com/dollarshaveclub/furan/blob/master/protos/models.proto#L5-L10)
for details.

An [HTTPS adapter](https://github.com/dollarshaveclub/furan/blob/master/HTTP-API.md) is
available for testing convenience.

CLI
---

See the help output for full details:

```bash
$ go build
$ ./furan -h
API application to build Docker images on command

Usage:
  furan [command]

Available Commands:
  build       Build and push a docker image from repo
  server      Run Furan server
  trigger     Start a build on a remote Furan server

Flags:
  -z, --consul-db-svc                 Discover Cassandra nodes through Consul
  -d, --db-dc string                  Comma-delimited list of Cassandra datacenters (if not using Consul discovery) (default "us-west-2")
  -i, --db-init                       Initialize DB keyspace and tables (only necessary on first run)
  -b, --db-keyspace string            Cassandra keyspace (default "furan")
  -n, --db-nodes string               Comma-delimited list of Cassandra nodes (if not using Consul discovery)
  -l, --db-rf-per-dc uint             Cassandra replication factor per DC (if initializing DB) (default 2)
  -g, --github-token-path string      Vault path (appended to prefix) for GitHub token (default "/github/token")
  -f, --kafka-brokers string          Comma-delimited list of Kafka brokers (default "localhost:9092")
  -j, --kafka-max-open-sends uint     Max number of simultaneous in-flight Kafka message sends (default 100)
  -m, --kafka-topic string            Kafka topic to publish build events (required for build monitoring) (default "furan-events")
  -v, --svc-name string               Consul service name for Cassandra (default "cassandra")
  -a, --vault-addr string             Vault URL (default "https://foobar.com")
  -p, --vault-app-id string           Vault App-ID
  -e, --vault-dockercfg-path string   Vault path to .dockercfg contents (default "/dockercfg")
  -x, --vault-prefix string           Vault path prefix for secrets (default "secret/production/furan")
  -t, --vault-token string            Vault token (if using token auth) (default "xxxxx")
  -k, --vault-token-auth              Use Vault token-based auth
  -u, --vault-user-id-path string     Path to file containing Vault User-ID

Use "furan [command] --help" for more information about a command.
```
