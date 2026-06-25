# Домашнее задание к занятию "Docker. Часть 2" - Плотников Вячеслав

### Задание 1

Docker Compose нужен для настройки и запуска многоконтейнерных приложений Docker с помощью одного файла конфигурации, лично мою жизнь он улучшит тем, что сэкономит часы рутины, избавит от захламления компьютера и позволит развернуть личное облако
---

### Задание 2

```
version: '3.8'

networks:
  plotnikov-vi-my-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16

volumes:
  prometheus_data:
  grafana_data:
  alertmanager_data:

services:

```
---

### Задание 3

```
prometheus:
  image: prom/prometheus
  container_name: plotnikov-vi-netology-prometheus

  restart: unless-stopped

  ports:
    - "9090:9090"

  volumes:
    - prometheus_data:/prometheus
    - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml

  networks:
    - plotnikov-vi-my-netology-hw

  depends_on:
      - pushgateway
```
---

### Задание 4

```
pushgateway:
  image: prom/pushgateway
  container_name: plotnikov-vi-netology-pushgateway

  restart: unless-stopped

  ports:
    - "9091:9091"

  networks:
    - plotnikov-vi-my-netology-hw
```
---

### Задание 5

```
grafana:
  image: grafana/grafana
  container_name: plotnikov-vi-netology-grafana

  restart: unless-stopped

  ports:
    - "80:3000"

  environment:
    - GF_PATHS_CONFIG=/etc/grafana/custom.ini

  volumes:
    - grafana_data:/var/lib/grafana
    - ./grafana/custom.ini:/etc/grafana/custom.ini

  networks:
    - plotnikov-vi-my-netology-hw

  depends_on:
      - prometheus
```
---

### Задание 7

```
version: '3.8'

networks:
  plotnikov-vi-my-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16

volumes:
  prometheus_data:
  grafana_data:
  alertmanager_data:

services:

  prometheus:
    image: prom/prometheus:latest
    container_name: plotnikov-vi-netology-prometheus
    restart: unless-stopped
    volumes:
      - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus_data:/prometheus
    ports:
      - "9090:9090"
    networks:
      - plotnikov-vi-my-netology-hw
    depends_on:
      - pushgateway
      - alertmanager

  pushgateway:
    image: prom/pushgateway:latest
    container_name: plotnikov-vi-netology-pushgateway
    restart: unless-stopped
    ports:
      - "9091:9091"
    networks:
      - plotnikov-vi-my-netology-hw

  grafana:
    image: grafana/grafana:latest
    container_name: plotnikov-vi-netology-grafana
    restart: unless-stopped
    environment:
      - GF_PATHS_CONFIG=/etc/grafana/custom.ini
    volumes:
      - ./grafana/custom.ini:/etc/grafana/custom.ini
      - grafana_data:/var/lib/grafana
    ports:
      - "80:3000"  
    networks:
      - plotnikov-vi-my-netology-hw
    depends_on:
      - prometheus

  alertmanager:
    image: prom/alertmanager:latest
    container_name: plotnikov-vi-netology-alertmanager
    restart: unless-stopped
    volumes:
      - ./alertmanager/alertmanager.yml:/etc/alertmanager/alertmanager.yml
      - alertmanager_data:/alertmanager
    ports:
      - "9093:9093"
    networks:
      - plotnikov-vi-my-netology-hw

```
![Скриншот-1](https://github.com/morfaquarius/docker-part-2/blob/main/img/img1.png)
![Скриншот-2](https://github.com/morfaquarius/docker-part-2/blob/main/img/img2.png)

### Задание 8

![Скриншот-3](https://github.com/morfaquarius/docker-part-2/blob/main/img/img3.png)

### Задание 9

![Скриншот-4](https://github.com/morfaquarius/docker-part-2/blob/main/img/img4.png)



