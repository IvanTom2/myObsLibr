Перечислением - это набор каких-либо именованных значений, называемых элементами. Перечисление можно рассматривать как своего рода словарь, только неизменяемый. 

Класс перечисления должен быть производным одного из классов `enum`:
1. `class C1(enum.Enum)`. Базовый класс для создания перечислений, чьи элементы способны хранить значения произвольного типа. Элементы такого перечисления способны содержать одинаковые значения. 
2. `@unique(class C2(enum.Enum))`. Декоратор, который позволяет хранить только уникальные значения. 
3. `class C3(enum.IntEnum)`. Только целочисленные значения. 
4. `class C4(enum.Flag)`. 
5. `class C5(enum.IntFlag)`. 

___
```
from enum import Enum
class Frameworks3(Enum):
	LARAVEL = "Laravel"
	DJANGO = "Django"
	EXPRESS = "Express"
	RAILS = "Ruby on Rails"
	# Обычные методы. Вызываются у элемента перечисления.
	def describe(self):
		return self.name, self.value
	def __str__(self):
		return str(__class__.__name__) + "." + self.name + ": " + \
		self.value
	# Статический метод. Вызывается у самого класса перечисления.
	@classmethod
	def getmostpopular(cls):
		return cls.LARAVEL
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