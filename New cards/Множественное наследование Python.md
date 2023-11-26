Множественное наследование производится путем наследования сразу нескольких классов. Стоит заметить, что если наследуется два и более класса, у которых есть методы с одинаковым наименование, то у наследователя в качестве метода будет выступать ***метод первого наследуемого класса***. Это можно исправить, явно указав наследование. 

___
```
class Class1(object):
	def func1(self):
		print('Class 1 func 1')

class Class2(Class1):
	def func2(self):
		print('Class 2 func 2')

class Class3(Class1):
	def func1(self):
		print('Class 3 func 1')

	def func2(self):
		print('Class 3 func 2')

	def func3(self):
		print('Class 3 func 3')

class Class4(Class2, Class3):
	def func4(self):
		print('Class 4 func 4')

class Class5(Class2, Class3):
	func2 = Class3.func2

	def func5(self):
		print('Class 5 func 5')

```
___
Зачем это нужно: [[]] 
Примеры применения: [[]] 
Основные ошибки: [[]]
___
Relations: [[Python Hints]] [[Классы Python]] 
Tags: #code
References: [[Python 3 и PyQT 6]] 
Query: 