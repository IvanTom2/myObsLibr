Фикстуры - это подготовка окружения для теста. Реализуются через декоратор `@pytest.fixture`. 

В Pytest нет аргументов у функций. Чтобы использовать аргументы у функций, необходимо использовать фикстуры, иначе никак. 

Фикстуры могут быть размещены как методы классов, так и в качестве функций. Классовые фикстуры работают только в класс. Фикстуры-функции работают везде (в рамках файла). 

Чтобы импортировать фикстуры, их нужно добавить в специальный файл `conftest.py`. Для использования их не нужно явно импортировать - они будут доступны и без этого. 

___
```
import pytest


class Fruit:
    def __init__(self, name):
        self.name = name

    def __eq__(self, other):
        return self.name == other.name


@pytest.fixture
def my_fruit():
    return Fruit("apple")


@pytest.fixture
def fruit_basket(my_fruit):
    return [Fruit("banana"), my_fruit]


def test_my_fruit_in_basket(my_fruit, fruit_basket):
    assert my_fruit in fruit_basket
```
___
Зачем это нужно: [[]] 
Примеры применения: [[]] 
Основные ошибки: [[]]
___
Relations: [[Python Pytest]] [[Pytest Fixture]] 
Tags: #code
References: 
Query: 