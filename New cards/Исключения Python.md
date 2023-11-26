Вызвать исключение можно 3 способами:
1. `raise Exception` - вызов класса исключения, автоматически формируется объект исключения.
2. `raise Exception(msg)` - вызов экземпляра исключения.
3. `raise ValueError() from err` - генерация одного исключения из другого (актуально при обработке исключений). 

![[Pasted image 20231126155350.png]]

___
```
try:
	...
except ExceptionClass:
	...
except ExceptionClass:
	...
else:
	# когда не произошла исключение
	...
finally:
	# последний блок, который обязательно исполняется
	...

try:
	x = 1 / 0
except Exception as err:
	raise ValueError() from err

class MyError(Exception):
	def __init__(self, value):
		self.msg = value
	def __str__(self):
		return self.msg

```
___
Зачем это нужно: [[]] 
Примеры применения: [[]] 
Основные ошибки: [[]]
___
Relations: [[Классы Python]] [[Python Hints]] 
Tags: #code
References: 
Query: 