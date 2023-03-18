# Git

## Default git

- `git help <command>`: help по командам
- `git init`: создаём git репозиторий
- `git status`: что происходит
- `git add <blob/tree>`: добавляем blob/tree
- `git commit`: создаём новый коммит
    - Пишем [хорошее сообщение коммита](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)!
- `git log`: смотрим на то, что происходило
- `git log --all --graph --decorate`: красиво смотрим на то, что происходило
- `git diff <blob/tree>`: изменения blob/tree mdash; staging vs HEAD
- `git diff <revision> <blob/tree>`:  изменения blob/tree &mdash; revision vs HEAD
- `git checkout <revision>`: обновляем HEAD на revision

## Ветки и слияние веток

`git branch <branch name>` создаем
`git checkout -b <branch name>` сразу создаём и переходим

`git merge` сливаем ветки и резолвим конфликты:

- `git branch`: посмотреть все ветки
- `git branch <name>`: создаём ветку
- `git checkout -b <name>`: сразу создаём и переходим
    - тоже самое, что и `git branch <name>; git checkout <name>`
- `git merge <revision>`: сливаем ветку с текущей
- `git mergetool`: резолвим конфликты красиво
- `git rebase`: переназначаем часть графа на другой родитель, множество опций,
см. лекцию
  - `git rebase -i`: ну хоть как-то понятно перезначаем часть графа на другой
  родитель

## Храним на сервере

- `git remote`: все серверы, которые хранят нашу копию
- `git remote add <name> <url>`: добавляем то, где мы будем хранить
- `git push <remote> <local branch>:<remote branch>`: посылаем объекты на remote и обновляем remote ветку
- `git branch --set-upstream-to=<remote>/<remote branch>`: ставим соответствие между local и remote (github за Вас это делаем сам)
- `git fetch`: забираем изменения из remote
- `git pull`: забираем и сразу делаем merge, эквивалентно `git fetch; git merge`
- `git clone`: клонируем себе репозиторий

## Undo

- `git commit --amend`: обновляем последний коммит
- `git reset HEAD <blob/tree>`: убираем blob/tree из staging
- `git checkout -- <blob/tree>`: убираем все локальные изменения с blob/tree

# Advanced Git

- `git config`: git [кастомизируется](https://git-scm.com/docs/git-config)
- `git clone --depth=1`: скачиваем репозиторий и не храним всю историю, полезно для больших репозиториев
- `git add -p`: интерактивное добавление, например, почанково
- `git rebase -i`: делаем move в графе красиво
  - Обратите внимание! Иногда с Вас будут требовать сделать squash коммитов, например, Вы сделали очень много изменений мелких,
    а финально будет красиво иметь только один коммит, стоит сделать
    `git rebase -i d86f2765c..d6c5244c9` и поставить напротив всех коммитов,
    кроме первого/последнего метку squash
- `git blame`: кто что поменял в blob/tree
- `git stash`: убрать все локальные изменения и сохранить для будущего использования
  - `git stash pop`: вернуть все изменения, которые были сохранениы
- `git bisect`: бинарный поиск по коммитам, например, чтобы найти виновных в
  сломанном тесте
- `.gitignore`: файл для блокировки
  каких-то расширений, которые не надо коммитить, например, сборка бинарей или
  библиотек

