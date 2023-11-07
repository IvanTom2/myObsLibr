Чтобы сохранить изображение в Riak, необходимо его поместить в какой-либо каталог (то есть явно иметь), и при добавлении указать соответствующие заголовки. 

Чтобы передать значение изображения, необходимо добавить к запросу флаг `--data-binary @img_path.jpg`. 

___
```
curl -X PUT http://localhost:8091/riak/photos/polly.jpg \
-H "Content-type: image/jpeg" \
-H "Link: </riak/animals/polly>; riaktag=\"photo\"" \
--data-binary @polly_image.jpg
```
___
Зачем это нужно: [[]] 
Примеры применения: [[]] 
Основные ошибки: [[]]
___
Relations: [[Riak]] [[Типы MIME в Riak]] 
Tags: #code
References: [[Семь баз данных]] 
Query: 