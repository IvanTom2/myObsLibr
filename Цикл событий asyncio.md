Цикл событий необходим для низкоуровневых операций. 

Управляя циклом событий, его нужно вызвать. Кроме того, при окончании асинхронных операций его необходимо закрыть. 

Для выполнения функции на следующей итерации цикла необходимо вызвать метод цикла событий call_soon(). 

___
```
import asyncio

loop = asyncio.new_event_loop()
loop.run_until_complete(func())
loop = asnycio.get_running_loop()
loop.call_soon(new_func())
loop.close()
```

___
Relations: [[asyncio Python]] 
Tags: #code 
References: [[Asyncio (книга)]] 
Query: 