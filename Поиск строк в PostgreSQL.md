___
В SQL дефолтный способ поиска по тексту - оператор LIKE (ILIKE) и регулярные выражения (в т.ч. в psql). 

Разница между LIKE и ILIKE в том, что ILIKE применяется без учета регистра. 

% и _ являются метасимволами. Применимы только с LIKE (ILIKE). 
% сопоставляется с произвольным числом символов (например, после строки string)
\_ ровно один произвольный символ (например, после строки string)
\_% - один символ и произвольное количество символов

Если не прописывать эти метасимволы, то поиск будет идти по вхождению строки (судя по всему):
1. ILIKE 'string' будет искать строку 'string', 'STRING', 'StRiNg' и т.д.
2. LIKE 'string' будеть искать конкретную строку 'string'
___
[[Code]]:
```
Например, в БД есть Stardust, Stardust Memories

SELECT title FROM movies WHERE title ILIKE 'stardust'; // Stardust
SELECT title FROM movies WHERE title ILIKE 'stardust%'; // Stadust, Stardust Mem
SELECT title FROM movies WHERE title ILIKE 'stardust_%'; // Stardust Memories
SELECT title FROM movies WHERE title ILIKE 'stardust_'; // NULL
SELECT title FROM movies WHERE title ILIKE 'stardus_'; // Stardust


```
___
Language: [[SQL]]
Key-words:  [[PostgreSQL]]
Question query?: поиск строк в PostgreSQL