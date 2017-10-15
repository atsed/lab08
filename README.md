[![Build Status](https://travis-ci.org/GolubDobra/lab07.svg?branch=master)](https://travis-ci.org/GolubDobra/lab07)
## Laboratory work VII

Данная лабораторная работа посвещена изучению систем документирования исходного кода на примере **Doxygen**

```ShellSession
$ open https://www.stack.nl/~dimitri/doxygen/manual/index.html
```

## Tasks

- [X] 1. Создать публичный репозиторий с названием **lab07** на сервисе **GitHub**
- [X] 2. Выполнить инструкцию учебного материала
- [X] 3. Ознакомиться со ссылками учебного материала
- [X] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial
Делаем первоначальные настройки, добавляя значения переменным окружения
```ShellSession
$ export GITHUB_USERNAME=<имя_пользователя>      #Устанавливаем значение переменной окружения GITHUB_USERNAME
$ alias edit=atom				#Создаем алиас на редактирование файлов одним из редакторов(atom)
```
#### Делаем первоначальные настройки для соединения с репозиторием 
```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab06 lab07
$ cd lab07
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab07
```
#### Работа с Doxygen
```ShellSession
#Создаем каталог docs
$ mkdir docs
#Создаем файл doxygen.conf
$ doxygen -g docs/doxygen.conf
#Редактирование файла doxygen.conf
$ cat docs/doxygen.conf
```
#### Работа с файлом doxygen.conf
```ShellSession
$ sed -i '' 's/\(PROJECT_NAME.*=\).*$/\1 print/g' docs/doxygen.conf	#Добавление информации в документацию doxygen.conf
$ sed -i '' 's/\(EXAMPLE_PATH.*=\).*$/\1 examples/g' docs/doxygen.conf
$ sed -i '' 's/\(INCLUDE_PATH.*=\).*$/\1 examples/g' docs/doxygen.conf
$ sed -i '' 's/\(INPUT *=\).*$/\1 README.md include/g' docs/doxygen.conf
$ sed -i '' 's/\(USE_MDFILE_AS_MAINPAGE.*=\).*$/\1 README.md/g' docs/doxygen.conf
$ sed -i '' 's/\(OUTPUT_DIRECTORY.*=\).*$/\1 docs/g' docs/doxygen.conf
```
#### Редактирование README.md
```ShellSession
$ sed -i '' 's/lab06/lab07/g' README.md
```
#### Документирование и редактирование print.hpp
```ShellSession
# документируем функции print
#Редактируем файл print.hpp
$ edit include/print.hpp
```
#### Выполняем команды для настройки локального репозитория для дальйшей отправки
```ShellSession
$ git add .
$ git commit -m"added doxygen.conf"		#Создаем коммит
$ git push origin master			#Выгружаем локальный репозиторий в удаленный репозиторий
```
#### Работа с Travis
```ShellSession
$ travis login --auto
$ travis enable
```
#### Создание базового файла документации, перемещение файлов, отправление изменений на удаленный репозиторий 
```ShellSession
$ doxygen docs/doxygen.conf		#Перемещение и удаление файлов
$ ls | grep "[^docs]" | xargs rm -rf
$ mv docs/html/* . && rm -rf docs
$ git checkout -b gh-pages		#Перемещаемся на ветку gh-pages
$ git add .
$ git commit -m"added documentation"	#Создаем коммит
$ git push origin gh-pages		#Выгружаем локальный репозиторий в удаленный репозиторий 
$ git checkout master			#Перемещаемся на ветку master
```
#### Работа с лабораторной и drive.google
```ShellSession
$ mkdir artifacts && cd artifacts		#Создаем каталог artifacts и перемещаемся в него
$ screencapture -T 10 screenshot.jpg # или png	#Делаем снимок экрана и помещаем его в каталог artifacts
<Command>-T
$ open https://${GITHUB_USERNAME}.github.io/lab07/print_8hpp_source.html	#Открываем https://${GITHUB_USERNAME}.github.io/lab07/print_8hpp_source.html
$ gdrive upload screenshot.jpg # или png	#Выгружаем сделанный снимок экрана
$ SCREENSHOT_ID=`gdrive list | grep screenshot | awk '{ print $1; }'`	#Задаем значения переменной SCREENSHOT_ID
$ gdrive share ${SCREENSHOT_ID} --role reader --type user --email rusdevops@gmail.com	#Даем права на просмотр для пользователя rusdevops@gmail.com
$ echo https://drive.google.com/open?id=${SCREENSHOT_ID}	#Выводим ссылку на наше изображение
#https://drive.google.com/open?id=0Bw35q7PD8b3lUTBNbXM2YkJWaGM
```

## Report

```ShellSession
$ cd ~/workspace/labs/
$ export LAB_NUMBER=07
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [HTML](https://ru.wikipedia.org/wiki/HTML)
- [LAΤΕΧ](https://ru.wikipedia.org/wiki/LaTeX)
- [man](https://ru.wikipedia.org/wiki/Man_(%D0%BA%D0%BE%D0%BC%D0%B0%D0%BD%D0%B4%D0%B0_Unix))
- [CHM](https://ru.wikipedia.org/wiki/HTMLHelp)
- [PostScript](https://ru.wikipedia.org/wiki/PostScript)

```
Copyright (c) 2017 Братья Вершинины
```
