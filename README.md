# Домашнее задание к занятию 15 «Система сбора логов Elastic Stack»  Мельник Юрий Алексндрович

## Подготовка 
На хосте (Linux):
```
sudo sysctl -w vm.max_map_count=262144
```


Из директории help 
```
docker-compose up -d
```

После старта (даём контейнерам 1–2 минуты на прогрев):

![рисунок 1](https://github.com/ysatii/monitoring-hw4/blob/main/img/img_1.jpg)
![рисунок 2](https://github.com/ysatii/monitoring-hw4/blob/main/img/img_2.jpg)


## Задание 1

Вам необходимо поднять в докере и связать между собой:

- elasticsearch (hot и warm ноды);
- logstash;
- kibana;
- filebeat.

Logstash следует сконфигурировать для приёма по tcp json-сообщений.

Filebeat следует сконфигурировать для отправки логов docker вашей системы в logstash.

В директории [help](./help) находится манифест docker-compose и конфигурации filebeat/logstash для быстрого 
выполнения этого задания.

Результатом выполнения задания должны быть:

- скриншот `docker ps` через 5 минут после старта всех контейнеров (их должно быть 5);
- скриншот интерфейса kibana;
- docker-compose манифест (если вы не использовали директорию help);
- ваши yml-конфигурации для стека (если вы не использовали директорию help).


## Решение  1

![рисунок 3](https://github.com/ysatii/monitoring-hw4/blob/main/img/img_3.jpg)

![рисунок 7](https://github.com/ysatii/monitoring-hw4/blob/main/img/img_7.jpg)

## Задание 2

Перейдите в меню [создания index-patterns  в kibana](http://localhost:5601/app/management/kibana/indexPatterns/create) и создайте несколько index-patterns из имеющихся.

Перейдите в меню просмотра логов в kibana (Discover) и самостоятельно изучите, как отображаются логи и как производить поиск по логам.

В манифесте директории help также приведенно dummy-приложение, которое генерирует рандомные события в stdout-контейнера.
Эти логи должны порождать индекс logstash-* в elasticsearch. Если этого индекса нет — воспользуйтесь советами и источниками из раздела «Дополнительные ссылки» этого задания.
 
---
## Решение  2
![рисунок 4](https://github.com/ysatii/monitoring-hw4/blob/main/img/img_4.jpg)
![рисунок 5](https://github.com/ysatii/monitoring-hw4/blob/main/img/img_5.jpg)
![рисунок 6](https://github.com/ysatii/monitoring-hw4/blob/main/img/img_6.jpg)
![рисунок 7](https://github.com/ysatii/monitoring-hw4/blob/main/img/img_7.jpg)
![рисунок 8](https://github.com/ysatii/monitoring-hw4/blob/main/img/img_8.jpg)
![рисунок 9](https://github.com/ysatii/monitoring-hw4/blob/main/img/img_9.jpg)
![рисунок 10](https://github.com/ysatii/monitoring-hw4/blob/main/img/img_10.jpg)
![рисунок 11](https://github.com/ysatii/monitoring-hw4/blob/main/img/img_11.jpg)

## Список контейнеров

| Имя службы | Порты (host:container) | Назначение |
|------------|------------------------|------------|
| **es-hot** | 9200 → 9200, 9300 → 9300 | Основная нода Elasticsearch (hot), хранит и обрабатывает «горячие» данные, принимает запросы REST. |
| **es-warm** | (нет проброса наружу, только во внутренней сети) | Вторая нода Elasticsearch (warm), хранит «тёплые» данные, участвует в кластере. |
| **kibana** | 5601 → 5601 | Веб-интерфейс для работы с Elasticsearch: настройка Data Views, визуализации, Discover, Dashboard. |
| **logstash** | 5044 → 5044, 5046 → 5046, 9600 → 9600 | Принимает логи от Filebeat (Beats input), обрабатывает фильтрами (json/grok), пишет в Elasticsearch. |
| **filebeat** | (порты не проброшены) | Агент, который читает Docker-логи контейнеров на хосте и отправляет их в Logstash. |
| **some_app** | (порты не проброшены) | Тестовое приложение-«логогенератор» (Python), каждую секунду пишет INFO/WARN/ERROR в stdout, чтобы были данные для Filebeat. |

## Дополнительные ссылки

При выполнении задания используйте дополнительные ресурсы:

- [поднимаем elk в docker](https://www.elastic.co/guide/en/elastic-stack-get-started/current/get-started-docker.html);
- [поднимаем elk в docker с filebeat и docker-логами](https://www.sarulabs.com/post/5/2019-08-12/sending-docker-logs-to-elasticsearch-and-kibana-with-filebeat.html);
- [конфигурируем logstash](https://www.elastic.co/guide/en/logstash/current/configuration.html);
- [плагины filter для logstash](https://www.elastic.co/guide/en/logstash/current/filter-plugins.html);
- [конфигурируем filebeat](https://www.elastic.co/guide/en/beats/libbeat/5.3/config-file-format.html);
- [привязываем индексы из elastic в kibana](https://www.elastic.co/guide/en/kibana/current/index-patterns.html);
- [как просматривать логи в kibana](https://www.elastic.co/guide/en/kibana/current/discover.html);
- [решение ошибки increase vm.max_map_count elasticsearch](https://stackoverflow.com/questions/42889241/how-to-increase-vm-max-map-count).

В процессе выполнения в зависимости от системы могут также возникнуть не указанные здесь проблемы.

Используйте output stdout filebeat/kibana и api elasticsearch для изучения корня проблемы и её устранения.

### Как оформить решение задания

Выполненное домашнее задание пришлите в виде ссылки на .md-файл в вашем репозитории.

---

 
