___
Скачивание файлов любого типа через telegram (телеграм) api. Ограничение скачиваемого файла составляет 20 мб.
Способ предполагает, что мы знаем айди файла, который хотим скачать - получаем его из обрабатываемого сообщения, либо из диалога в целом. 

Как скачать и сохранить файл в python [[download and save img py requests]]
___
[[Code]]:
```
URL = "https://api.telegram.org" # домен телеграма
TOKEN = 'bot55555:qwedfasdasdgqew' # токен
message = requests.get(...).json() # предпосылка к получению file_id

file_id = message["message"]["photo"][-1]["file_id"]
file_info = requests.get(f"{URL}/{TOKEN}/getFile?file_id={file_id}").json()
download_path = file_info["result"]["file_path"]

file = requests.get(f"{URL}/file/{TOKEN}/{download_path}")
```
___
Language: [[Python]]
Key-words:  [[Telegram]] [[Download file]] [[HTTP]]
Question query?:  Как скачать файл в telegram (телеграм)