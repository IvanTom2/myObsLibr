Определение класса начинается с ключевого слова `struct ClassName {data}`. Тело класса может содержать данные или быть пустым. Тело класса также формирует новую область видимости. 

Также класс можно определять с помощью ключевого слова `class`. 

После определения класса, точнее после определения тела класса, необходимо ставить `;`. 

___
```
#include <string>
#include <iostream>

struct Sales_data
{
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

int main()
{
    Sales_data book1, book2;

    book1.bookNo = "abc1";
    book2.bookNo = "abc2";

    std::cout << book1.bookNo << std::endl;
    std::cout << book2.bookNo << std::endl;

    return 0;
}

```
___
Зачем это нужно: [[]] 
Примеры применения: [[]] 
Основные ошибки: [[]]
___
Relations: [[C++]] 
Tags: #code
References: 
Query: 