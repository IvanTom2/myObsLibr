Для неблокирующего ввода данных необходима реализация потокового читателя. В приведенном ниже коде создается потоковый читатель, затем он передается в протокол. Далее вызывается сопрограмма `connect_read_pipe`, которой передается лямбда-фукция, являющаяся фабрикой заранее определенного класса (протокола), а также передается `sys.stdin`. 

___
```
import sys
import shutil
from asyncio import StreamReader
from collections import deque
from typing import Callable, Deque, Awaitable
import asyncio
import os
import tty


def save_cursor_position():
    sys.stdout.write("\0337")


def restore_cursor_position():
    sys.stdout.write("\0338")


def move_to_top_of_screen():
    sys.stdout.write("\033[H")


def delete_line():
    sys.stdout.write("\033[2K")


def clear_line():
    sys.stdout.write("\033[2K\033[0G")


def move_back_one_char():
    sys.stdout.write("\033[1D")


def move_to_bottom_of_screen() -> int:
    _, total_rows = shutil.get_terminal_size()
    input_row = total_rows - 1
    sys.stdout.write(f"\033[{input_row}E")
    return total_rows


async def create_stdin_reader() -> StreamReader:
    stream_reader = asyncio.StreamReader()
    protocol = asyncio.StreamReaderProtocol(stream_reader)
    loop = asyncio.get_running_loop()
    await loop.connect_read_pipe(lambda: protocol, sys.stdin)
    return stream_reader


class MessageStore(object):
    def __init__(
        self,
        callback: Callable[[Deque], Awaitable[None]],
        max_size: int,
    ) -> None:
        self._callback = callback
        self._deque = deque(maxlen=max_size)

    async def append(self, item):
        self._deque.append(item)
        await self._callback(self._deque)


async def read_line(stdin_reader: StreamReader) -> str:
    def erase_last_char():
        move_back_one_char()
        sys.stdout.write(" ")
        move_back_one_char()

    delete_char = b"\x7f"
    input_buffer = deque()

    while (input_char := await stdin_reader.read(1)) != b"\n":
        if input_char == delete_char:
            if len(input_buffer) > 0:
                input_buffer.pop()
                erase_last_char()
                sys.stdout.flush()
        else:
            input_buffer.append(input_char)
            sys.stdout.write(input_char.decode())
            sys.stdout.flush()

    clear_line()
    return b"".join(input_buffer).decode()


async def sleep(delay: int, message_store: MessageStore):
    await message_store.append(f"Начало задержки {delay}")
    await asyncio.sleep(delay)
    await message_store.append(f"Конец задержки {delay}")


async def main():
    tty.setcbreak(sys.stdin)
    os.system("clear")
    rows = move_to_bottom_of_screen()

    async def redraw_output(items: deque):
        save_cursor_position()
        move_to_top_of_screen()
        for item in items:
            delete_line()
            print(item)
        restore_cursor_position()

    messages = MessageStore(redraw_output, rows - 1)
    stdin_reader = await create_stdin_reader()

    while True:
        line = await read_line(stdin_reader)
        delay_time = int(line)
        asyncio.create_task(sleep(delay_time, messages))
        # pass


if __name__ == "__main__":
    asyncio.run(main())

```
___

Relations: [[Потоки данных asyncio Python]] 
Tags: #code
References: [[Asyncio (книга)]] 
Query: 