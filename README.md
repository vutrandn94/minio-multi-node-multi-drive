# Deploy MinIO: Multi-Node Multi-Drive

## Require
- Format disk XFS for high performance
- Minimum: 4 server
- Software: Docker, Docker Compose

## Server info
**Setting hosts file**
```
54.255.132.217 minio1
18.143.143.23 minio2
52.77.236.94 minio3
13.228.30.246 minio4
```

## Deploy
**Prepare**
```
# groupadd -g 1001 minio

# useradd -m -u 1001 -g 1001 -s /usr/sbin/nologin minio

# mkdir -p /mnt/data-{0..1}

# chown minio:minio /tmp/data-{0..1}

```

**docker-compose.yml in minio1**
```
version: '2'

services:
  minio1:
    image: 'bitnami/minio:latest'
    restart: always
    environment:
      - MINIO_ROOT_USER=root
      - MINIO_ROOT_PASSWORD=xxxxx
      - MINIO_DISTRIBUTED_MODE_ENABLED=yes
      - MINIO_DISTRIBUTED_NODES=minio{1...4}/bitnami/minio/data-{0...1}
    ports:
      - 9000:9000
      - 9001:9001
    volumes:
      - /mnt/data-0:/bitnami/minio/data-0
      - /mnt/data-1:/bitnami/minio/data-1
```

**docker-compose.yml in minio2**
```
version: '2'

services:
  minio2:
    image: 'bitnami/minio:latest'
    restart: always
    environment:
      - MINIO_ROOT_USER=root
      - MINIO_ROOT_PASSWORD=xxxxx
      - MINIO_DISTRIBUTED_MODE_ENABLED=yes
      - MINIO_DISTRIBUTED_NODES=minio{1...4}/bitnami/minio/data-{0...1}
    ports:
      - 9000:9000
      - 9001:9001
    volumes:
      - /mnt/data-0:/bitnami/minio/data-0
      - /mnt/data-1:/bitnami/minio/data-1
```

**docker-compose.yml in minio3**
```
version: '2'

services:
  minio3:
    image: 'bitnami/minio:latest'
    restart: always
    environment:
      - MINIO_ROOT_USER=root
      - MINIO_ROOT_PASSWORD=xxxxx
      - MINIO_DISTRIBUTED_MODE_ENABLED=yes
      - MINIO_DISTRIBUTED_NODES=minio{1...4}/bitnami/minio/data-{0...1}
    ports:
      - 9000:9000
      - 9001:9001
    volumes:
      - /mnt/data-0:/bitnami/minio/data-0
      - /mnt/data-1:/bitnami/minio/data-1
```

**docker-compose.yml in minio4**
```
version: '2'

services:
  minio4:
    image: 'bitnami/minio:latest'
    restart: always
    environment:
      - MINIO_ROOT_USER=root
      - MINIO_ROOT_PASSWORD=xxxxx
      - MINIO_DISTRIBUTED_MODE_ENABLED=yes
      - MINIO_DISTRIBUTED_NODES=minio{1...4}/bitnami/minio/data-{0...1}
    ports:
      - 9000:9000
      - 9001:9001
    volumes:
      - /mnt/data-0:/bitnami/minio/data-0
      - /mnt/data-1:/bitnami/minio/data-1
```

**Deploy service**
```
# docker-compose up -d
```

- Access MinioUI: [http://<MINIO_SERVER_IP>:9001] or config Nginx / HAProxy to loadbalance for MinIO nodes