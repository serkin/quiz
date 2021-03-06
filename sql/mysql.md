



- In which cities customers spent more money on `Beauty` in January than in February

<details>
  <summary>Answer</summary>

```sql
    SELECT cities.name,
         t1.m AS j,
         t2.m AS f
    FROM   cities
         LEFT JOIN (SELECT Sum(products.price) AS m,
                         cities.id_city
                  FROM   cities
                         LEFT JOIN users
                                ON users.id_city = cities.id_city
                         LEFT JOIN orders
                                ON users.id_user = orders.id_user
                         LEFT JOIN order_products
                                ON order_products.id_order = orders.id_order
                         LEFT JOIN products
                                ON products.id_product =
                                   order_products.id_product
                         LEFT JOIN categories
                                ON categories.id_category = products.id_category
                  WHERE  categories.name = 'Beauty'
                         AND Monthname (orders.date) = 'January'
                  GROUP  BY cities.id_city) AS t1
              ON cities.id_city = t1.id_city
       LEFT JOIN (SELECT Sum(products.price) AS m,
                         cities.id_city
                  FROM   cities
                         LEFT JOIN users
                                ON users.id_city = cities.id_city
                         LEFT JOIN orders
                                ON users.id_user = orders.id_user
                         LEFT JOIN order_products
                                ON order_products.id_order = orders.id_order
                         LEFT JOIN products
                                ON products.id_product =
                                   order_products.id_product
                         LEFT JOIN categories
                                ON categories.id_category = products.id_category
                  WHERE  categories.name = 'Beauty'
                         AND Monthname (orders.date) = 'February'
                  GROUP  BY cities.id_city) AS t2
              ON cities.id_city = t2.id_city
    WHERE  t2.m > t1.m
```


</details>

<hr>


- Show orders with `Camcorder` but without `Ironing Board`

<details>
  <summary>Answer</summary>

```sql
  SELECT orders.id_order
  FROM   customers
     LEFT JOIN orders
            ON customers.id_customer = orders.id_customer
     LEFT JOIN order_products
            ON order_products.id_order = orders.id_order
     LEFT JOIN products
            ON products.id_product = order_products.id_product
  WHERE  products.name = 'Camcorder'
     AND orders.id_order NOT IN (SELECT orders.id_order
                               FROM   orders
                                      LEFT JOIN order_products
                                             ON order_products.id_order =
                                                orders.id_order
                                      LEFT JOIN products
                                             ON products.id_product =
                                                order_products.id_product
                               WHERE  products.name = 'Ironing Board')
  GROUP  BY orders.id_order
```


</details>

<hr>




- Show customers who spent more than $200 on `Beauty` but who spent less than $100 on `Books`

<details>
  <summary>Answer</summary>

```sql
  SELECT t1.id_customer,
       t1.name
  FROM   (SELECT customers.id_customer,
               customers.name,
               Sum(products.price) AS total
        FROM   customers
               LEFT JOIN orders
                      ON customers.id_customer = orders.id_customer
               LEFT JOIN order_products
                      ON order_products.id_order = orders.id_order
               LEFT JOIN products
                      ON products.id_product = order_products.id_product
               LEFT JOIN categories
                      ON products.id_category = categories.id_category
        WHERE  categories.name = 'Books'
        GROUP  BY orders.id_order
        HAVING total > 4) AS t1
       INNER JOIN (SELECT customers.id_customer,
                          customers.name,
                          Sum(products.price) AS total
                 FROM   customers
                        LEFT JOIN orders
                               ON customers.id_customer = orders.id_customer
                        LEFT JOIN order_products
                               ON order_products.id_order = orders.id_order
                        LEFT JOIN products
                               ON products.id_product =
                                  order_products.id_product
                        LEFT JOIN categories
                               ON products.id_category =
                                  categories.id_category
                 WHERE  categories.name = 'Beauty'
                 GROUP  BY orders.id_order
                 HAVING total < 400) AS t2
  ON t1.id_customer = t2.id_customer 
```


</details>

<hr>


http://superuser.com/questions/288621/create-mysql-database-with-one-line-in-bash

cross join
natural join
outer join


Показать клиентов потративших больше $200 в разделе Books но не потративших больше $100 в разделе Beaty
Показать страну клиенты из которой совершают покупки в разделе Books
Показать товары которые чаще всего покупают клиенты со средним чеком больше $200
Показать клиентов которые заказали eye set но не заказавших Book
Show orders with `Camcorder` but without `Ironing Board`
Показать клиентов с одинаковыми именами не купивших eye set
Показать заказы содержащие eye set но не содержащие Book
Показать страны, в которыех жители тратят столько же на Books  сколько и на Beauty
Показать клиентов которые в марте купили на такую же сумму как и в январе
В каких городах люди больше тратили на Beaty в январе чем в феврале

Показать всех клиентов и товары которые они не купили
