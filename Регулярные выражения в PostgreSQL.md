___
В psql для регулярных выражений прописывается тильда ~
1. ~ 'regex' поиск регулярки
2. !~ 'regex' обратный поиск регулярки
3. !~* 'regex' обратный поиск регулярки без учета регистра

Для более эффективного поиска можно индексировать текстовые столбцы (при условии, что значения представлены в нижнем регистре). 
___
[[Code]]:
```
SELECT COUNT(*) FROM movies; 2861
SELECT COUNT(*) FROM movies WHERE title ~ '^the'; 0
SELECT COUNT(*) FROM movies WHERE title ~* '^the'; 650
SELECT COUNT(*) FROM movies WHERE title !~ '^the'; 2861
SELECT COUNT(*) FROM movies WHERE title !~* '^the'; 2211

CREATE INDEX movies_title_pattern ON movies (lower(title) text_pattern_ops);
```
___
Language: [[SQL]]
Key-words:  [[PostgreSQL]] [[Поиск строк в PostgreSQL]]
Question query?: 