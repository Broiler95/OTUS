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
cqlsh> use homework;
cqlsh:homework> CREATE TABLE user(id int, name text, email text, date timestamp, Primary key( id, name, email ) );
cqlsh:homework> CREATE TABLE orders(id int, product text, color text, article int, Primary key( (id, product), article ) );
```
## Наполнили данными
![image](https://github.com/Broiler95/OTUS/assets/114237633/d7cc971d-6c6a-4786-9a66-99455db97fe9)
![image](https://github.com/Broiler95/OTUS/assets/114237633/586fd7c0-937f-477c-bc9b-db1113a73be4)
## Несколько запросов с использованием WHERE
```
cqlsh:homework> delete from orders where id = 1 and product = 'phone' and article = 876487 ;
```
![image](https://github.com/Broiler95/OTUS/assets/114237633/3a242af2-1376-454d-98d6-eb63f9f52513)

```
cqlsh:homework> select * from user where id = 5 and name = 'maria' ;
```
![image](https://github.com/Broiler95/OTUS/assets/114237633/9deabec0-3272-4514-a3c8-30b5766359e5)


