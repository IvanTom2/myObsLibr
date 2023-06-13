___
Написав приложение, например, веб-сервер, стоит задуматься о том, как поступать в случае, если мы его хотим отключить и остаются необработанные запросы. Лучше всего в данной ситуации закончить обрабатывать запросы. 

Для реализации специальной логики остановки приложения можно использовать сигналы ОС (библиотека signal в python). Например, SIGINT - сигнал прерывания через остановку CTRL+C или SIGTERM - при вызове kill снятия процесса.  

Для добавления обработчика сигналов на уровне цикла событий, необходимо использовать метода add_signal_handler. Он безопасно взаимодействует с циклом событий, и, соответственно, обрабатывает сигналы.  
___
[[Code]]:
```
import asyncio
import signal

def cancel_tasks():
	print('Получен сигнал SIGINT!')
	tasks: Set[asyncio.Task] = asyncio.all_tasks()
	print(f'Снимается {len(tasks)} задач.')
	[task.cancel() for task in tasks]
	
loop.add_signal_handler(signal.SIGINT, cancel_tasks)
```
___
Language: [[Python]]
Key-words:  [[Asyncio]] [[asyncio python]]
Question query?: 