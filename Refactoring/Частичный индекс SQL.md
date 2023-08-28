___
Идея частичного индекса состоит в том, чтобы использовать только подмножество таблицы, т.е. с условием WHERE. 

Например, второй индекс из code. Этот индекс будет использоваться во всех запросах, в которых мы выбираем отмененные рейсы, независимо от любых других условий фильтрации. 
___
[[Code]]:
```
CREATE INDEX flight_actual_departure_not_null ON flight (actual_departure)
		WHERE actual_departure IS NOT NULL;

CREATE INDEX flight_canceled ON flight (flight_id)
		WHERE status = 'Canceled';
```
___
Language: [[SQL]]
Key-words:  [[Индексы SQL]]
Question query?: 