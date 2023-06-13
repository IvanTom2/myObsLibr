___
Представление - это объект, который не является материализованной таблицей, а лишь кодом, который способен эту таблицу извлечь. 

Здесь возникает проблема с инкапсуляцией. Нет понимания какой запрос скрывает представление - и это ведет к проблемам. Например, внутри такого запроса может присутствовать приведения к типу, что приведет к невозможности использовать индекс, а это приведет к уменьшению скорости выполнения запроса. 

Представления не дают никаких преимуществ в скорости выполнения запросов (могут только мешать при недолжном знании, как записан их код).
___
[[Code]]:
```
CREATE VIEW flight_stats AS
SELECT bl.flight_id,
	departure_airport,
	(avg(price))::numeric(7,2) AS avg_price,
	count(DISTINCT passenger_id) AS num_passengers
FROM booking b
	JOIN booking_leg bl USING (booking_id)
	JOIN flight f USING (flight_id)
	JOIN passenger p USING (booking_id)
GROUP BY 1,2;
```
___
Language: [[SQL]]
Key-words:  [[Длинные запросы SQL]] [[Оптимизация запросов PostgreSQL]]
Question query?: 