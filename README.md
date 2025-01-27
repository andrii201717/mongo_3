Найдите средний возраст из коллекции ich.US_Adult_Income
[
  {
    $group:
      /**
       * _id: The id of the group.
       * fieldN: The first field name.
       */
      {
        _id: 0,
        avg_age: {
          $avg: "$age"
        }
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        avg_age: {
          $round: ["$avg_age", 2]
        }
      }
  }
]

Поменяв подключение к базе данных, создать коллекцию orders_NAME (для уникальности - добавим ваше имя в название) 
со свойствами id, customer, product, amount, city, используя следующие данные:

id customer product amount city
1 Olga Apple 15.55 Berlin
2 Anna Apple 10.05 Madrid
3 Olga Kiwi 9.6 Berlin
4 Anton Apple 20 Roma
5 Olga Banana 8 Madrid
6 Petr Orange 18.3 Paris

Найти сколько всего было совершено покупок

db["orders_Andrii_Tabaka"].find().count()

Найти сколько всего раз были куплены яблоки

db["orders_Andrii_Tabaka"].find({product: "Apple"}).count()

Вывести идентификаторы трех самые дорогих покупок

db["orders_Andrii_Tabaka"].find({}, {_id: 0, id: 1, amount: 1}).sort({amount: -1}).limit(3)

Найти сколько всего покупок было совершено в Берлине

db["orders_Andrii_Tabaka"].find({city: "Berlin"}).count()

Найти количество покупок яблок в городах Берлин и Мадрид

db["orders_Andrii_Tabaka"].find({city: {$in: ["Berlin", "Madrid"]}, product: "Apple"}).count()

Найти сколько было потрачено каждым покупателем

db["orders_Andrii_Tabaka"].aggregate([{$group: {_id: "$customer" , total: {$sum : '$amount'}}}])

Найти в каких городах совершала покупки Ольга

[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        customer: "Olga"
      }
  },
  {
    $group:
      /**
       * _id: The id of the group.
       * fieldN: The first field name.
       */
      {
        _id: "$city"
      }
  }
]
