Связанный список - это линейная структура данных, в которой существуют связанные узлы. Каждый узел содержит данные и адрес следующего за ним узла. В случае односвязного списка можно переходить лишь вперед по узлам, как показана на изображении ниже. 
![[Pasted image 20231028144845.png]]

Первый узел является головным и называется `HEAD`. Последний узел также может быть определен, потому что последний узел указывает на нулевое значение `NULL`, а не на узел. Последний указатель сетапится сразу же при создании узла (если определить структуру данных узла `Node`) - если для узла не назначить связь со следующим узлом, то по-дефолту будет считаться, что это конечный узел списка, так как он будет указывать на `NULL`. 

Преимущества связанного списка:
1. Динамическая структура данных. Загрузка и освобождение памяти происходит динамически во время операций вставки и удаления из связанного листа. 
2. Прост в добавлении или удалении данных. При вставке данных нет нужды в сдвиге прочих элементов списка (в отличии от обычного списка). Необходимо лишь обновить связи. 
3. Эффективное использование памяти. Нет нужды в переизбытке памяти, так как память заполняется и высвобождается динамически. 
4. Многие продвинутые структуры данных могут базироваться на связанном списке. 

Связанные списки бывают:
1. [[Односвязный список Singly Linked List]]
2. [[Двусвязный список Doubly Linked List]]
3. [[Кольцевой (замкнутый) связанный список Circular Linked List]]

___
Зачем это нужно: [[]] 
Примеры применения: [[Singly Linked List Python]] 
Основные ошибки: [[]] 
___
Relations: [[Структуры данных]] 
Tags: #practice 
References: 
Query: List Node, Linked List, связанный список