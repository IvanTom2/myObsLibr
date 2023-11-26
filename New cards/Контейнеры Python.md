Контейнер - это объект, который хранит и предоставляет доступ к массиву значений. По сути, самым просты контейнером является `List`. 

Чтобы превратить класс в контейнер, необходимо определить следующие дандер-методы:
1. `__getitem__(self, index)` - вызов элемента по индексу.
2. `__setitem__(self, index, value)` - изменение значения по индексу. 
3. `__delitem__(self, index)` - удаление значения по индексу.
4. `__contains__(self, value)` - проверка наличия значения в контейнере. 

___
```
class MutableString:
	def __init__(self, s):
		self.__s = list(s)
  
	def __iter__(self):
		self.__i = 0
		return self
		
	def __next__(self):
		if self.__i > len(self.__s) - 1:
			raise StopIteration
		else:
			a = self.__s[self.__i]
			self.__i = self.__i + 1
			return a
			
	def __len__(self):
		return len(self.__s)
	
	def __str__(self):
		return "".join(self.__s)
		
	def __iscorrectindex(self, i):
		if type(i) == int or type(i) == slice:
			if type(i) == int and i > self.__len__() - 1:
				raise IndexError
		else:
			raise TypeError
			
	def __getitem__(self, i):
		self.__iscorrectindex(i)
		return self.__s[i]
		
	def __setitem__(self, i, v):
		self.__iscorrectindex(i)
		self.__s[i] = v
		
	def __delitem__(self, i):
		self.__iscorrectindex(i)
		del self.__s[i]
		
	def __contains__(self, v):
		return v in self.__s



```
___
Зачем это нужно: [[]] 
Примеры применения: [[]] 
Основные ошибки: [[]]
___
Relations: [[Классы Python]] 
Tags: #code
References: [[Python 3 и PyQT 6]] 
Query: 