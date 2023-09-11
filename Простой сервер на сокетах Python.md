Сервер можно построить на сокетах в Python. Для этого необходимо:
1. Создать TCP-сервер
2. Задать адрес сокета
3. Поставить сокет прослушивать запросы на подключение
4. Дождаться подключения и выделить клиенту подключение
5. Выполнять серверный код

Приведенное ниже решение отличается тем, что может поддерживать только одного клиента. 

___
```
import socket

# Создание TCP-сервера
server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

# Установка адреса сокета
server.bind(('127.0.0.1', 8000))
server.listen()

# Серверный код
try:
	# Прослушивание подключений
	connection, client_address = server.accept()

	buffer = b''
	while buffer[-2:] != b'\r\n':
		data = connection.recv(2)
		if not data:
			break
		else:
			buffer = buffer + data

	connection.sendall(buffer)
	connection.sendall(str.encode('Ваше подключение закрыто.'))

except Exception as ex:
	pass

finally:
	connection.close()

```
___
Relations: [[Python Hints]] 
Tags: #code
References: [[Asyncio (книга)]] 
Query: 