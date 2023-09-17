Как известно, для вставки данных в БД используется команда `execute`. Для извлечения данных используется команда `fetch`. 

Данные возвращаются в структуре `Record`, которая похожа на словарь. Если бы мы хотели извлечь одну запись, то можно было бы использовать `fetchrow`. Между `fetch` и `fetchrow` ***нет разницы в производительности***, так как все результаты сохраняются в память (а не подмножество, именно все). 

___
```
import asyncpg
import asyncio
from asyncpg import Record
from typing import List


async def main():
    connection: asyncpg.connection.Connection = await asyncpg.connect(
        host="127.0.0.1",
        port=5432,
        user="admin",
        database="asyncpg_test",
        password="Parad228Hurt",
    )

    await connection.execute("INSERT INTO brand VALUES(DEFAULT, 'Levis')")
    await connection.execute("INSERT INTO brand VALUES(DEFAULT, 'Seven')")

    brand_query = "SELECT brand_id, brand_name FROM brand"
    results: List[Record] = await connection.fetch(brand_query)

    for brand in results:
        print(f'id: {brand["brand_id"]}, name: {brand["brand_name"]}')

    await connection.close()


if __name__ == "__main__":
    asyncio.run(main())

```
___

Relations: [[Асинхронная работа с БД asyncio Python]] 
Tags: #code
References: [[Asyncio (книга)]] 
Query: 