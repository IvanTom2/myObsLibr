Асинхронная работа с `PostgreSQL` предполагает использование [[Асинхронные генераторы]]. Если данных очень много, то их нельзя поместить в оперативную память. На стороне PostgreSQL помогает ***потоковый курсор базы данных***, который хорошо сочитается с асинхронным генератором. 

В `asyncpg` реализован потоковый курсор `cursor` - это асинхронный генератор. Стоит отметить, что PostgreSQL позволяет потоковое чтение данных только ***при открытой транзакции***. 
У объекта `cursor` есть параметр `prefetch`, который отвечает за то, сколько записей будет захватываться за операцию чтения. 

Помимо этого, курсор можно использовать для пропуска части данных через `cursor.forward` и выборки последующих записей (например, после 500-й).

Также можно использовать собственный генератор для извлечения определенного количества данных. 

___
```
import asyncio
import asyncpg

async def take(generator, to_take: int):
	item_count = 0
	async for item in generator:
		if item_count > to_take - 1:
			return
		item_count = item_count + 1
		yield item

async def main():
    connection: asyncpg.connection.Connection = await asyncpg.connect(
        host="127.0.0.1",
        port=5432,
        user="admin",
        database="asyncpg_test",
        password="Parad228Hurt",
    )

    query = "SELECT product_id, product_name FROM product"
    async with connection.transaction():
        async for product in connection.cursor(query):
            print(product)

    async with connection.transaction():
        cursor = await connection.cursor(query)
        await cursor.forward(500)
        products = await cursor.fetch(100)
        for product in products:
            print(product)

	async with connection.transaction():
		query = 'SELECT product_id, product_name from product'
		product_generator = connection.cursor(query)
		
		async for product in take(product_generator, 5):
			print(product)

    await connection.close()


if __name__ == "__main__":
    asyncio.run(main())

```
___

Relations: [[Асинхронная работа с БД asyncio Python]] 
Tags: #code
References: [[Asyncio (книга)]] 
Query: 