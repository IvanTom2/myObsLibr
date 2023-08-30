Комбинированный подход для поиска строк заключается в том, чтобы сгруппировать различных методы для улучшения перформанса поиска. 

___
[[Code]]:
```
SELECT * FROM actors
WHERE metaphone(name,8) % metaphone(‘Robin Williams’,8)
ORDER BY levenshtein(lower(‘Robin Williams’), lower(name));
```

___
Relations: [[Поиск строк в PostgreSQL]] 
Tags: #code
References: [[Семь баз данных]] 
Query: 