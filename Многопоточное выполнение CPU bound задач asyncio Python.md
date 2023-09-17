Многопоточное выполнение CPU-bound задач в общем случае не самая лучшая идея. Тем не менее, бывают ситуации, когда многопоточное выполнение счетных задач можно ***значительно*** повысить производительность. 



___
```
import asyncio
import functools
import hashlib
import os
from concurrent.futures.thread import ThreadPoolExecutor
import random
import string
import time


def random_password(length: int) -> bytes:
    ascii_lowercase = string.ascii_lowercase.encode()
    return b"".join(bytes(random.choice(ascii_lowercase)) for _ in range(length))


def hash(password: bytes) -> str:
    salt = os.urandom(16)
    return str(hashlib.scrypt(password, salt=salt, n=2048, p=1, r=8))


async def main():
    loop = asyncio.get_running_loop()
    tasks = []

    with ThreadPoolExecutor() as pool:
        for password in passwords:
            tasks.append(loop.run_in_executor(pool, functools.partial(hash, password)))

    await asyncio.gather(*tasks)


if __name__ == "__main__":
    passwords = [random_password(10) for _ in range(10000)]

    start = time.time()
    asyncio.run(main())
    print(time.time() - start)

```
___

Relations: [[asyncio Python]] 
Tags: #code
References: [[Asyncio (книга)]] 
Query: 