Группировка должна выполняться по принципу: сначала фильтруем, потом группируем.  Фильтрация всех столбцов, используемых в предложении GROUP BY, должна выполняться на уровне группировки. 

В некоторых ситуациях порядок должен быть противоположный: сначала группируем, потом выполняем все другие операции. Так необходимо сделать, когда группировка уменьшает размер промежуточного набора данных. 

___
```
1. Фильтрация перед группировкой

1.1 Плохо!
SELECT a.flight_id,
	a.avg_price,
	a.num_passengers
FROM (
	SELECT bl.flight_id,
		departure_airport,
		(avg(price))::numeric (7,2) AS avg_price,
		count(DISTINCT passenger_id) AS num_passengers
	FROM booking b
		JOIN booking_leg bl USING (booking_id)
		JOIN flight f USING (flight_id)
		JOIN passenger p USING (booking_id)
	GROUP BY 1,2
) a
	WHERE flight_id IN (
	SELECT flight_id
	FROM flight
	WHERE scheduled_departure BETWEEN '07-03-2020' AND '07-05-2020'
);

1.2 Хорошо!
SELECT bl.flight_id,
	departure_airport,
	(avg(price))::numeric (7,2) AS avg_price,
	count(DISTINCT passenger_id) AS num_passengers
FROM booking b
	JOIN booking_leg bl USING (booking_id)
	JOIN flight f USING (flight_id)
	JOIN passenger p USING (booking_id)
WHERE scheduled_departure BETWEEN '07-03-2020' AND '07-05-2020'
GROUP BY 1,2

2. Сначала группируем, затем выбираем

2.1 Плохо! 
SELECT city,
	date_trunc('month', scheduled_departure) AS month,
	count(*) passengers
FROM airport a
	JOIN flight f ON airport_code = departure_airport
	JOIN booking_leg l ON f.flight_id =l.flight_id
	JOIN boarding_pass b ON b.booking_leg_id = l.booking_leg_id
GROUP BY 1,2
ORDER BY 3 DESC

2.2 Хорошо!
SELECT city,
	date_trunc('month', scheduled_departure),
	sum(passengers) passengers
FROM airport a
	JOIN flight f ON airport_code = departure_airport
	JOIN (
		SELECT flight_id, count(*) passengers
			FROM booking_leg l
				JOIN boarding_pass b USING (booking_leg_id)
		GROUP BY flight_id
	) cnt USING (flight_id)
GROUP BY 1,2
ORDER BY 3 DESC
```

___
Relations: [[Длинные запросы SQL]] 
Tags: #code
References: [[Оптимизация запросов PostgreSQL Домбровская]] 
Query: 