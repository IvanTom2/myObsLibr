Чтобы добавить записи через Python, можно использовать дефолтные `requests`. Либо асинхронные запросы, чтобы дело шло быстрее. 

Вся суть в том, чтобы правильно взаимодействовать с Riak через REST HTTP. 

___
```
import json
import random
import requests


styles = ["single", "double", "king", "suite", "queen"]

for floor in range(1, 100):
    rooms_block = floor * 100
    for room in range(1, 101):
        style = random.sample(styles, 1)[0]
        key = rooms_block + room

        data = {
            "room": key,
            "capacity": random.randint(0, 8) + 1,
            "style": style,
        }

        url = f"http://localhost:8098/riak/rooms/{key}"

        data_json = json.dumps(data)
        response = requests.put(
            url,
            json=data_json,
            headers={
                "Content-Type": "application/json",
            },
        )

        if response.status_code == 204:
            print(f"Room {key} added successfully.")
        else:
            print(f"Failed to add room {key}: {response.text}")

```
___
Зачем это нужно: [[]] 
Примеры применения: [[]] 
Основные ошибки: [[]]
___
Relations: [[REST Riak]] [[Python Hints]] 
Tags: #code
References: 
Query: 