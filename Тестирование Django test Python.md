Чтобы прописать тесты для Django, необходимо создать файл test.py в директории, где размещен проект. 

___
```
from django.test import TestCase

class SmokeTest(TestCase):
	def test_bad_maths(self):
		self.assertEqual(1 + 1, 3)

# Запуск тестов в Django -> через терминал Bash
# Для начала нужно перейти в директорию проекта
python3 -m manage test
```
___
Relations: [[Django Python]] 
Tags: #code
References: 
Query: Модульные тесты в Django, тестирование в Django