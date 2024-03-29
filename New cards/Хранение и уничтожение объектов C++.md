В C++ объекты разработчик может решить, где именно хранить объекты (для повышения эффективности программы):
1. ***Стек***. Это область памяти, которая напрямую используется процессором во время выполнения программы. Переменные, храняющиеся в стеке, называются ***автоматическими***. 
2. ***Статическая память***. Это фиксированный блок памяти, выделяемый перед началом выполнения программы. 
3. ***Куча***. Область памяти для динамического создания объектов. 

При хранении данных в стеке или статической памяти на первое место ставится скорость выделения и освобождения памяти, очень существенная в некоторых ситуациях. Однако за нее приходится расплачиваться гибкостью, так как программист должен точно знать количество, жизненный цикл и тип объекта во время написания программы. 

Существует альтернатива стеку и статической памяти. Объекты могут быть созданы динамически в специальном пуле памяти, называемом ***кучей***. В этом случае программист до момента выполнения программы точно не знает, сколько объектов потребуется, каким будет их жизненный цикл или фактический тип. Эти решения принимаются "на месте" во время выполнения программы. Если программисту понадобится новый объект, то он просто создает его в куче с помощью ключевого слова `new`. Когда объект становится ненужным, он удалит его с помощью `delete`. 

Операции с кучей происходят динамически, поэтому выделение памяти происходит гораздо дольше, чем в стеке. Кроме того, если объект создается в стеке или в статической памяти, то ***продолжительность жизни*** объекта определяется компилятором (компилятор уничтожает созданный объект). Но если объект создается в куче, то компилятор не располагает информацией о сроке его существования. В C++ программист сам должен уничтожить объект в своей программе с помощью ключевого слова `delete`. 

Есть альтернатива - ***уборщик мусора***. На самом деле, по стандарту в C++ не включен уборщик мусора, потому что он накладывает дополнительные затраты ресурсов. Но сторонние производители ПО предоставляют возможность использовать уборщик мусора в C++. 

___
```

```
___
Зачем это нужно: [[]] 
Примеры применения: [[]] 
Основные ошибки: [[]]
___
Relations: [[C++]] 
Tags: #code
References: [[Философия C++ Эккель]] 
Query: 