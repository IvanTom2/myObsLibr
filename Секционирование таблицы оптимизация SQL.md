Секционирование - создание таблицы с секциями. Например, секционирование диапазоном. В таком случае диапазоны, назначенные разным секциям, не могут пересекаться, а строка, которая не попадает ни в одну секцию, не может быть помещена в таблицу. 

Секционирование может сократить время, необходимое для полного сканирования таблицы. 

___
```
CREATE TABLE boarding_pass_part (
	boarding_pass_id SERIAL,
	passenger_id BIGINT NOT NULL,
	booking_leg_id BIGINT NOT NULL,
	seat TEXT,
	boarding_time TIMESTAMPTZ,
	precheck BOOLEAN NOT NULL,
	update_ts TIMESTAMPTZ
	)
PARTITION BY RANGE (boarding_time);
```

___
Relations: [[Длинные запросы SQL]] 
Tags: #code  
References: [[Оптимизация запросов PostgreSQL Домбровская]] 
Query: 