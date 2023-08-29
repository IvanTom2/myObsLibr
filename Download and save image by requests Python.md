Данный метод позволяет скачать картинку из интернета с помощью HTTP запросов в python, а также сохранить ее в указанной директории. 

___
```
img = requests.get(f"{URL}")
with open(f"{path}.jpg", "wb") as to_save:
	to_save.write(img.content)
```
___
Relations: [[Python Hints]] 
Tags: #code
References: 
Query: Как скачать и сохранить картинку python