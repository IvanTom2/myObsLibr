Как и в Python, любой объект, который представляет собой значение, отличное от `nil` или `false` является значение `true`. Например, можно использовать в условной конструкции строку или литеральное значение. Хотя на некоторые кейсы Ruby выдает `warning`, это не крашит программу. 

***Очень важно!*** Значение 0 в Ruby интерпретируется как `true`. В том числе, пустая строка интерпретируется также как `true`. 

Для логического "И" в Ruby предусмотрено ключевое слово `and` и его альтернатива `&&`. Для логического "ИЛИ" предусмотрено ключевое слово `or` и его альтернатива `||`. 

___
```
puts 'This appears to be true.' if 1
puts 'This appears to be true.' if 0

puts 'This appears to be true.' if "random string"
puts 'This appears to be true.' if ""

puts 'This appears to be true.' if true
puts 'This appears to be true.' if false
puts 'This appears to be true.' if nil

```
___
Зачем это нужно: [[]] 
Примеры применения: [[]] 
Основные ошибки: [[]]
___
Relations: [[Ruby]] 
Tags: #code
References: [[Семь языков программирования за семь недель]] 
Query: 