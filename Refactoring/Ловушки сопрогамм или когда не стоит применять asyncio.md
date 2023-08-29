___
1. CPU-bound задачи: вычислительные задачи, в которых следует применять многопроцессорность.
2. Оборачивание неасинхронных API-библиотек по типу requests. Они заблокируют поток выполнения. Даже time.sleep(). 
___
[[Code]]:
```
1. Don't use asycnio with CPU-bound tasks without pool of worker -> it cause increase of execution time. 
2. Don't user asyncio with no-asyncio API's. 
```
___
Language: [[Python]]
Key-words:  [[asyncio Python]]
Question query?: 