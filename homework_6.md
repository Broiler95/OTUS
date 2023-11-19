## Развернуть docker и 3 узловый кластер Cassandra
docker-compose.yml:
```
version: '3'

services:
  cassandra1:
    image: cassandra:latest
    container_name: cassandra1
    ports:
      - "9042:9042"
    environment:
      - CASSANDRA_SEEDS=cassandra1,cassandra2,cassandra3

  cassandra2:
    image: cassandra:latest
    container_name: cassandra2
    environment:
      - CASSANDRA_SEEDS=cassandra1,cassandra2,cassandra3

  cassandra3:
    image: cassandra:latest
    container_name: cassandra3
    environment:
      - CASSANDRA_SEEDS=cassandra1,cassandra2,cassandra3
```
![image](https://github.com/Broiler95/OTUS/assets/114237633/cff4a802-7761-47e4-9fca-945d1a6315be)

## Создаём KEYSPACE с 2-мя таблицами
```
cqlsh> CREATE KEYSPACE homework WITH replication = {'class': 'SimpleStrategy', 'replication_factor' : '2' };
cqlsh> CREATE TABLE user(id int, name text, email text, date timestamp, Primary key( id, name, email ) );
cqlsh> CREATE TABLE orders(id int, product text, color text, article int, Primary key( (id, product), article ) );
```
