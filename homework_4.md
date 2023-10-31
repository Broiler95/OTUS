# Кластер Couchbase
Сформировал docker-compose.yml
```
version: '3'

services:
  couchbase1:
    image: couchbase/server
    volumes:
      - ./node1:/opt/couchbase/var
  couchbase2:
    image: couchbase/server
    volumes:
      - ./node2:/opt/couchbase/var
  couchbase3:
    image: couchbase/server
    volumes:
      - ./node3:/opt/couchbase/var
  couchbase4:
    image: couchbase/server
    volumes:
      - ./node4:/opt/couchbase/var
    ports:
      - 8091:8091
      - 8092:8092
      - 8093:8093
      - 11210:11210
```

Собрал кластер:
![image](https://github.com/Broiler95/OTUS/assets/114237633/74852417-61d5-4a29-9d59-ec638401b0fc)

# Наполнение тестовыми данными
Создал тестовый бакет:
![image](https://github.com/Broiler95/OTUS/assets/114237633/0e576d39-0857-4ba1-bc25-278e63bc00af)

# Проверка отказоустойчивости
Вывел одну ноду (контейнер) из строя:
![image](https://github.com/Broiler95/OTUS/assets/114237633/6c9e2f22-438c-488e-94fd-433aaa69f37d)

Сделал Rebalance:
![image](https://github.com/Broiler95/OTUS/assets/114237633/b827953e-4cfc-415c-8158-5d5e78b7d5e3)

Проблемная нода покинула кластер.
