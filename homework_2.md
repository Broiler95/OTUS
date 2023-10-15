# Установка MongoDB
MongoDB развернул в Docker с помощью docker-compose.
![image](https://github.com/Broiler95/OTUS/assets/114237633/34e41bca-ff7b-4cc9-a1ee-a34af90b7f2c)

# Выполнение запросов
1. Загрузил датасет в БД test:
![image](https://github.com/Broiler95/OTUS/assets/114237633/2baf2037-1228-4089-820b-991524d37592)
![image](https://github.com/Broiler95/OTUS/assets/114237633/7b927e9c-b7c1-4cf0-8608-3eae7cb74c83)


2. Получил все данные из коллекции:

**db.getCollection('sales').find({})**
![image](https://github.com/Broiler95/OTUS/assets/114237633/2476e5d8-908c-46da-b679-7c0acb59b5f8)

3. Вывел только строки saleDate:

![image](https://github.com/Broiler95/OTUS/assets/114237633/a341fad0-c88b-4a68-8b51-024606ec7274)

4. Вывыел все строки содержашие name:notepad:

   **db.getCollection('sales').find({ 'items.name': 'notepad' }, {'items.$': 1})**
![image](https://github.com/Broiler95/OTUS/assets/114237633/c3d6655a-8ef7-4b62-9221-9effe5880c59)

6. Вывел все возраста старше 40:

   **db.getCollection('sales').find({ 'customer.age': { $gt: 40 } })**
![image](https://github.com/Broiler95/OTUS/assets/114237633/817ddd9f-a1d4-4eb7-b73a-9b70b0d45270)


