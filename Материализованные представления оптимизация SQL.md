Материализованное представление отличается от обычного тем, что хранит не только код запроса, но и саму таблицу. Тем не менее, в отличии от таблицы, материализованное представление не обновляется и содержит данные на момент времени, когда оно вызывалось или делало REFRESH. 

В целом, материализованные представления предоставляют более быстрый доступ к данным, но требуют обновления. Если данные часто обновляются и нужен постоянный доступ именно к свежим данным, то материализованные представления навряд ли подойдут. 

Кроме того, материализованное представление можно использовать в различных запросах! 

___
```
CREATE MATERIALIZED VIEW flight_departure_mv AS
SELECT bl.flight_id,
	departure_airport,
	coalesce(actual_departure, scheduled_departure)::date departure_date,
	count(DISTINCT passenger_id) AS num_passengers
FROM booking b
	JOIN booking_leg bl USING (booking_id)
	JOIN flight f USING (flight_id)
	JOIN passenger p USING (booking_id)
GROUP BY 1,2,3;
```

___
Relations: [[Длинные запросы SQL]] [[Представления и оптимизация SQL]] 
Tags: #theory #code 
References: [[Оптимизация запросов PostgreSQL Домбровская]] 
Query: 