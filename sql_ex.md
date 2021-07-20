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