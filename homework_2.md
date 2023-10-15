# Установка MongoDB
MongoDB развернул в Docker с помощью docker-compose.
![image](https://github.com/Broiler95/OTUS/assets/114237633/34e41bca-ff7b-4cc9-a1ee-a34af90b7f2c)

# Выполнение запросов
1. Загрузил датасет в БД test:
![image](https://github.com/Broiler95/OTUS/assets/114237633/2baf2037-1228-4089-820b-991524d37592)
![image](https://github.com/Broiler95/OTUS/assets/114237633/7b927e9c-b7c1-4cf0-8608-3eae7cb74c83)


2. Получил все данные из коллекции: **db.getCollection('sales').find({})**
   ![image](https://github.com/Broiler95/OTUS/assets/114237633/2476e5d8-908c-46da-b679-7c0acb59b5f8)

3. Вывел только строки saleDate:

  ![image](https://github.com/Broiler95/OTUS/assets/114237633/a341fad0-c88b-4a68-8b51-024606ec7274)

4. Вывыел все строки содержашие name:notepad:

   **db.getCollection('sales').find({ 'items.name': 'notepad' }, {'items.$': 1})**
![image](https://github.com/Broiler95/OTUS/assets/114237633/c3d6655a-8ef7-4b62-9221-9effe5880c59)

5. Вывел все возраста старше 40:

   **db.getCollection('sales').find({ 'customer.age': { $gt: 40 } })**
![image](https://github.com/Broiler95/OTUS/assets/114237633/817ddd9f-a1d4-4eb7-b73a-9b70b0d45270)

6. Заменил строки purchaseMethod: Phone на Email:

**db.getCollection('sales').updateOne({ purchaseMethod: "Phone" }, {$set: { purchaseMethod: "Email" } })**
![image](https://github.com/Broiler95/OTUS/assets/114237633/5241c898-1618-418b-9a8f-11335771d30b)

7. Проверил через Robo3T, что данные иззменились
   ![image](https://github.com/Broiler95/OTUS/assets/114237633/6c8930a4-b02a-426e-b061-c06eda79803a)
