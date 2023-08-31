Похожи на временные таблицы. Создаются через оператор WITH. Называются еще CTE

Если нерекурсивное CTE используется в запросе только один раз, то оно встраивается во внешний запрос. Если несколько, то поведение меняется: планирование CTE выполняется отдельно от остальной части запроса. 
Кроме того, ключевое слово MATERIALIZED вызывает последнее поведение (т.е. отдельное планирование), а NOT MATERIALIZED наоборот. 

___
```
WITH flights_totals AS (
	SELECT bl.flight_id,
		departure_airport,
		(avg(price))::numeric(7,2) AS avg_price,
		count(DISTINCT passenger_id) AS num_passengers
	FROM booking b
		JOIN booking_leg bl USING (booking_id)
		JOIN flight f USING (flight_id)
		JOIN passenger p USING (booking_id)
	GROUP BY 1,2
)
SELECT flight_id,
	avg_price,
	num_passengers
FROM flights_totals
WHERE departure_airport = 'ORD';
```

___
Relations: [[Длинные запросы SQL]]  
Tags: #code  
References: [[Оптимизация запросов PostgreSQL Домбровская]] 
Query: 