___
Баг возникает при запуске веб-ресурса на основе nginx-сервера. Проблема в том, что в Django 4.0 необходимо добавлять доверенности на домены, откуда идут запросы. А т.к. nginx делает переадресацию (судя по всему) на Django, он ему не доверяет или что-то в этом духе. 

Для решения в настройках проекта Django необходимо выставить CSRF_TRUSTED_ORIGINS параметр. Он является списком и ему нужно присвоить значения домена. 
___
[[Code]]:
```
My domain was superlists.ivan-tomilov.ru
>> in settings.py
CSRF_TRUSTED_ORIGINS = ['http://superlists.ivan-tomilov.ru',
						'https://superlists.ivan-tomilov.ru']
```
___
Language: [[Python]]
Key-words:  [[Bug]]
***Question query?***: 
***Bug text***: Forbidden (403) CSRF verification failed. Request aborted. Reason given for failure: Origin checking failed does not match any trusted origins