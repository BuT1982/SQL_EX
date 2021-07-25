Схема БД состоит из четырех таблиц: ![БД](https://sql-ex.ru/images/computers.gif)
Таблица Product представляет производителя (maker), номер модели (model) и тип ('PC' - ПК, 'Laptop' - ПК-блокнот или 'Printer' - принтер). Предполагается, что номера моделей в таблице Product уникальны для всех производителей и типов продуктов. В таблице PC для каждого ПК, однозначно определяемого уникальным кодом – code, указаны модель – model (внешний ключ к таблице Product), скорость - speed (процессора в мегагерцах), объем памяти - ram (в мегабайтах), размер диска - hd (в гигабайтах), скорость считывающего устройства - cd (например, '4x') и цена - price. Таблица Laptop аналогична таблице РС за исключением того, что вместо скорости CD содержит размер экрана -screen (в дюймах). В таблице Printer для каждой модели принтера указывается, является ли он цветным - color ('y', если цветной), тип принтера - type (лазерный – 'Laser', струйный – 'Jet' или матричный – 'Matrix') и цена - price.


**Exercise: 1.** Найдите номер модели, скорость и размер жесткого диска для всех ПК стоимостью менее 500 дол. 
Вывести: model, speed и hd
```sql
SELECT model, speed, hd FROM PC
WHERE price < 500
```

**Exercise: 2.** Найдите производителей принтеров.
Вывести: maker
```sql
SELECT DISTINCT maker FROM Product
WHERE type = 'Printer'
```

**Exercise: 3.** Найдите номер модели, объем памяти и размеры экранов ПК-блокнотов, цена которых превышает 1000 дол.
```sql
SELECT model, ram, screen FROM laptop
WHERE price > 1000
```

**Exercise: 4.** Найдите все записи таблицы Printer для цветных принтеров.
```sql
SELECT * FROM printer
WHERE color = 'y'
```

**Exercise: 5.** Найдите номер модели, скорость и размер жесткого диска ПК, имеющих 12x или 24x CD и цену менее 600 дол.
```sql
SELECT model, speed, hd FROM pc
WHERE (cd='12x' or cd = '24x') AND price < 600
```

**Exercise: 6.** Для каждого производителя, выпускающего ПК-блокноты c объёмом жесткого диска не менее 10 Гбайт, найти скорости таких ПК-блокнотов. 
Вывод: производитель, скорость.
```sql
SELECT Distinct maker, speed 
FROM 
Product INNER JOIN Laptop
ON Laptop.model = Product.model
WHERE hd>=10
```

**Exercise: 7.** Найдите номера моделей и цены всех имеющихся в продаже продуктов (любого типа) производителя B
```sql
SELECT Product.model, price from Product
JOIN PC ON Product.model = PC.model
WHERE maker = 'B'
UNION
SELECT Product.model, price from Product
JOIN Laptop ON Product.model = Laptop.model
WHERE maker = 'B'
UNION
SELECT Product.model, price from Product
JOIN Printer ON Product.model = Printer.model
WHERE maker = 'B'
```

**Exercise: 8.** Найдите производителя, выпускающего ПК, но не ПК-блокноты.
```sql
SELECT DISTINCT maker
FROM Product 
WHERE type = 'PC' AND
maker NOT IN (SELECT DISTINCT maker
FROM Product 
WHERE type = 'Laptop')
```

**Exercise: 9.** Найдите производителей ПК с процессором не менее 450 Мгц. 
Вывести: Maker
```sql
SELECT DISTINCT maker FROM Product
JOIN PC ON Product.model = PC.model
WHERE speed>=450
```

**Exercise: 10.** Найдите модели принтеров, имеющих самую высокую цену. 
Вывести: model, price
```sql
SELECT model, price 
FROM Printer
WHERE price = (SELECT max(price) 
FROM Printer)
```

**Exercise: 11.** Найдите среднюю скорость ПК.
```sql
SELECT AVG(speed) FROM PC
```

**Exercise: 12.** Найдите среднюю скорость ПК-блокнотов, цена которых превышает 1000 дол.
```sql
SELECT AVG(speed) FROM Laptop
WHERE price>1000
```

**Exercise: 13.** Найдите среднюю скорость ПК, выпущенных производителем A.
```sql
SELECT AVG(speed) FROM PC
JOIN Product ON PC.model = Product.model
WHERE maker = 'A'
```

**Exercise: 15.** Найдите размеры жестких дисков, совпадающих у двух и более PC. 
Вывести: HD
```sql
SELECT hd FROM PC
GROUP BY hd
HAVING count(hd)>=2
```

**Exercise: 16.** Найдите модели ПК-блокнотов, скорость которых меньше скорости каждого из ПК.
Вывести: type, model, speed
```sql
SELECT DISTINCT A.model AS model, B.model AS model, A.speed, A.ram
FROM PC AS A, PC B
WHERE A.speed = B.speed AND A.ram = B.ram AND
 A.model > B.model
```

**Exercise: 17.** Найдите пары моделей PC, имеющих одинаковые скорость и RAM. В результате каждая пара указывается только один раз, т.е. (i,j), но не (j,i), Порядок вывода: модель с большим номером, модель с меньшим номером, скорость и RAM.
```sql
SELECT DISTINCT type, Laptop.model, speed FROM Laptop
JOIN Product ON Laptop.model = Product.model
WHERE Speed < ALL (SELECT Speed FROM PC)
```

**Exercise: 18.** Найдите производителей самых дешевых цветных принтеров.
Вывести: maker, price
```sql
SELECT DISTINCT maker, price FROM Printer INNER 
JOIN Product ON Printer.model = Product.model
WHERE price = (SELECT MIN(price) 
FROM Printer
WHERE color = 'y') AND
color = 'y'
```

**Exercise: 19.** Для каждого производителя, имеющего модели в таблице Laptop, найдите средний размер экрана выпускаемых им ПК-блокнотов.
Вывести: maker, средний размер экрана.
```sql
SELECT maker, AVG(screen) AS Avg_screen FROM Laptop
Join Product ON Product.model = Laptop.model
Group by maker
```

**Exercise: 20.** Найдите производителей, выпускающих по меньшей мере три различных модели ПК.
Вывести: Maker, число моделей ПК.
```sql
SELECT maker, COUNT(model) FROM Product
WHERE type = 'PC'
GROUP BY maker
HAVING COUNT(model) >= 3
```

---
![БД](https://sql-ex.ru/images/ships.gif)
Рассматривается БД кораблей, участвовавших во второй мировой войне. Имеются следующие отношения:
Classes (class, type, country, numGuns, bore, displacement)
Ships (name, class, launched)
Battles (name, date)
Outcomes (ship, battle, result)
Корабли в «классах» построены по одному и тому же проекту, и классу присваивается либо имя первого корабля, построенного по данному проекту, либо названию класса дается имя проекта, которое не совпадает ни с одним из кораблей в БД. Корабль, давший название классу, называется головным.
Отношение Classes содержит имя класса, тип (bb для боевого (линейного) корабля или bc для боевого крейсера), страну, в которой построен корабль, число главных орудий, калибр орудий (диаметр ствола орудия в дюймах) и водоизмещение ( вес в тоннах). В отношении Ships записаны название корабля, имя его класса и год спуска на воду. В отношение Battles включены название и дата битвы, в которой участвовали корабли, а в отношении Outcomes – результат участия данного корабля в битве (потоплен-sunk, поврежден - damaged или невредим - OK).
Замечания. 1) В отношение Outcomes могут входить корабли, отсутствующие в отношении Ships. 2) Потопленный корабль в последующих битвах участия не принимает.

**Exercise: 14.** Найдите класс, имя и страну для кораблей из таблицы Ships, имеющих не менее 10 орудий.
```sql
SELECT ships.class, name, country 
FROM Ships
JOIN Classes ON Ships.class= Classes.class
WHERE numGuns>=10
```