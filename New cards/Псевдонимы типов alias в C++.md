Для определения псевдонимов существуют несколько конструкций:
1. `typedef` - ключевое слово при объявлении. 
2. `using` - объявление псевдонима. 

***Задается псевдоним типа!*** Иными словами, для определения переменных можно будет использовать не `int`, а `pseudoname_int`. 

___
```
typedef double wages;
using count = int; 

// пример
typedef double wages;
using counts = int;

wages wage = 0.04;
counts count = 10;


```
___
Зачем это нужно: [[]] 
Примеры применения: [[]] 
Основные ошибки: [[]]
___
Relations: [[Работа с типами в C++]] 
Tags: #code
References: 
Query: 