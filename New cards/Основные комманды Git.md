
![[Pasted image 20231125202647.png]]
___
```
git init # инициализация репозитория
git clone # клонирование репозитория

git status # просмотр состояния файлов
git diff # просмотр изменений в файлах
git diff --staged # просмотр изменений в стейджах

git add # застейджить изменения в файле
git add -A # застейджить изменения во всех файлах

git commit # коммит
git commit -m "text" # коммит с фаст-комментом
git commit -a # эквивалент git add -A + commit

git rm file # удаление файла стейджа
git rm file --cached # удаление файла из всего индекса
git rm *.log # удаление по шаблону

git mv file # переименование/перемещение файла (хз зачем)

git log # просмотр истории коммитов
git log -p # просмотр патчей (истории изменений)
git log -p -2 # ограничение 2-мя последними записями
git log --stat # статистика изменений в коммитах
git log --pretty # вывод лога в другом формате
git log --pretty=format:"%h - %an, %ar : %s"

git commit --amend # для внесения изменений в последний коммит
git reset HEAD file # для удаления из стейджа
git checkout -- file # удаляет изменения в файле (полностью удаляет изменения)

git restore --staged file # убирает файл из стейджа
git restore file # удаляет изменения в файле (полностью удаляет изменения)

git remote add NAME URL # добавление удаленного репозитория
git remote fetch NAME # получить изменения с репозитория,                              # создается новая ветка
git remote pull NAME # получить изменения со слиянием
git remote show NAME # информация о репозитории
git remote rename NAME NAME1 # переименовать
git remote remove NAME # удалить ссылку на репозиторий

git tag v1.4-lw # легковесный тег
git tag -a v1.4 -m "Version 1.4" # аннотированный тег
git push NAME v1.4 # пуш тега (теги не передаются с коммитами)
git push NAME --tage # пуш всех тегов
git push NAME --follow-tags # пуш аннотированных тегов
git tag -d v1.4 # удаление тега (локально)
git push NAME --delete v1.4 # пуш удаления тега

git checkout v1.4 # переход на версию тега


```
___
Зачем это нужно: [[]] 
Примеры применения: [[]] 
Основные ошибки: [[]]
___
Relations: [[Git]] 
Tags: #code
References: [[Pro Git]] 
Query: 