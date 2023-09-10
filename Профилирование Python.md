Профилирование нагрузки ЦП при вызове определенной функции можно реализовать с помощью:
1. `time.time()` - обычный вывод времени выполнения функции
2. `cProfile` - более подробный вывод работы функции
3. `line_profiler` - построчное профилирование функции

___
```
>> bash
python3 -m pip install psutil, line_profiler, memory_profiler

>> programm.py
from line_profiler import profile as cpu_profile
from memory_profiler import profile as mem_profile

@mem_profile
@cpu_profile
def func(*args):
	...
	return ...

if __name__ == '__main__':
	args = ...
	func(*args)

>> bash
kernprof -l -v programm.py // for cpu profile
python3 -m programm.py // for memory profile

```
___
Relations: [[Python]] 
Tags: #code
References: [[Высокопроизводительные Python-приложения]] 
Query: 