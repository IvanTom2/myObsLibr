Данный паттерн проектирования используется в микросервисной архитектуре с целью сократить количество запросов на сервис, который не отвечает.

Сервис может перестать отвечать вследствие высокой нагрузки или сетевых проблем. В таком случае, может быть предусмотрено N-количество ретраев. Все ретраи будут выполнять повторные запросы на заведомо неработающий сервис и занимать ресурсы и время. 

Есть три состояния, которые предлагает данный паттерн:
1. Замкнутость - когда все работает и запросы к сервису отправляются.
2. Разомкнутость - когда в течение определенного периода произошло определенное количество ошибок, после чего ***любые запросы к этому сервису отправляться не будут***. Имеется в виду то, что на уровне сервиса произошла ошибка и смысла отправлять к нему запросы не имеет. 
3. Полуразомкнутость - данное состояние возникает после выхода из состояния разомкнутости (после определенного количества времени нахождения в состоянии разомкнутости). В данном состоянии отправляется единственный запрос на проверку того, была ли исправлена ошибка. Если сервис начинает работать исправно, то он прерыватель переходи в состояние замкнутости. 
![[Pasted image 20230923112735.png]]

___
```
import asyncio
from datetime import datetime, timedelta


class CircuitOpenException(Exception):
    pass


class CircuitBreaker:
    def __init__(
        self,
        callback,
        timeout: float,
        time_window: float,
        max_failures: int,
        reset_interval: float,
    ):
        self.callback = callback
        self.timeout = timeout
        self.time_window = time_window
        self.max_failures = max_failures
        self.reset_interval = reset_interval

        self.last_request_time = None
        self.last_failure_time = None
        self.current_failures = 0

    async def request(self, *args, **kwargs):
        if self.current_failures >= self.max_failures:
            if datetime.now() > self.last_request_time + timedelta(
                seconds=self.reset_interval
            ):
                self._reset("Circuit is going from open to closed, resetting!")
                return await self._do_request(*args, **kwargs)
            else:
                print("Circuit is open, failing fast!")
                raise CircuitOpenException()
        else:
            if (
                self.last_failure_time
                and datetime.now()
                > self.last_failure_time + timedelta(seconds=self.time_window)
            ):
                self._reset("Interval since first failure elapsed, resetting!")
            print("Circuit is closed, requesting!")
            return await self._do_request(*args, **kwargs)

    def _reset(self, msg: str):
        print(msg)
        self.last_failure_time = None
        self.current_failures = 0

    async def _do_request(self, *args, **kwargs):
        try:
            print("Making request!")
            self.last_request_time = datetime.now()
            return await asyncio.wait_for(
                self.callback(*args, **kwargs), timeout=self.timeout
            )
        except Exception as e:
            self.current_failures = self.current_failures + 1
            if self.last_failure_time is None:
                self.last_failure_time = datetime.now()
            raise


```
___

Relations: [[Микросервисы]] [[Микросервисы asyncio Python]] 
Tags: #code
References: [[Asyncio (книга)]] 
Query: 