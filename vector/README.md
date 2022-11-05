# Installation Vector

## RPM-based

1. Add repo
```bash
curl -1sLf 'https://repositories.timber.io/public/vector/cfg/setup/bash.rpm.sh' \
  | sudo -E bash
```

2. Install
```bash
sudo yum install vector
```

## Debain-based

1. Add repo
```bash
curl -1sLf \
  'https://repositories.timber.io/public/vector/cfg/setup/bash.deb.sh' \
| sudo -E bash
```

2. Install
```bash
sudo apt install vector
```

# Configuration

## Docker/Docker compose

For you need to bind the following volumes

- ```/etc/vector/```
- ```/logs```

Variables for clickhouse

- ```CLICK_HOUSE_URL=<clickhouser-address>```
- ```CLICK_HOUSE_DATABASE=<db-name>```
- ```CLICK_HOUSE_RAW_TABLE=<db-table>```
- ```GW_LOG_FILE=<log-path>```


## Docker compose

For docker compose use the following ```docker-complse.yaml```

```yaml
version: "3"
services:
  clickhouseserver:
   image: yandex/clickhouse-server:latest
   hostname: clickhouse-server
   container_name: clickhouse
   networks:
     - clicknet
   ports:
     - "8123:8123"
     - "9009:9009"
   volumes:
     - ./clickhouse/config:/etc/clickhouse-server
     - ./clickhouse/data:/var/lib/clickhouse

  vector:
    image: timberio/vector:0.24.2-debian
    hostname: vector
    container_name: vector
    networks:
      - clicknet
    ports:
      - "8383:8383"
      - "8686:8686"
      - "9000:9000"
    volumes:
      - ./vector/config:/etc/vector
      - ./vector/logs:/logs
    environment:
      - CLICK_HOUSE_URL=http://clickhouseserver:8123
      - CLICK_HOUSE_DATABASE=sampv3
      - CLICK_HOUSE_RAW_TABLE=rawlogdata
      - GW_LOG_FILE=/logs/my_log.log
        #- VECTOR_LOG=debug

networks:
  clicknet:
    driver: bridge
```

Related:
```
* https://vector.dev/docs/setup/installation/
```


