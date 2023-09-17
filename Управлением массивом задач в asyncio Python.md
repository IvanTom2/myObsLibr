`asyncio.gather`
Самый простой способ организации массы задач - это использование `asyncio.gather`. Преимущество заключается в том, что не нужно обертывать все сопрограммы по отдельности в задачи с помощью `asyncio.create_task`. Возвращаемый массив значений задач будет точно такой же, как и переданный - значения будут находиться в том же порядке. 

Недостатки:
1. Обработка исключений. Обработка реализована в двух вариантах, которые переключаются с помощью `asyncio.gather(return_exceptions: bool)`. Проблема в том, что `asyncio.gather` ***не позволяет останавливать запущенные задачи после возникновения исключения***. 
   1. При `return_exceptions = False` будет возбуждено такое же исключение, что в и сопрограмме, в точке `await asyncio.gather()`. Данный режим ***не останавливает прочие уже работающие задачи в рамках gather***. 
   2. При `return_exceptions = True` исключение возвращается в том же списке, что и результаты. Вызов `asyncio.gather` не возбудит исключений и возвращаемые ошибки можно обработать как угодно. 
2. Невозможно получать уже доступные значения выполненных задач до тех пор, пока не будут завершены все задачи. 

`asyncio.as_completed`
Для получения результатов по мере их появления, необходимо использовать конструкцию `asyncio.as_comleted`. 
Помимо прочего, в данной конструкции можно использовать `timeout` который будет возбуждать исключение `TimeoutException` в точке ожидания `await`. 
Проблемы:
1. Данный метод ***возвращает результаты в недетерминированном порядке***. Иными словами, возвращаются результаты тех задач, которые были выполнены первыми. 
2. Исключения по истечению таймаута возбуждаются как положено. Тем не менее, все созданные задачи продолжают работать. Если мы захотим их снять, то будет трудно понять, какие из них еще работают. 

`asyncio.wait`
Более сложный метод, который предоставляет более гибкое управление массивом задач. Возвращает два множества задач: выполненные и ожидающиеся. Среди выполненных могут быть задачи, которые заместо значения содержат ошибку. На входе можно задать несколько способов управления возвратом значений:
1. `ALL_COMPLETED` - схоже на `asyncio.gather` в том, что управление будет оставаться в точке `await asyncio.wait` до тех пор, пока не будут завершены все переданные задачи. Отличие в том, что в точке `await asyncio.wait` не возбуждаются исключения - они возвращаются вместе с задачами. 
2. `FIRST_EXCEPTION` - `asyncio.wait` немедленно вернет задачи при первом возникновении исключения. Далее ожидающиеся задачи можно будет, например, закрыть. А результаты отработавших задач пустить дальше на обработку. 
3. `FIRST_COMPLETED` - это режим обработки задач по мере поступления результатов. 

При использовании `asyncio.wait` можно задать таймаут на выполнение задач. Стоит учитывать:
1. При возникновении таймаута задачи сами по себе ***не снимаются***.
2. При возникновении таймаута не возвращается исключение `TimeoutException`. Метод возвращает все завершенные задачи, а также те, которые в момент таймаута еще не завершились. 
Иными словами, останутся `pending` задачи - они будут работать до тех пор, пока их не снять. 

При использовании `asyncio.wait` рекомендуется оборачивать сопрограммы в задачи. Это необходимо потому что `asyncio.wait` ***возвращает задачи***. Если мы будем передавать сопрограммы, то будет сложно сравнить что мы передали и что мы получили. 

___
```
### asyncio.gather usage
import asyncio

async def func(num: int) -> int:
	await asyncio.sleep(num)
	return num

async def _gather():
	nums = range(len(100))
	rets = [func(num) for num in nums]
	rets = await asyncio.gather(*rets)

### asyncio.as_completed usage
async def _as_compl():
	nums = list(range(10))
	rets = [func(num) for num in nums]
	for finished in asyncio.as_completed(rets, timeout=5):
		try:
			num = await finished
			print(num)
		except asyncio.TimeoutException:
			print('Raised timeout')

### asyncio.wait(return_when='ALL_COMPLETED')
async def _all_compl():
	nums = list(range(10))
	rets = [asyncio.create_task(func(num)) for num in nums]
	done, pending = await asyncio.wait(rets, return_when='ALL_COMPLETED')

	for done_task in done:
		print(await done_task)

### asyncio.wait(return_when='ALL_COMPLETED')
async def _first_compl():
	nums = list(range(10))
	pending = [asyncio.create_task(func(num)) for num in nums]

	while pending:
		done, pending = await asyncio.wait(pending, return_when='FIRST_COMPLETED')

		for done_task in done:
			print(await done_task)


if __name__ == '__main__':
	asyncio.run(_gather())
	asyncio.run(_as_compl())

```
___

Relations: [[asyncio Python]] 
Tags: #code
References: [[Asyncio (книга)]] 
Query: 