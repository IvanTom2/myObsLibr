Условные конструкции в Ruby, на первый взгляд, выглядят простыми. Здесь можно использовать однострочные условные конструкции, многострочные и даже выделена обратная конструкция `unless`, которая, по сути, является альтернативой `if not` в Python. 

Важно отметить, что в отличии от Python, в Ruby нет необходимости прописывать двоеточие `:` после каждой условной конструкции. 

Каждая условная конструкция (кроме построчных) должна открываться через определенное ключевое слово (`if`, `unless`) и закрываться через `end`, что означает конец условной конструкции. В противном случае интерпретатор выведет ошибку. 

___
```
x = 4
puts("This appear to be false") if not false
puts("This appear to be false") if not !true

puts("This appear to be false") unless x == 4
puts("This appear to be true") if x == 4

if x == 4
    puts("This appear to be true")
elsif x == 5
    puts("This appear to be false")     
else 
    puts("Is it an error?")
end

unless x == 4
    puts("This appear to be false")
else
    puts("This appear to be true")
end

```
___
Зачем это нужно: [[]] 
Примеры применения: [[]] 
Основные ошибки: [[]]
___
Relations: [[Ruby]] [[Булева алгебра и логические операторы Ruby]] 
Tags: #code 
References: [[Семь языков программирования за семь недель]] 
Query: 