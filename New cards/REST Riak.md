Riak поддерживает REST API, то есть содержит в себе сервер, в спомощью которого можно запрашивать данные по сети через протокол HTTP. 

Соответственно, Riak поддерживает операции CRUD: `POST`, `GET`, `PUT`, `DELETE`. 

Чтобы добавить запись в Riak, можно воспользоваться утилитой `cUrl`. При добавлении новой записи в новый сегмент, последний автоматически создается. 

Также Riak способен генерировать ключи. Если мы попытаемся добавить новую запись в сегмент без указания ключа, то Riak создаст ключ, добавит запись и в ответ на запрос вернет значение ключа. Ключ вернется, если отправить запрос через метод `POST`. 
Сгенерированный ключ вернется в заголовке запроса `Location`. 

Если забыл ключи, то их можно найти, обратившись к сегменту. 

Также для записи можно устанавливать [[Ссылки в Riak]] и метаданные. Метаданные обычно не выводятся, их нужно запрашивать со специальным флагом `-I` (вообще с данным флагом всегда выводятся различные метаданные). Метаданные добавляются с помощью обычно записи, но с добавлением `X-Riak-Meta-{name}`. 

___
```
// добавление записи в виде HTML
curl -v -X PUT http://localhost:8098/riak/favs/db -H "Content-Type: text/html" -d "html><body><h1>My new favorite DB is RIAK</h1></body></html>"

// добавление записи в виде JSON
curl -v -X PUT http://localhost:8098/riak/animals/ace -H "Content-Type:application/json" -d '{"nickname":"The Wonder Dog", "breed":"German Shepherd"}'

// добавление записи с возвратом записанного значения
curl -v -X PUT http://localhost:8098/riak/animals/polly?returnbody=true -H "Content-Type: application/json" -d '{"nickname" : "Sweet Polly Purebred", "breed" : "Purebred"}'

// добавление записи с автоматической генерацией ключа
curl -i -X POST http://localhost:8098/riak/animals \
-H “Content-Type: application/json” \
-d ‘{“nickname” : “Sergeant Stubby”, “breed” : “Terrier”}’

// удаление записи по ключу
curl -i -X DELETE http://localhost:8098/riak/animals/JeMYlwAvHvFi49nPF2OZL6tQ1Dy

// получение всех ключей сегмента
curl http://localhost:8098/riak/animals?keys=true

// установка ссылки и мета-данных
curl -X PUT http://localhost:8091/riak/cages/1 \
-H "Content-Type: application/json" \
-H "X-Riak-Meta-Color: Pink" \
-H "Link: </riak/animals/polly>; riaktag=\"contains\"" \
-d '{"room" : 101}'

```
___
Зачем это нужно: [[]] 
Примеры применения: [[]] 
Основные ошибки: [[]]
___
Relations: [[Riak]] 
Tags: #code 
References: [[Семь баз данных]] 
Query: 