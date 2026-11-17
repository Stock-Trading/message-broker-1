# message-broker-1

Local message broker environment for the Stock-Trading project. `docker-compose.yml` runs a single-node Apache Kafka broker (KRaft mode, no ZooKeeper) together with a web UI.

## Services

| Service    | Image                         | Host port | Purpose                                  |
|------------|-------------------------------|-----------|------------------------------------------|
| `kafka`    | `apache/kafka:3.9.1`          | 9092      | Kafka broker + controller (single node)  |
| `kafka-ui` | `ghcr.io/kafbat/kafka-ui:latest` | 9080   | Web UI for browsing topics and messages  |

## Prerequisites

- Docker with Docker Compose v2 (`docker compose`)

## Usage

From the directory containing `docker-compose.yml`:

```powershell
docker compose up -d      # start
docker compose ps         # check status
docker compose logs -f kafka
docker compose down       # stop (add -v to remove volumes)
```

## Connecting

- **From the host machine** (apps running locally): `localhost:9092`
- **From other containers** on the same compose network: `kafka:29092`
- **Kafka UI**: <http://localhost:9080> (cluster name: `local`)

## Notes

- Intended for local development only: listeners use `PLAINTEXT` (no auth/TLS).
- Replication factors and min ISR are set to 1 because there is only one broker.
- Data is not persisted to a volume; topics are lost when the container is removed.
- `kafka-ui` uses the `latest` tag; pin a version for reproducible setups.
