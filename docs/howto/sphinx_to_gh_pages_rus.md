---
tags:
  - git
  - sphinx
---

# Публикация sphinx-документации на GitHub Pages

Первоисточник доступен по [ссылке](https://daler.github.io/sphinxdoc-test/includeme.html).

## Настроить основной репозиторий

```
mkdir sphinxdoc-test
cd sphinxdoc-test
git init
touch README
git add README
git commit -m 'first commit'
git remote add origin git@github.com:___/sphinxdoc-test.git
git push origin master
```

## Создать sphinx-проект в основном репозитории

```
mkdir sphinxdoc-test/docs
cd docs
sphinx-quickstart
```

## Настроить папку для сборки

Для сборки сайта с документацией и дальнейшей публикацией на GitHub Pages
необходимо создать ещё одну отдельную папку и настроить её должным образом.

### Создать папку

```
cd ..
mkdir sphixdoc-test-docs
cd sphixdoc-test-docs
```

### Клонировать репозиторий в папку `html`

```
git clone git@github.com:___/sphinxdoc-test.git html
cd html
```

### Создть ветку для публикации

```
git branch gh-pages
```

### Настроить git для публикации

```
git symbolic-ref HEAD refs/heads/gh-pages  # auto-switches branches to gh-pages
rm .git/index
git clean -fdx
```

`symbolic-ref` - создаёт или обновляет символьную ссылку, переключая указатель
на указанную в аргументах ветку.

`git clean -fdx` - полностью очищает текущую папку.

## Изменить `Makefile`

В скрипте сборки необходимо изменить значение параметра `BUILDDIR`, указав путь
к ранее созданной папке для публикации.

```
BUILDDIR=../../sphinx-test-docs
```

Для упрощения процесса публикации изменений, в `Makefile` добавьте шаг отправки
данных в репозиторий:

```
cd $(BUILDDIR)/html
git add .
git commit -m "rebuilt docs"
git push origin gh-pages
```

## Добавить `.nojekyll` файл

Файл `.nojekyll` необходимо добавить в репозиторий для того, чтобы стандартные
парсеры github игнорировали страницы, сгенерированные `sphinx`:

```
cd sphinxdoc-test-docs/html
touch .nojekyll
git add .nojekyll
git commit -m "added .nojekyll"
```

## Настройка на другой машине

При необходимости воспроизвести рабочую среду на другой машине необходимо:

1. Клонировать основной репозиторий:

```
git clone git@github.com:___/sphinxdoc-test.git
```

2. Настроить папку для сборки документации:

```
mkdir sphinxdoc-test-docs
cd sphinxdoc-test-docs
git clone git@github.com:___/sphinxdoc-test.git html
```

3. Добавить локальную ветку для отслеживания ветки `gh-pages`:

```
git checkout -b gh-pages remotes/origin/gh-pages
```
