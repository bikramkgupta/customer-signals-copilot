# Stage 5: Cloud Config - Users & Topics

**Status**: TODO
**Prerequisites**: Database clusters online (Stage 4)

---

## Overview

Create application-level resources: databases, users, and Kafka topics.

---

## Tasks

### 5.1 PostgreSQL Database & User

```bash
# Get cluster ID (if not saved from Stage 4)
PG_ID=$(doctl databases list --format ID,Name --no-header | grep signals-postgres | awk '{print $1}')

# Create database
doctl databases db create $PG_ID signals_app

# Create user
doctl databases user create $PG_ID signals_user
```

- [ ] Database `signals_app` created
- [ ] User `signals_user` created
- [ ] Password captured: `____________`

### 5.2 Kafka Topics

```bash
# Get cluster ID (if not saved from Stage 4)
KAFKA_ID=$(doctl databases list --format ID,Name --no-header | grep signals-kafka | awk '{print $1}')

# Create topics (3 partitions, replication factor 2)
doctl databases topics create $KAFKA_ID signals.raw.v1 --partitions 3 --replication-factor 2
doctl databases topics create $KAFKA_ID signals.ai.jobs.v1 --partitions 3 --replication-factor 2
doctl databases topics create $KAFKA_ID signals.dlq.v1 --partitions 3 --replication-factor 2
```

- [ ] Topic `signals.raw.v1` created
- [ ] Topic `signals.ai.jobs.v1` created
- [ ] Topic `signals.dlq.v1` created

### 5.3 OpenSearch User

```bash
# Get cluster ID (if not saved from Stage 4)
OS_ID=$(doctl databases list --format ID,Name --no-header | grep signals-opensearch | awk '{print $1}')

# Create user
doctl databases user create $OS_ID signals_user
```

- [ ] User `signals_user` created
- [ ] Password captured: `____________`

### 5.4 Capture Connection Info

```bash
# PostgreSQL
doctl databases connection $PG_ID --format Host,Port,User,Password,Database

# Kafka
doctl databases connection $KAFKA_ID --format Host,Port,User,Password

# OpenSearch
doctl databases connection $OS_ID --format Host,Port,User,Password
```

---

## Connection Strings (capture here)

| Service | Public Host | Port | User | Password |
|---------|-------------|------|------|----------|
| PostgreSQL | signals-postgres-do-user-XXXXX-0.e.db.ondigitalocean.com | 25060 | signals_user | |
| Kafka | signals-kafka-do-user-XXXXX-0.e.db.ondigitalocean.com | 25080 | doadmin | |
| OpenSearch | signals-opensearch-do-user-XXXXX-0.e.db.ondigitalocean.com | 25060 | signals_user | |

---

## Private URL Templates

Fill in with actual values after capturing connection info:

```bash
# PostgreSQL (replace <PASSWORD> and <...> with actual values)
DATABASE_PRIVATE_URL=postgresql://signals_user:<PASSWORD>@private-signals-postgres-do-user-<...>.e.db.ondigitalocean.com:25060/signals_app?sslmode=require

# Kafka (replace <...> with actual value)
KAFKA_PRIVATE_BROKERS=private-signals-kafka-do-user-<...>.e.db.ondigitalocean.com:25080

# OpenSearch (replace <PASSWORD> and <...> with actual values)
OPENSEARCH_PRIVATE_URL=https://signals_user:<PASSWORD>@private-signals-opensearch-do-user-<...>.e.db.ondigitalocean.com:25060
```

---

## Verification

```bash
# List topics
doctl databases topics list $KAFKA_ID

# Verify database
doctl databases db list $PG_ID

# Verify users
doctl databases user list $PG_ID
doctl databases user list $OS_ID
```
