___
Какие проблемы может вызывать временная таблица:
1. Отсутствие индексов во временной таблице, в отличии от дефолтной. Т.е. когда мы создаем временную таблицу, нам необходимо создавать индексы по новой, если мы хотим продолжать ей пользоваться также эффективно, как дефолтными (у которых, конечно, индексы созданы). 
2. Отсутствие статистики - оптимизатор не может использовать информацию о статистике, т.к. ее нет, т.к. таблица новая. 
3. Занимает место на диске. 
4. Чрезмерный ввод-вывод. Временная таблица - это таблица, а для ее сохранения необходимо сохранить ее на жестком диске. Для чтения также необходимо время. 
5. Вызов временной таблицы не дает шансов оптимизатору выбирать оптимальный порядок соединений. 
___
[[Code]]:
```
1. Неэффективное использование временной таблицы

CREATE TEMP TABLE flights_totals AS
SELECT bl.flight_id,
	departure_airport,
	(avg(price))::numeric(7,2) AS avg_price,
	count(DISTINCT passenger_id) AS num_passengers
FROM booking b
	JOIN booking_leg bl USING (booking_id)
	JOIN flight f USING (flight_id)
	JOIN passenger p USING (booking_id)
GROUP BY 1,2;
SELECT flight_id,
	avg_price,
	num_passengers
FROM flights_totals
WHERE departure_airport = 'ORD';
```
___
Language: [[SQL]]
Key-words:  [[Оптимизация запросов PostgreSQL]] [[Длинные запросы SQL]]
Question query?: 