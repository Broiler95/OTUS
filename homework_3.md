# Кластер MongoDB
Сформировал следующий docker-compose файл:
```
version: '3'

services:
  mongo-configsrv-1:
    image: mongo:7
    container_name: mongo-configsrv-1
    hostname: mongo-configsrv-1
    command: ["--configsvr", "--replSet", "config-replica-set", "--bind_ip_all", "--port", "4001"]
    ports:
      - 40001:40001
  
  mongo-configsrv-2:
    image: mongo:7
    container_name: mongo-configsrv-2
    hostname: mongo-configsrv-2
    command: ["--configsvr", "--replSet", "config-replica-set", "--bind_ip_all", "--port", "4002"]
    ports:
      - 40002:40002

  mongo-configsrv-3:
    image: mongo:7
    container_name: mongo-configsrv-3
    hostname: mongo-configsrv-3
    command: ["--configsvr", "--replSet", "config-replica-set", "--bind_ip_all", "--port", "4003"]
    ports:
      - 40003:40003

  mongo-shard-1-rs-1:
    image: mongo:7
    container_name: mongo-shard-1-rs-1
    hostname: mongo-shard-1-rs-1
    command: ["--shardsvr", "--replSet", "shard-replica-set-1", "--bind_ip_all", "--port", "40011"]
    ports:
      - 40011:40011

  mongo-shard-1-rs-2:
    image: mongo:7
    container_name: mongo-shard-1-rs-2
    hostname: mongo-shard-1-rs-2
    command: ["--shardsvr", "--replSet", "shard-replica-set-1", "--bind_ip_all", "--port", "40012"]
    ports:
      - 40012:40012

  mongo-shard-1-rs-3:
    image: mongo:7
    container_name: mongo-shard-1-rs-3
    hostname: mongo-shard-1-rs-3
    command: ["--shardsvr", "--replSet", "shard-replica-set-1", "--bind_ip_all", "--port", "40013"]
    ports:
      - 40013:40013

  mongo-shard-2-rs-1:
    image: mongo:7
    container_name: mongo-shard-2-rs-1
    hostname: mongo-shard-2-rs-1
    command: ["--shardsvr", "--replSet", "shard-replica-set-2", "--bind_ip_all", "--port", "40021"]
    ports:
      - 40021:40021

  mongo-shard-2-rs-2:
    image: mongo:7
    container_name: mongo-shard-2-rs-2
    hostname: mongo-shard-2-rs-2
    command: ["--shardsvr", "--replSet", "shard-replica-set-2", "--bind_ip_all", "--port", "40022"]
    ports:
      - 40022:40022

  mongo-shard-2-rs-3:
    image: mongo:7
    container_name: mongo-shard-2-rs-3
    hostname: mongo-shard-2-rs-3
    command: ["--shardsvr", "--replSet", "shard-replica-set-2", "--bind_ip_all", "--port", "40023"]
    ports:
      - 40023:40023

  mongo-shard-3-rs-1:
    image: mongo:7
    container_name: mongo-shard-3-rs-1
    hostname: mongo-shard-3-rs-1
    command: ["--shardsvr", "--replSet", "shard-replica-set-3", "--bind_ip_all", "--port", "40031"]
    ports:
      - 40031:40031

  mongo-shard-3-rs-2:
    image: mongo:7
    container_name: mongo-shard-3-rs-2
    hostname: mongo-shard-3-rs-2
    command: ["--shardsvr", "--replSet", "shard-replica-set-3", "--bind_ip_all", "--port", "40032"]
    ports:
      - 40032:40032

  mongo-shard-3-rs-3:
    image: mongo:7
    container_name: mongo-shard-3-rs-3
    hostname: mongo-shard-3-rs-3
    command: ["--shardsvr", "--replSet", "shard-replica-set-3", "--bind_ip_all", "--port", "40033"]
    ports:
      - 40033:40033

  mongos-shard:
    image: mongo:7
    container_name: mongos-shard
    hostname: mongos-shard
    command: mongos --configdb config-replica-set/mongo-configsvr-1:40001,mongo-configsvr-2:40002,mongo-configsvr-3:40003 --bind_ip_all
    ports:
      - "27017:27017"
```
``sh.status()``
![image](https://github.com/Broiler95/OTUS/assets/114237633/ada37a38-fade-4b2d-a046-2d9b3d00059f)

``use bank
for (var i=0; i<100; i++) { db.tickets2.insertOne({name: "Max ammout of cost tickets", amount: Math.random()*100}) }``
![image](https://github.com/Broiler95/OTUS/assets/114237633/049b8163-bcee-46d9-8f6e-6f9dd5928d3f)

Ключ шардирования
```use bank
sh.enableSharding("bank")

use config
 db.settings.updateOne(
   { _id: "chunksize" },
   { $set: { _id: "chunksize", value: 1 } },
   { upsert: true }
)
```

``use bank
for (var i=0; i<10000; i++) { db.tickets.insertOne({name: "Max ammout of cost tickets", amount: Math.random()*100}) }
sh.status()``
![image](https://github.com/Broiler95/OTUS/assets/114237633/e6d3223b-428c-446f-89ae-282808abf6a9)

## Роняем инстансы
Зашел на secondary ноду:
![image](https://github.com/Broiler95/OTUS/assets/114237633/65ae95af-eb90-4e15-bd34-8089efa27c71)

Уронил primary ноду:

![image](https://github.com/Broiler95/OTUS/assets/114237633/04dac5d1-29b4-44f9-8308-55a7df2614d2)

После чего secondary нода стала primary:
![image](https://github.com/Broiler95/OTUS/assets/114237633/c8624413-5dc5-407c-bcc3-9ebfdac52930)

## Создаем пользовтелей с ролями
Создал юзера **sasha** и присвоили ему роль чтение и запись:
![image](https://github.com/Broiler95/OTUS/assets/114237633/9632e22b-2359-4937-9011-75d1cdf6ba6f)

Создал юзера **ivan** и присвоил ему роль администратора любых баз:
![image](https://github.com/Broiler95/OTUS/assets/114237633/5702d2aa-e907-453e-8e6b-8e189836a52c)

Проверим вход под **ivan**:
![image](https://github.com/Broiler95/OTUS/assets/114237633/855a78ec-fa55-4b56-9087-9d96e63cd6bd)





