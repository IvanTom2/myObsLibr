___
Бенчмарк - это замер скорости исполнения программного кода. 
Бенчмарк выводит характеристики системы и скорость исполнения кода.

В ниже приведенном примере можно увидеть значение 102.8 ns/op - это означает скорость выполнения в 102.8 наносекунд для функции Repeate. 
10364270 - количество выполнений. Соответственно, время исполнения бенчмарка заняло 1.187 секунды. 

Стоит заметить, что по дефолту бенчмарк запускается последовательно. 
___
[[Code]]:
```
func Repeate(str string, rcount int) string {
	var repeated string
	for i:=0; i<rcount; i++ {
		repeated += str
	}
	return repeated
}

func BenchmarkRepeate(b *testing.B) {
	for i:=0; i<b.N; i++ {
		Repeat("b", 5)
	}
}

// запуск бенчмарка в консоли
go test -bench=.

// вывод результатов бенчмарка
goos: linux
goarch: amd64
pkg: iteration
cpu: Intel(R) Core(TM) i5-8300H CPU @ 2.30GHz
BenchmarkRepeate-8      10364270               102.8 ns/op
PASS
ok      iteration       1.187s

```
___
Language: [[GoLang]]
Key-words:  [[Тестирование в GoLang]]
Question query?: Бенчмарк (benchmark) в Go (го)