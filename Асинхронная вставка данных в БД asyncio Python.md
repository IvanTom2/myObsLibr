Для вставки значений используется `execute`. Тем не менее, когда необходимо вставить большое количество значений, можно использовать команду `exectumany`. 

`executemany` принимает одну SQL команду и список параметров, которые обозначаются через `$`. Например, если будет задана команда `executemany("INSERT INTO table VALUES($1, $2)", [('a', 'b'), ('c', 'd')])`, то будет передано две команды `INSERT INTO` со значениями из соответствующих кортежей. 

___
```
import asyncpg
import asyncio
from typing import List, Tuple, Union
from random import randint, sample


def load_common_words() -> List[str]:
    with open(
        "/home/mainus/Learning/Python/Asyncio/async_db/common_words.txt"
    ) as common_words:
        common_words = common_words.readlines()
        return [w.strip() for w in common_words]


def generate_brand_names(words: list[str]) -> list[str]:
    return [(words[index],) for index in sample(range(100), 100)]


async def insert_brands(
    common_words: list[str],
    connection: asyncpg.connection.Connection,
):
    brands = generate_brand_names(common_words)
    insert_brands = "INSERT INTO brand VALUES(DEFAULT, $1)"
    return await connection.executemany(insert_brands, brands)


async def main_brand_insertion():
    common_words = load_common_words()
    connection: asyncpg.connection.Connection = await asyncpg.connect(
        host="127.0.0.1",
        port=5432,
        user="admin",
        database="asyncpg_test",
        password="Parad228Hurt",
    )

    await insert_brands(common_words, connection)

if __name__ == '__main__':
	asyncio.run(main_brand_insertion())

```
___

Relations: [[Асинхронная работа с БД asyncio Python]] 
Tags: #code
References: [[Asyncio (книга)]] 
Query: 