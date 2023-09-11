Данное решение отличается от [[Простой сервер на сокетах Python]] тем, что может поддерживать сразу несколько клиентов. 

Его минусы в том, что происходит слишком много итераций в цикле `while`, а также большое количество обработок исключений почти на каждой итерации каждого цикла. 

___
```
import socket

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

server_address = ("127.0.0.1", 8000)
server.bind(server_address)

server.listen()
server.setblocking(False)

connections: list[socket.socket] = []

try:
    while True:
        try:
            connection, client_address = server_socket.accept()
            connection.setblocking(False)
            connections.append(connection)
            print(f"Получен запрос на подключение от {client_address}!")

        except BlockingIOError:
            pass

        for conn in connections:
            try:
                buffer = b""

                while buffer[-2:] != b"\r\n":
                    data = conn.recv(2)

                    if not data:
                        break

                    else:
                        buffer = buffer + data
                conn.send(buffer)

            except BlockingIOError:
                pass

except Exception as ex:
    print(ex)

finally:
    server.close()

```
___
Relations: [[Python Hints]] [[Простой сервер на сокетах Python]] 
Tags: #code
References: [[Asyncio (книга)]] 
Query: 