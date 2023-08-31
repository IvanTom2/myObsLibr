Созданы как развитие идеи сканирования только индекса. Специально разработаны для включения в индекс столбцов, необходимых для частых запросов определенного вида. 

___
```
CREATE INDEX flight_depart_arr_sched_dep_inc_sched_arr
	ON flight (departure_airport,
				arrival_airport,
				scheduled_departure)
	INCLUDE (scheduled_arrival);
```

___
Relations: [[Индексы SQL]] [[Составные индексы SQL]] 
Tags: #code 
References: [[Оптимизация запросов PostgreSQL Домбровская]] 
Query: 