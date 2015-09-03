# SQL Homework

For this homework, we will use sqlite3, it is already available on your machine using the command `sqlite3`, copy the file sql_dump.sql in a folder where your sqlite3 database will be, then type:

```
sqlite3 wishlists.db < sql_dump.sql
```

This will create the database wishlists. Unlike Postgres, sqlite3 create a file in the folder where you are, if you delete this file, you delete the db !

To connect to the database, type `sqlite3 wishlists.db`, then you can execute sql statements.

Write SQL statements that:

    1. Selects the names of all products that are not on sale.

sqlite> select name from products where on_sale = 'f';
Teddy Bear
Cat Ears
Card Against Humanity
Set of 12 Mason Jars

    2. Selects the names of all products that cost less than £20.

sqlite> select name from products where price < 20;
Teddy Bear
The Ruby Programming Language
Silicon Valley Monopoly
Set of 12 Mason Jars



    3. Selects the name and price of the most expensive product.

sqlite> SELECT name, MAX(price) FROM products;
Cat Ears|99.99

    4. Selects the name and price of the second most expensive product.

sqlite> select name, MAX(price) from products where price <> (select MAX(price) from products);
Brown Leather Boots|82.0

    5. Selects the name and price of the least expensive product.

sqlite> SELECT name, MIN(price) FROM products;
Set of 12 Mason Jars|6.22

    6. Selects the names and prices of all products, ordered by price in descending order.

sqlite> select name, price from products order by price DESC;
Cat Ears|99.99
Brown Leather Boots|82.0
Lonely Pillow|78.82
Card Against Humanity|25.0
Hoodie|25.0
Set of silverware|22.99
The Ruby Programming Language|19.99
Teddy Bear|17.99
Silicon Valley Monopoly|10.99
Set of 12 Mason Jars|6.22


    7. Selects the average price of all products.

sqlite> select avg(price) from products
   ...> ;
38.899

    8. Selects the sum of the price of all products.
sqlite> SELECT SUM(price) from products;
388.99

    9. Selects the sum of the price of all products whose prices is less than £20.
sqlite> SELECT SUM(price) from products WHERE price < 20;
55.19

    10. Selects the id of the user with your name.
sqlite> SELECT id from users where name = 'Cheryl Wee';
8

    11. Selects the names of all users whose names start with the letter "J".
sqlite> SELECT name from users where name LIKE 'J%';
Jon Rogers
James Gotsell

    12. Selects the number of users whose first names are "Spencer".

sqlite> select count(name) from users where name LIKE '%Spencer%';
1

    13. Selects the number of users who want a "Teddy Bear".

sqlite> select count(id) from wishlists where product_id = 1;
6

    14. Selects the count of items on the wishlish of the user with your name.

sqlite> select count(id) from wishlists where user_id = 8;
5

    15. Selects the count and name of all products on the wishlist, ordered by count in descending order.

sqlite> select name, count(name) from products JOIN wishlists ON wishlists.product_id = products.id GROUP BY name ORDER BY count(name) DESC;

Hoodie|17
Card Against Humanity|16
Cat Ears|15
The Ruby Programming Language|9
Teddy Bear|6
Silicon Valley Monopoly|5
Brown Leather Boots|4
Lonely Pillow|2
Set of 12 Mason Jars|2


    16. Selects the count and name of all products that are not on sale on the wishlist, ordered by count in descending order.

sqlite> select name, count(name) from products JOIN wishlists ON wishlists.product_id = products.id where on_sale = 'f' GROUP BY name ORDER BY count(name) DESC;
Card Against Humanity|16
Cat Ears|15
Teddy Bear|6
Set of 12 Mason Jars|2



    17. Inserts a user with the name "Jonathan Anderson" into the users table. Ensure the created_at column is set to the current time.

sqlite> INSERT INTO users ('created_at', 'name')  VALUES (current_timestamp, 'Jonathan Anderson3');
sqlite> select * from users
   ...> ;
1|2013-04-01 20:09:41.926399|Jon Rogers
2|2013-04-01 20:09:41.932863|James Gotsell
3|2013-04-01 20:09:41.934730|Erica Porter
4|2013-04-01 20:09:41.936167|Christabel Samuels
5|2013-04-01 20:09:41.937655|Dani Zraikat
6|2013-04-01 20:09:41.938977|Rane Gowan
7|2013-04-01 20:09:41.940520|Hassan Mohammadi
8|2013-04-01 20:09:41.942043|Cheryl Wee
9|2013-04-01 20:09:41.943542|Tyrone Compton
10|2013-04-01 20:09:41.945032|Filippo Matoso
11|2013-04-01 20:09:41.946512|Spencer Meyer
12|2013-04-01 20:09:41.947799|Elena Sanna
13|2013-04-01 20:09:41.949360|Gerry Mathe
14|2013-04-01 20:09:41.951026|ALex Chin
15|2014-04-01 20:09:41.926399|Jonathan Anderson
16|2015-09-03 18:56:17|Jonathan Anderson2
17|2015-09-03 19:05:41|Jonathan Anderson3
sqlite> 

    18. Selects the id of the user with the name "Jonathan Anderson"?

sqlite> select id from users where name = "Jonathan Anderson3";
17

    19. Inserts a wishlist entry for the user with the name "Jonathan Anderson" for the product "The Ruby Programming Language".

sqlite> INSERT INTO wishlists ('created_at', 'user_id', 'product_id') VALUES (current_timestamp, (select id from users where name = "Jonathan Anderson3"), (select id from products where name = 'The Ruby Programming Language') );
sqlite> select * from wishlists;
1|2013-04-01 20:09:41.992086|13|1
2|2013-04-01 20:09:41.994331|2|1
3|2013-04-01 20:09:41.996116|8|1
...
78|2015-09-03 19:13:45|17|4
79|2015-09-03 19:13:59|17|4

    20. Updates the name of the "Jonathan Anderson" user to be "Jon Anderson".

UPDATE users SET name = 'Jon Anderson' WHERE id = 17;

sqlite> UPDATE users SET name = 'Jon Anderson' WHERE id = 17;
sqlite> select * from users
   ...> ;
1|2013-04-01 20:09:41.926399|Jon Rogers
2|2013-04-01 20:09:41.932863|James Gotsell
3|2013-04-01 20:09:41.934730|Erica Porter
4|2013-04-01 20:09:41.936167|Christabel Samuels
5|2013-04-01 20:09:41.937655|Dani Zraikat
6|2013-04-01 20:09:41.938977|Rane Gowan
7|2013-04-01 20:09:41.940520|Hassan Mohammadi
8|2013-04-01 20:09:41.942043|Cheryl Wee
9|2013-04-01 20:09:41.943542|Tyrone Compton
10|2013-04-01 20:09:41.945032|Filippo Matoso
11|2013-04-01 20:09:41.946512|Spencer Meyer
12|2013-04-01 20:09:41.947799|Elena Sanna
13|2013-04-01 20:09:41.949360|Gerry Mathe
14|2013-04-01 20:09:41.951026|ALex Chin
15|2014-04-01 20:09:41.926399|Jonathan Anderson
16|2015-09-03 18:56:17|Jonathan Anderson2
17|2015-09-03 19:05:41|Jon Anderson

    21. Deletes the user with the name "Jon Anderson".

sqlite> DELETE from users WHERE name = 'Jon Anderson';
sqlite> select * from users
   ...> ;
1|2013-04-01 20:09:41.926399|Jon Rogers
2|2013-04-01 20:09:41.932863|James Gotsell
3|2013-04-01 20:09:41.934730|Erica Porter
4|2013-04-01 20:09:41.936167|Christabel Samuels
5|2013-04-01 20:09:41.937655|Dani Zraikat
6|2013-04-01 20:09:41.938977|Rane Gowan
7|2013-04-01 20:09:41.940520|Hassan Mohammadi
8|2013-04-01 20:09:41.942043|Cheryl Wee
9|2013-04-01 20:09:41.943542|Tyrone Compton
10|2013-04-01 20:09:41.945032|Filippo Matoso
11|2013-04-01 20:09:41.946512|Spencer Meyer
12|2013-04-01 20:09:41.947799|Elena Sanna
13|2013-04-01 20:09:41.949360|Gerry Mathe
14|2013-04-01 20:09:41.951026|ALex Chin
15|2014-04-01 20:09:41.926399|Jonathan Anderson
16|2015-09-03 18:56:17|Jonathan Anderson2
sqlite> 

    22. Deletes the wishlist item for the user you just deleted.

DELETE from wishlists where user_id= '17';

sqlite> select * from wishlists
   ...> ;
1|2013-04-01 20:09:41.992086|13|1

...

77|2013-04-01 20:09:50.145741|10|9

Please supply SQL statements, and the results they generate.

###Hints
  - As with anything, if you get stuck, move on, then go back if you have time.
  - Don't spend all night on it!
  - Use the resources on teh internetz to solve harder ones - there are solutions to these questions that work with what we've learnt today, but other tools exist in SQL that could make the queries 'better' or 'easier'.
 