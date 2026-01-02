# Stage 4: Cloud Database Clusters

**Status**: TODO
**Prerequisites**: Local testing complete (Stage 3)

---

## Overview

Create managed database clusters in DigitalOcean (syd1 region).

---

## Tasks

### 4.1 Create PostgreSQL Cluster
- [ ] Command: `doctl databases create signals-postgres --engine pg --version 16 --region syd1 --size db-s-1vcpu-2gb --num-nodes 1`
- [ ] Capture cluster ID: `PG_ID=`

### 4.2 Create Kafka Cluster
- [ ] Command: `doctl databases create signals-kafka --engine kafka --version 3.8 --region syd1 --size db-s-2vcpu-4gb --num-nodes 3`
- [ ] Capture cluster ID: `KAFKA_ID=`

### 4.3 Create OpenSearch Cluster
- [ ] Command: `doctl databases create signals-opensearch --engine opensearch --version 2 --region syd1 --size db-s-2vcpu-4gb --num-nodes 1`
- [ ] Capture cluster ID: `OS_ID=`

### 4.4 Create Spaces Bucket
- [ ] Via DO Console: Create `signals-uploads` bucket in syd1

---

## Verification

```bash
doctl databases list --format ID,Name,Engine,Region,Status
```

| Cluster | Engine | Region | Status |
|---------|--------|--------|--------|
| signals-postgres | PostgreSQL | syd1 | [ ] |
| signals-kafka | Kafka | syd1 | [ ] |
| signals-opensearch | OpenSearch | syd1 | [ ] |

---

## Captured IDs

```bash
# Save these for subsequent stages
export PG_ID=
export KAFKA_ID=
export OS_ID=
```

---

## Notes

- Cluster creation takes 5-10 minutes each
- Wait for status "online" before proceeding to Stage 5
- Kafka requires minimum 3 nodes for production use
