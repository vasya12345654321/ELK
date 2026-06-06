# Домашнее задание к занятию «ELK»

**Выполнил:** Михаил Лукьянов

---

# Задание 1. Elasticsearch

Установлен и запущен Elasticsearch 7.17.10 в Docker.

В конфигурации Elasticsearch изменён параметр `cluster.name` на нестандартное значение:

```yaml
cluster.name=my-super-cluster
```

Проверка состояния кластера:

```bash
curl -X GET "localhost:9200/_cluster/health?pretty"
```

Скриншот выполнения команды:

<img width="559" height="206" alt="1" src="https://github.com/user-attachments/assets/0595d246-174b-4cab-846b-addea5a9b84e" />


---

# Задание 2. Kibana

Установлена и запущена Kibana 7.17.10.

В интерфейсе Dev Tools выполнен запрос:

```http
GET /_cluster/health?pretty
```

Получен ответ от Elasticsearch с именем кластера:

```text
my-super-cluster
```

Скриншот интерфейса Kibana:

<img width="960" height="463" alt="2" src="https://github.com/user-attachments/assets/006ceb06-a728-4b62-9445-ab91f97f1a69" />


---

# Задание 3. Logstash

Развернуты контейнеры:

* Elasticsearch
* Kibana
* Nginx
* Logstash

Настроена передача логов Nginx в Elasticsearch через Logstash.

Конфигурация Logstash находится в файле:

```text
logstash.conf
```

Используемый индекс:

```text
nginx-logstash-real
```

Проверка индекса:

```bash
curl http://localhost:9200/_cat/indices?v
```

Пример результата:

```text
yellow open nginx-logstash-real
```

Скриншот Kibana с логами Nginx:

<img width="960" height="505" alt="33" src="https://github.com/user-attachments/assets/a061cfa6-50dd-48e0-a1c2-acadb10c1dfb" />


---

# Задание 4. Filebeat

Установлен и настроен Filebeat 7.17.10.

Настроена передача логов Nginx напрямую в Elasticsearch через Filebeat.

Конфигурация Filebeat находится в файле:

```text
filebeat.yml
```

Проверка созданного индекса:

```bash
curl http://localhost:9200/_cat/indices?v | grep filebeat
```

Результат:

```text
yellow open filebeat-7.17.10-2026.06.05-000001
```

Скриншот Kibana с логами, полученными через Filebeat:

<img width="959" height="503" alt="44" src="https://github.com/user-attachments/assets/003e3f65-24ca-4766-888f-31d35d4fe691" />


---

# Использованные файлы

## docker-compose.yml

Использовался для запуска:

* Elasticsearch
* Kibana
* Nginx
* Logstash
* Filebeat

## logstash.conf

Конфигурация Logstash для передачи логов в Elasticsearch.

## filebeat.yml

### Задание 5*. Доставка данных

В качестве дополнительного сервиса использовался пользовательский лог-файл `app.log`.

Пример записей:

2026-06-06 INFO User login success

2026-06-06 ERROR Database connection failed

2026-06-06 WARN Disk usage 80%

Logstash считывал лог из файла, с помощью фильтра `grok` разбирал его на поля:

* level
* log_date
* log_message

После обработки данные отправлялись в Elasticsearch в индекс `custom-app-log` и отображались в Kibana.

Скриншот:

<img width="960" height="503" alt="5" src="https://github.com/user-attachments/assets/011dbaf6-832d-4efd-9aa6-32a261ae2737" />




Конфигурация Filebeat для сбора Docker-логов и передачи их в Elasticsearch.
