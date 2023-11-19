## Подводные камни восстановления данных
Я попробовал восстановить данные из снапшота. 
Для меня неудобство и сложность заключается в том, что для восстановления, например, из **sstableloader** недостаточно указать место, где лежит снапшот, нужно произвести множество действий:
```
$ sudo rm /catalogkeyspace/magazine/*
$ cd /catalogkeyspace/magazine/
$ sudo cp /etc/cassandra/data/catalogkeyspace/magazine-446eae30c22a11e9b1350d927649052c/snapshots/magazine/* /catalogkeyspace/magazine
$ mkdir <keyspace_name>
$ ln -s <path_to_snapshot_folder> <keyspace_name>/<table_name>
$ sstableloader --nodes cassandra1  /catalogkeyspace/magazine/
```
И только после этого удалось получить данные в keyspace.

Также для восстановления из снапшота необходимо, чтобы уже существовал нужный keyspace и table.
