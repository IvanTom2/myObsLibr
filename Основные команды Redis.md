
___
```
SET key value // добавить ключ-значение
GET key // извлечь значение по ключу
INCR key // икрементация (добавление единицы) к значению (только числа, а то баг)

MULTI // открывает транзакцию (либо выполнены все операции, либо ни одна)
EXEC // закрывает и выполняет транзакцию
DISCARD // прерывает транзакцию - транзакция не начинает выполняться

MSET key value key value // добавление нескольких значений
MGET key key // извлечение нескольких значений
HVALS hash // извлечение всех значение (например из хеша - HVALS)

MSET h1:h2:h3 value_name value // пример добавления хеша

RPUSH list elements // добавление в спискок справа
LPUSH list elements// добавление в список слева
LRANGE list start stop // получение части списка по позиции
LREM list count value // удалить из списка значение count - кол-во удалений

RPOPLPUSH list1 list2 // удаление с конца списка1 и добавление в список2
BRPOP key timeout // дожидается значения в течение timeout

SADD key elements // добавить (создать) множество и его элементы
SMEMBERS key // извлечь элементы множества
SINTER keys // пересечение множеств
SDIFF keys // разность множеств
SUNION keys // объединение множеств
SUNIONSTORE key keys // объединение множеств и сохранение их под key
SMOVE // перемещение значения из множества1 в множество2
SCARD // кол-во элементов во множестве
SPOP // извлечение случайного элемента
SREM key values // удаление конкретных элементов со значение value

ZADD pos value // добавляет элемент в отсортированное множество 
ZINCRBY key pos value // изменяет оценку элемента во множестве (отсорт.)
ZRANGE start end // извлечение элементов списка с наибольшим значением (если +)
ZRANGE start end WITHSCORES // такое же, только извлечение элементов с оценками
ZRANGEBYSCORE start end // извлечение эл. по оценке между start и end
ZRANGEBYSCORE inf -inf // извлечение всех элементов по оценке
ZUNIONSTORE key int keys // объед. и сохр. int элементов из keys-множеств

EXPIRE key seconds // добавления времени хранения для ключа (когда ключ создан)
EXISTS key // проверка, существует ли ключ
SETEX key seconds value // добавление ключа и его времени хранения в 1 команду
TTL key // остаток времени хранения ключа
PERSIST key // убрать время хранения для ключа (оставить его)
EXPIREAT key // время хранения ключа в абсолютном выражении (т.е. ДО UNIX)

SELECT db_int // переключение между базами данных (пронстранством имен)
MOVE key db_int // перемещение ключе между базами данных
```

___
Relations: [[Redis]] 
Tags: #code
References: [[Семь баз данных]] 
Query: 