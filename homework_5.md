![image](https://github.com/Broiler95/OTUS/assets/114237633/5cbb3c51-9e02-45be-bf2a-89f11af0ed59)## Развернул clickhouse server в docker compose
![image](https://github.com/Broiler95/OTUS/assets/114237633/f031f070-fc04-4170-bbc0-a633277ecaa8)

## Импорт тестовой БД
Был использован данный набор тестовых данных: https://clickhouse.com/docs/ru/getting-started/example-datasets/uk-price-paid

![image](https://github.com/Broiler95/OTUS/assets/114237633/c13882de-f7c9-4ff3-80fb-ca62e909e56b)

## Выполнить запросы и оценить скорость выполнения
```
SELECT toYear(date) AS year, round(avg(price)) AS price, bar(price, 0, 1000000, 80) FROM uk_price_paid GROUP BY year ORDER BY year;
```
![image](https://github.com/Broiler95/OTUS/assets/114237633/24534a0a-166b-4f66-ab8a-e2294d3f47cf)

Скорость выполнения: 1.317 секунд

```
SELECT toYear(date) AS year, round(avg(price)) AS price, bar(price, 0, 2000000, 100) FROM uk_price_paid WHERE town = 'LONDON' GROUP BY year ORDER BY year;
```
![image](https://github.com/Broiler95/OTUS/assets/114237633/4cb86c6a-ab96-48a6-ad70-bc8629600ddc)

Скорость выполнения: 0.036 секунд.

## Создан кластер Clickhouse
Кластер создан из двух шардов, в котором по две реплики.
Фрагмент **config.xml**:
```
...
 <remote_servers>
        <company_cluster>
            <shard>
                <replica>
                    <host>clickhouse01</host>
                    <port>9000</port>
                </replica>
                <replica>
                    <host>clickhouse02</host>
                    <port>9000</port>
                </replica>
            </shard>
            <shard>
                <replica>
                    <host>clickhouse03</host>
                    <port>9000</port>
                </replica>
                <replica>
                    <host>clickhouse04</host>
                    <port>9000</port>
                </replica>
            </shard>
        </company_cluster>
    </remote_servers>
...
```
## Выполнение запросов в кластере: 
```
SELECT toYear(date) AS year, round(avg(price)) AS price, bar(price, 0, 1000000, 80) FROM test.uk_price_paid GROUP BY year ORDER BY year;
```
![image](https://github.com/Broiler95/OTUS/assets/114237633/96c9317a-438b-415a-8ac1-929189191f58)

Скорость выполнения: 1.263 секунд. Быстрее на 0.054 секунды.

```
SELECT toYear(date) AS year, round(avg(price)) AS price, bar(price, 0, 2000000, 100) FROM test.uk_price_paid WHERE town = 'LONDON' GROUP BY year ORDER BY year;
```
![image](https://github.com/Broiler95/OTUS/assets/114237633/91dd7999-ff91-4a84-89de-687bd8ffea4d)

В этом случае скорость выполнения составила чуть больше, чем на однонодовом Clickhouse - 0.142 секунды

![image](https://github.com/Broiler95/OTUS/assets/114237633/0d707340-e8e9-4db4-907c-c74f0ba37096)
Последующие подобные запросы выполнялись за 0.041 секунд.



