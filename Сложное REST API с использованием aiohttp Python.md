Посколько мы ожидаем, что у нашего приложения будет много пользоавтелей, то нужно как-то оптимизировать его работу. 

Например, если приложение выполняет большое количество запросов к базе данных, то было бы неплохой выделить для него пул подключений к БД. 

Возникает вопрос - где хранить разделяемые данные в приложениях на основе `aiohttp`. Для хранения разделяемых данных класс `Application` содержит словарь. Тогда возникает вопрос, а как именно обращаться к самому приложению - т.е. экземпляру `Application`. Это можно сделать с помощью использования глобальной переменной, но есть способ лучше. `aiohttp` предоставляет класс `request`, который уже содержит в себе ссылку на экземпляр приложения! 

Второй вопрос - где именно создавать пул подкючений. Создавать его там же, где создается экземпляр приложения мы не можем, так как это будет происходить вне сопрограммы. Класс `Application` содержит в себе два дополнительных метода для решения проблемы:
1. Инициализация дополнительных ресурсов `.on_startup`
2. Деинсталляция ресурсов `.on_cleanup`

Оба метода принимают сопрограммы, которые сохраняются в экземпляре приложения. 

Для создания ***маршрута с параметрами***, в `aiohttp` используются фигурные скобки `{param}`. Так как можно передавать любой параметр, может возникнуть ситуация, что по нему ничего не вернется - в таком случае можно возвращать ошибку с определенными статусом (например 404). 
Переданный параметр также находится в экземпляре `Request` и его можно получить через метод `match_info['param']`. 

Стоит заметить, что обернутая декоратором-маршрутом сопрограмма при возникновении ошибки навряд-ли уронит сервер. Кроме того, если возникнет ошибка из определенного классом `web` пула ошибок, то она будет возвращена клиенту. Например, если возникнет какая-то ошибка, то (если это заранее определено) вернется ошибка `web.HTTPBadRequest()`.  

___
```
import asyncpg
from aiohttp import web
from aiohttp.web_app import Application
from aiohttp.web_request import Request
from aiohttp.web_response import Response
from asyncpg import Record
from asyncpg.pool import Pool
from typing import List, Dict


routes = web.RouteTableDef()
DB_KEY = "database"


async def create_database_pool(app: Application) -> None:
    print("Создается пул подключений к базе данных")
    pool: Pool = await asyncpg.create_pool(
        host="127.0.0.1",
        port=5432,
        user="admin",
        password="Parad228Hurt",
        database="asyncpg_test",
        min_size=6,
        max_size=6,
    )

    app[DB_KEY] = pool


async def destroy_database_pool(app: Application) -> None:
    print("Деинсталляция пула подключений к базе данных")
    pool: Pool = app[DB_KEY]
    await pool.close()


@routes.get("/brands")
async def get_brands(request: Request) -> Response:
    connection = request.app[DB_KEY]
    brand_query = "SELECT brand_id, brand_name FROM brand"

    results: list[Record] = await connection.fetch(brand_query)
    results_as_dict: list[dict] = [dict(brand) for brand in results]
    return web.json_response(results_as_dict)


@routes.get("/product/{id}")
async def get_product(request: Request) -> Response:
    try:
        string_id = request.match_info["id"]
        product_id = int(string_id)

        query = """
            SELECT product_id, product_name, brand_id
            FROM product
            WHERE product_id = $1;
            """

        connection: Pool = request.app[DB_KEY]
        result: Record = await connection.fetchrow(query, product_id)

        if result:
            return web.json_response(dict(result))
        return web.HTTPBadRequest()

    except Exception as ex:
        raise web.HTTPBadRequest()


@routes.post("/product")
async def create_product(request: Request) -> Response:
    PRODUCT_NAME = "product_name"
    BRAND_ID = "brand_id"

    if not request.can_read_body:
        raise web.HTTPBadRequest()

    body = await request.json()

    if PRODUCT_NAME in body and BRAND_ID in body:
        pool: Pool = request.app[DB_KEY]

        query = """
        INSERT INTO product(product_id, product_name, brand_id)
        VALUES (DEFAULT, $1, $2);
        """

        await pool.execute(
            query,
            body[PRODUCT_NAME],
            int(body[BRAND_ID]),
        )

        return web.Response(status=201)
    else:
        raise web.HTTPBadRequest()


if __name__ == "__main__":
    app = web.Application()
    app.on_startup.append(create_database_pool)
    app.on_cleanup.append(destroy_database_pool)
    app.add_routes(routes)

    web.run_app(app, host="127.0.0.1", port=8000)

```
___

Relations: [[asyncio Python]] [[Простое REST API с использованием aiohttp Python]] 
Tags: #code
References: [[Asyncio (книга)]] 
Query: 