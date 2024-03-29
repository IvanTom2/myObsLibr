PostgreSQL поддерживает простые средства обработки естественного языка. Используется так называемый полнотекстовый движок. Он принимает, например, отдельные слова и возвращает соответствующие результаты. 

Оператор @@ преобразует поле для поиска в тип tsvector, а сам запрос в тип tsquery. 
Слова в поле для поиска преобразуются в каком-либо виде. Кроме того, удаляются стоп слова (например английское "а"), т.к. по ним зачастую искать не имеет смысла. Такие слова находятся в файле english.stop в каталоге tsearch_data. 

В рамках преобразования слов применяются и другие подходы. Например, стеммизация слов и прочее. 

Полнотекстовый поиск будет очень медленно работать, если таблицы не индексированы. Индекс по значениям лексем имеет тип GIN (Generalized Inverted iNdex). 

___
```
SELECT title FROM movies WHERE title @@ 'night & day';

SELECT to_tsvector('A Hard Day''s Night'), to_tsquery('english', 'night &amp; day');

// для ускорения поиск можно создать специальный индекс по лексемам
CREATE INDEX movies_title_searchable ON movies USING gin(to_tsvector(‘english’, title));
// после создания индекса важно делать запрос не по дефолту, а в следующей форме
SELECT * FROM movies WHERE to_tsvector(‘english’,title) @@ ‘night & day’;
```

___
Relations: [[Поиск строк в PostgreSQL]] 
Tags: #code 
References: [[Семь баз данных]]
Query: 