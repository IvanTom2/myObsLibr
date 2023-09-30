В основе WSGI лежит простая Python-функция. В них нет места для асинхронных функций и ожидания `await`. Кроме того, WSGI поддерживает только жизненные циклы запрос-ответ и не будет работать с протоколами долговременных подключений, например, `WebSockets`. 

ASGI исправляет эту ситуацию путем использования сопрограмм. 

Представленные ниже реализации WSGI и ASGI являются низкоуровневыми. В работе обычно используются какие-либо специализированные каркасы. Один из каркасов ASGI называется `Starlette`

___
```
## -> wsgi_app.py
def wsgi_app(env, start_response):
    start_response("200 OK", [("Content-Type", "text/html")])
    return [b"WSGI hello!"]

## bash -> gunicorn wsgi_app:wsgi_app

## -> asgi_app.py
async def asgi_app(scope, receive, send):
    await send(
        {
            "type": "http.response.start",
            "status": 200,
            "headers": [[b"content-type", b"text/html"]],
        }
    )
    await send({"type": "http.response.body", "body": b"ASGI hello!"})

## bash -> uvicorn asgi_app:asgi_app

```
___

Relations: [[asyncio Python]] [[Python]] 
Tags: #code
References: [[Asyncio (книга)]]  
Query: 