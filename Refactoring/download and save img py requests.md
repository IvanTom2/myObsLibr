___
Данный метод позволяет скачать картинку из интернета с помощью HTTP запросов в python, а также сохранить ее в указанной директории. 
___
[[Code]]:
```
img = requests.get(f"{URL}")
with open(f"{path}.jpg", "wb") as to_save:
	to_save.write(img.content)
```
___
Language: [[Python]]
Key-words: [[Download]] [[Image]] 
Question query?: Как скачать и сохранить картинку python