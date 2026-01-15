# Домашнее задание к занятию «6.4. Docker. Часть 2 — Старцев Данила Антонович»

**Правила выполнения заданий к занятию «6.4. Docker. Часть 2»**

- Все задания выполняйте на основе [конфигов](https://github.com/netology-code/sdvps-homeworks/tree/main/lecture_demos/6-04) из лекции. 
- В заданиях описаны те параметры, которые необходимо изменить. 
- Если параметр не упомянут вообще, значит, его нужно оставить таким, какой он был в лекции. 
- Если в каком-то задании, например, в задании 2, нужно изменить параметр, подразумевается, что во всех следующих заданиях будет использоваться уже изменённый параметр.
- Проверяйте правильность отступов. Очень важно их соблюдать, так как это влияет на структуру данных.
- Выполнив все задания без звёздочки, вы должны получить полнофункциональный сервис.

### Задание 1

**Напишите ответ в свободной форме, не больше одного абзаца текста.**

Установите Docker Compose и опишите, для чего он нужен и как может улучшить лично вашу жизнь.

Решение:
Docker Compose упрощает работу с приложениями из нескольких контейнеров. Вместо кучи сложных команд вы описываете всю систему в одном файле и запускаете её одной строкой.

---

### Задание 2 

**Выполните действия и приложите текст конфига на этом этапе.** 

Создайте файл docker-compose.yml и внесите туда первичные настройки: 

 * version;
 * services;
 * volumes;
 * networks.

При выполнении задания используйте подсеть 10.5.0.0/16.
Ваша подсеть должна называться: <ваши фамилия и инициалы>-my-netology-hw.
Все приложения из последующих заданий должны находиться в этой конфигурации.
```
version: '3'

services:


volumes:


networks:
  StarcevDA-my-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16
          gateway: 10.5.0.1
```
---

### Задание 3 

**Выполните действия:** 

1. Создайте конфигурацию docker-compose для Prometheus с именем контейнера <ваши фамилия и инициалы>-netology-prometheus. 
2. Добавьте необходимые тома с данными и конфигурацией (конфигурация лежит в репозитории в директории [6-04/prometheus](https://github.com/netology-code/sdvps-homeworks/tree/main/lecture_demos/6-04/prometheus) ).
3. Обеспечьте внешний доступ к порту 9090 c докер-сервера.
```
docker-compose.yml конфиг к 3 заданию
version: '3'
services:
  prometheus:
    container_name: StarcevDA-netology-prometheus
    image: prom/prometheus:latest
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
      - ./prometheus/alert_rules.yml:/etc/prometheus/alert_rules.yml
      - prometheus_data:/prometheus

    networks:
      StarcevDA-my-netology-hw:
        ipv4_address: 10.5.0.2

volumes:
  prometheus_data:
    driver: local

networks:
  StarcevDA-my-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16
          gateway: 10.5.0.1
```
![Скриншот-1](https://github.com/MindMaze74/-Docker.-2-.-./blob/main/zd3/img/1.jpeg)
![Скриншот-2](https://github.com/MindMaze74/-Docker.-2-.-./blob/main/zd3/img/2.jpeg)
---

### Задание 4 

**Выполните действия:**

1. Создайте конфигурацию docker-compose для Pushgateway с именем контейнера <ваши фамилия и инициалы>-netology-pushgateway. 
2. Обеспечьте внешний доступ к порту 9091 c докер-сервера.
```
docker-compose.yml конфиг к 4 заданию
version: '3'

services:
  prometheus:
    container_name: StarcevDA-netology-prometheus
    image: prom/prometheus:latest
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
      - ./prometheus/alert_rules.yml:/etc/prometheus/alert_rules.yml
      - prometheus_data:/prometheus

    networks:
      StarcevDA-my-netology-hw:
        ipv4_address: 10.5.0.2
  pushgateway:
    container_name: StarcevDA-netology-pushgateway
    image: prom/pushgateway:latest
    ports:
      - "9091:9091"
    volumes:
      - pushgateway_data:/tmp/pushgateway
    networks:
      StarcevDA-my-netology-hw:
        ipv4_address: 10.5.0.3
  node-exporter:
    container_name: StarcevDA-netology-node-exporter
    image: prom/node-exporter:latest
    ports:
      - "9100:9100"
    networks:
      StarcevDA-my-netology-hw:
        ipv4_address: 10.5.0.4
volumes:
  prometheus_data:
    driver: local
  pushgateway_data:
    driver: local

networks:
  StarcevDA-my-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16
          gateway: 10.5.0.1
```
---
![Скриншот-3](https://github.com/MindMaze74/-Docker.-2-.-./blob/main/zd3/img/3.jpeg)
![Скриншот-4](https://github.com/MindMaze74/-Docker.-2-.-./blob/main/zd3/img/4.jpeg)
![Скриншот-5](https://github.com/MindMaze74/-Docker.-2-.-./blob/main/zd3/img/5.jpeg)
### Задание 5 

**Выполните действия:** 

1. Создайте конфигурацию docker-compose для Grafana с именем контейнера <ваши фамилия и инициалы>-netology-grafana. 
```
docker-compose.yml конфиг к 5 заданию
version: '3'

services:
  prometheus:
    container_name: StarcevDA-netology-prometheus
    image: prom/prometheus:latest
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
      - ./prometheus/alert_rules.yml:/etc/prometheus/alert_rules.yml
      - prometheus_data:/prometheus

    networks:
      StarcevDA-my-netology-hw:
        ipv4_address: 10.5.0.2
  pushgateway:
    container_name: StarcevDA-netology-pushgateway
    image: prom/pushgateway:latest
    ports:
      - "9091:9091"
    volumes:
      - pushgateway_data:/tmp/pushgateway
    networks:
      StarcevDA-my-netology-hw:
        ipv4_address: 10.5.0.3
  node-exporter:
    container_name: StarcevDA-netology-node-exporter
    image: prom/node-exporter:latest
    ports:
      - "9100:9100"
    networks:
      StarcevDA-my-netology-hw:
        ipv4_address: 10.5.0.4
  grafana:
    container_name: StarcevDA-netology-grafana
    image: grafana/grafana:latest
    ports:
      - "80:3000"
    environment:
      - GF_SECURITY_ADMIN_USER=StarcevDA
      - GF_SECURITY_ADMIN_PASSWORD=netology
    volumes:
      - grafana_data:/var/lib/grafana
    
    networks:
      StarcevDA-my-netology-hw:
        ipv4_address: 10.5.0.5
        
volumes:
  prometheus_data:
    driver: local
  pushgateway_data:
    driver: local
  grafana_data:
    driver: local
  grafana_config:
    driver: local

networks:
  StarcevDA-my-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16
          gateway: 10.5.0.1
```
2. Добавьте необходимые тома с данными и конфигурацией (конфигурация лежит в репозитории в директории [6-04/grafana](https://github.com/netology-code/sdvps-homeworks/blob/main/lecture_demos/6-04/grafana/custom.ini).
3. Добавьте переменную окружения с путем до файла с кастомными настройками (должен быть в томе), в самом файле пропишите логин=<ваши фамилия и инициалы> пароль=netology.
```
честно не получилось подгрузить конфиг графаны на авторизацию сделал через 
- GF_SECURITY_ADMIN_USER=StarcevDA
- GF_SECURITY_ADMIN_PASSWORD=netology

конфиг custom.ini
[security]

admin_user = StarcevDa
admin_password = netology
```
4. Обеспечьте внешний доступ к порту 3000 c порта 80 докер-сервера.

![Скриншот-6](https://github.com/MindMaze74/-Docker.-2-.-./blob/main/zd3/img/6.jpeg)
![Скриншот-7](https://github.com/MindMaze74/-Docker.-2-.-./blob/main/zd3/img/7.jpeg)
![Скриншот-8](https://github.com/MindMaze74/-Docker.-2-.-./blob/main/zd3/img/8.jpeg)


---

### Задание 6 

**Выполните действия.**

1. Настройте поочередность запуска контейнеров.
2. Настройте режимы перезапуска для контейнеров.
3. Настройте использование контейнерами одной сети.
5. Запустите сценарий в detached режиме.


Решение:
 работа продемонстрирована выше в заданиях 
---

### Задание 7 

**Выполните действия.**
1. Выполните запрос в Pushgateway для помещения метрики <ваши фамилия и инициалы> со значением 5 в Prometheus: ```echo "<ваши фамилия и инициалы> 5" | curl --data-binary @- http://localhost:9091/metrics/job/netology```.
2. Залогиньтесь в Grafana с помощью логина и пароля из предыдущего задания.
3. Cоздайте Data Source Prometheus (Home -> Connections -> Data sources -> Add data source -> Prometheus -> указать "Prometheus server URL = http://prometheus:9090" -> Save & Test).
4. Создайте график на основе добавленной в пункте 5 метрики (Build a dashboard -> Add visualization -> Prometheus -> Select metric -> Metric explorer -> <ваши фамилия и инициалы -> Apply.

В качестве решения приложите:

* docker-compose.yml **целиком**;
* скриншот команды docker ps после запуске docker-compose.yml;
* скриншот графика, постоенного на основе вашей метрики.
```
version: '3'

services:
  prometheus:
    container_name: StarcevDA-netology-prometheus
    image: prom/prometheus:latest
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
      - ./prometheus/alert_rules.yml:/etc/prometheus/alert_rules.yml
      - prometheus_data:/prometheus

    networks:
      StarcevDA-my-netology-hw:
        ipv4_address: 10.5.0.2
  pushgateway:
    container_name: StarcevDA-netology-pushgateway
    image: prom/pushgateway:latest
    ports:
      - "9091:9091"
    volumes:
      - pushgateway_data:/tmp/pushgateway
    networks:
      StarcevDA-my-netology-hw:
        ipv4_address: 10.5.0.3
  node-exporter:
    container_name: StarcevDA-netology-node-exporter
    image: prom/node-exporter:latest
    ports:
      - "9100:9100"
    networks:
      StarcevDA-my-netology-hw:
        ipv4_address: 10.5.0.4
  grafana:
    container_name: StarcevDA-netology-grafana
    image: grafana/grafana:latest
    ports:
      - "80:3000"
    environment:
      - GF_SECURITY_ADMIN_USER=StarcevDA
      - GF_SECURITY_ADMIN_PASSWORD=netology
    volumes:
      - grafana_data:/var/lib/grafana
    
    networks:
      StarcevDA-my-netology-hw:
        ipv4_address: 10.5.0.5
        
volumes:
  prometheus_data:
    driver: local
  pushgateway_data:
    driver: local
  grafana_data:
    driver: local
  grafana_config:
    driver: local

networks:
  StarcevDA-my-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16
          gateway: 10.5.0.1
```
![Скриншот-9](https://github.com/MindMaze74/-Docker.-2-.-./blob/main/zd3/img/9.jpeg)
![Скриншот-10](https://github.com/MindMaze74/-Docker.-2-.-./blob/main/zd3/img/10.jpeg)
![Скриншот-11](https://github.com/MindMaze74/-Docker.-2-.-./blob/main/zd3/img/11.jpeg)
![Скриншот-12](https://github.com/MindMaze74/-Docker.-2-.-./blob/main/zd3/img/12.jpeg)
![Скриншот-13](https://github.com/MindMaze74/-Docker.-2-.-./blob/main/zd3/img/13.jpeg)
---

### Задание 8

**Выполните действия:** 

1. Остановите и удалите все контейнеры одной командой.

В качестве решения приложите скриншот консоли с проделанными действиями.

---

## Дополнительные задания* (со звёздочкой)

Их выполнение необязательное и не влияет на получение зачёта по домашнему заданию. Можете их решить, если хотите лучше разобраться в материале.

---

### Задание 9* 

**Выполните действия:** 

1. Создайте конфигурацию docker-compose для Alertmanager с именем контейнера <ваши фамилия и инициалы>-netology-alertmanager. 
2. Добавьте необходимые тома с данными и [конфигурацией](https://github.com/netology-code/sdvps-homeworks/tree/main/6-04/alertmanager), сеть, режим и очередность запуска.
3. Обновите конфигурацию Prometheus (необходимые изменения ищите в презентации или документации) и перезапустите его. 
4. Обеспечьте внешний доступ к порту 9093 c докер-сервера.

В качестве решения приложите скриншот с событием из Alertmanager.

---

### Задание 10* 

Запустите свой сценарий на чистом железе без предзагруженных образов.

**Ответьте на вопросы в свободной форме:**

1. Опишите выполненный вами процесс развертывания сценария.
2. Как вы думаете зачем может понадобиться такой способ развертывания?
