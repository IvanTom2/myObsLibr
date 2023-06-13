___
Тестирование в Go включено в коробку и доступно из стандартной библиотеки. 
Для запуска процесса тестирования необходимо следующее:
1. Создать файл zzz_test.go где zzz - название файла
2. Создать функцию, которая начинается с Test
3. На входе в функцию должно быть определено только одно значение t \*testing.T
4. Для использования его необходимо импортировать "testing"

В Go есть инструменты для рассчета покрытия кода тестами. Для того, чтобы узнать покрытие тестами, необходимо запустить в консоли: 
go test -cover


___
[[Code]]:
```
package main

import "testing"

func TestFunc(t *testing.T) {
	result = func()
	expect = value

	if result != expect {
		t.Errorf("ERROR DESCRIPTION IN TEST")
	}
}
```
___
Language: [[GoLang]]
Key-words:  [[GoLang]]
Question query?: модульные тесты (unit tests) в Go (го)