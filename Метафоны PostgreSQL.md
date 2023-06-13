___
Метафоны - это представление слов в виде их звучания (в строковом виде). Есть различные алгоритмы сравнения звучания строк - metaphone, dmetaphone, dmetaphone_alt, soundex. Функции могут показывать различный перформанс в рамках различных наборов данных. 
___
[[Code]]:
```
SELECT title
FROM movies NATURAL JOIN movies_actors NATURAL JOIN actors
WHERE metaphone(name, 6) = metaphone(‘Broos Wils’, 6);
```
___
Language: [[SQL]]
Key-words:  [[PostgreSQL]] [[Поиск строк в PostgreSQL]]
Question query?: 