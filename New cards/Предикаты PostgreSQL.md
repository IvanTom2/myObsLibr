Предикатами являются логические выражения, значения на выходе которых являтся `TRUE`, `FALSE` или `UNKNOWN`. Предикаты можно объединять в выраения с помощью булевых операторов `AND`, `OR`, `NOT` порядок которых можно конкретизировать с помощью скобок (как в арифметических операциях). 
![[Pasted image 20231105114439.png]]

Стоит отметить, что предикаты выполняют реляционную операцию ограничения. Сложные предикаты могут быть переписаны еще в более сложные выражения. 
1. Предикат `condition1 AND condition2` эквивалентен ***пересечению*** ограничений по каждому из предикатов. `SELECT*FROM product WHERE maker = 'A' INTERSECT SELECT*FROM product WHERE maker = 'B'`. 
2. Предикат `condition1 OR condition2` эквивалентен ***объединению*** ограничений по каждому из предикатов. `SELECT*FROM product WHERE maker = 'A' UNION SELECT*FROM product WHERE maker = 'B'`. 
3. Предикат `NOT condition` эквивалентен разности исходного отношения и отношения с предикатом `condition`. `SELECT*FROM product EXCEPT SELECT*FROM product WHERE maker = 'A'`. 

Также существует предикат `BETWEEN`, которые проверяет, находятся ли значения атрибута в заданном интервале. `SELECT*FROM pc WHERE price BETWEEN 100 AND 500`. 

Существует предикат `IN` для проверки присутствия значений атрибута в заданном множестве значений. В качестве множества значений может выступать результирующее отношение. Стоит отметить, что результирующее отношение должно иметь в заголовке только один атрибут, иначе будет возникать ошибка. 

___
```
SELECT maker, model, type
FROM Product 
WHERE type='PC' AND (maker='A' OR maker='B');

SELECT*FROM pc 
WHERE hd IN (10,20) 
AND model IN 
(SELECT model FROM product WHERE maker = 'A');

```
___
Зачем это нужно: [[]] 
Примеры применения: [[]] 
Основные ошибки: [[]]
___
Relations: [[SELECT PostgreSQL]] 
Tags: #code
References: 
Query: 