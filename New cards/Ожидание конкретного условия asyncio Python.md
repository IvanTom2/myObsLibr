У условий asycnio есть метод `condition.wait_for()`, который позволяет ожидать конкретного предиката, пока он не вернет состояние `True`. Это работает также в связке с `condition.notify_all()` - вызывается триггер и затем перед началом работы будет проведена проверка дополнительного условия. Если оно выполнено, то начинается исполнение дальнейшего кода. 

Такой вид условий полезен, когда имеются общие разделяемые ресурсы. Но на самом деле это довольно редкий случай. 

___
```
import asyncio
from enum import Enum


class ConnectionState(Enum):
    WAIT_INIT = 0
    INITIALIZING = 1
    INITIALIZED = 2


class Connection:
    def __init__(self):
        self._state = ConnectionState.WAIT_INIT
        self._condition = asyncio.Condition()

    async def initialize(self):
        await self._change_state(ConnectionState.INITIALIZING)
        print("initialize: Инициализация подключения...")
        await asyncio.sleep(3)  # имитируется время инициализации подключения
        print("initialize: Подключение инициализировано")
        await self._change_state(ConnectionState.INITIALIZED)

    async def execute(self, query: str):
        async with self._condition:
            print("execute: Ожидание инициализации подключения")
            await self._condition.wait_for(self._is_initialized)
            print(f"execute: Выполняется {query}!!!")
            await asyncio.sleep(3)  # имитация долгого запроса

    async def _change_state(self, state: ConnectionState):
        async with self._condition:
            print(f"change_state: Состояние изменяется с {self._state} на {state}")
            self._state = state
            self._condition.notify_all()

    def _is_initialized(self):
        if self._state is not ConnectionState.INITIALIZED:
            print(
                f"_is_initialized: Инициализация подключения не закончена, состояние равно {self._state}"
            )
            return False
        print(f"_is_initialized: Подключение инициализировано!")
        return True


async def main():
    connection = Connection()
    query_one = asyncio.create_task(connection.execute("select * from table"))
    query_two = asyncio.create_task(connection.execute("select * from other_table"))
    asyncio.create_task(connection.initialize())
    await query_one
    await query_two


if __name__ == "__main__":
    asyncio.run(main())

```
___
Зачем это нужно: [[]] 
Примеры применения: [[]] 
Основные ошибки: [[]]
___
Relations: [[Условия в asyncio Python]] 
Tags: #code
References: [[Asyncio (книга)]] 
Query: 