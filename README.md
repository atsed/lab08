[![Build Status](https://travis-ci.org/desta-study/lab08.svg?branch=master)](https://travis-ci.org/desta-study/lab08)
## Laboratory work VIII
Данная лабораторная работа посвящена изучению средств пакетирования на примере **CPack**

```ShellSession
$ open https://cmake.org/Wiki/CMake:CPackPackageGenerators
```

## Tasks

- [X] 1. Создать публичный репозиторий с названием **lab08** на сервисе **GitHub**
- [X] 2. Выполнить инструкцию учебного материала
- [X] 3. Ознакомиться со ссылками учебного материала
- [X] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial
#### Делаем первоначальные настройки, добавляя значения переменным окружения
```ShellSession
$ export GITHUB_USERNAME=desta-study     #Устанавливаем значение переменной окружения GITHUB_USERNAME
$ export GITHUB_EMAIL=masterenko1to@mail.ru    #Устанавливаем значение переменной окружения GITHUB_EMAIL
$ alias edit=subl   #Создаем алиас на редактирование файлов одним из редакторов
$ alias gsed=sed # for *-nix system   #Создаем алиас на команду sed
```
#### Проводим первоначальные настройки для соединения с репозиторием восьмой лабораторной работы
```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab07 lab08   #Клонирование удаленного репозитория седьмой лабораторной в локальный каталог восьмой лабораторной
$ cd lab08    #Меняем директорию на lab08
$ git remote remove origin    #Отключаемся от удаленного репозитория седьмой лабораторной
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab08   #Подключаемся к удаленному репозиторию 8 лабораторной
```
#### Внесение изменений в CMakeLists.txt
```ShellSession
$ gsed -i '/project(print)/a\   #Здесь вносим данные о версии проекта print
set(PRINT_VERSION_STRING "v${PRINT_VERSION}")
' CMakeLists.txt
$ gsed -i '/project(print)/a\
set(PRINT_VERSION \
\${PRINT_VERSION_MAJOR}.\${PRINT_VERSION_MINOR}.\${PRINT_VERSION_PATCH}.\${PRINT_VERSION_TWEAK})
' CMakeLists.txt
$ gsed -i '/project(print)/a\
set(PRINT_VERSION_TWEAK 0)
' CMakeLists.txt
$ gsed -i '/project(print)/a\
set(PRINT_VERSION_PATCH 0)
' CMakeLists.txt
$ gsed -i '/project(print)/a\
set(PRINT_VERSION_MINOR 1)
' CMakeLists.txt
$ gsed -i '/project(print)/a\
set(PRINT_VERSION_MAJOR 0)
' CMakeLists.txt
```
#### Работа с DESCRIPTION и ChangeLog.md
```ShellSession
$ touch DESCRIPTION && edit DESCRIPTION     #Создание DESCRIPTION и его редактирование
$ touch ChangeLog.md      #Создание файла ChangeLog.md
$ DATE=`date` cat > ChangeLog.md <<EOF      #Внесение изменений в файл Changelog.md
* ${DATE} ${GITHUB_USERNAME} <${GITHUB_EMAIL}> 0.1.0.0
- Initial RPM release
EOF
```
#### Работа с CPackConfig.cmake
```ShellSession
$ cat > CPackConfig.cmake <<EOF
include(InstallRequiredSystemLibraries)
EOF
```

```ShellSession
$ cat >> CPackConfig.cmake <<EOF    #Внесение параметров версий, связи пакета с почтой и описание пакета
set(CPACK_PACKAGE_CONTACT ${GITHUB_EMAIL})
set(CPACK_PACKAGE_VERSION_MAJOR \${PRINT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR \${PRINT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH \${PRINT_VERSION_PATCH})
set(CPACK_PACKAGE_VERSION_TWEAK \${PRINT_VERSION_TWEAK})
set(CPACK_PACKAGE_VERSION \${PRINT_VERSION})
set(CPACK_PACKAGE_DESCRIPTION_FILE \${CMAKE_CURRENT_SOURCE_DIR}/DESCRIPTION)
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "static c++ library for printing")
EOF
```

```ShellSession
$ cat >> CPackConfig.cmake <<EOF    #Привязываем лицензию и README.md к Cpack

set(CPACK_RESOURCE_FILE_LICENSE \${CMAKE_CURRENT_SOURCE_DIR}/LICENSE)
set(CPACK_RESOURCE_FILE_README \${CMAKE_CURRENT_SOURCE_DIR}/README.md)
EOF
```

```ShellSession
$ cat >> CPackConfig.cmake <<EOF      #Устанавливаем параметры для CPACK_RPM_PACKAGE
set(CPACK_RPM_PACKAGE_NAME "print-devel")       #Задание имени пакета
set(CPACK_RPM_PACKAGE_LICENSE "MIT")        #Задаем CPACK_RPM_PACKAGE_LICENSE
set(CPACK_RPM_PACKAGE_GROUP "print")        #Задаем CPACK_RPM_PACKAGE_GROUP
set(CPACK_RPM_PACKAGE_URL "https://github.com/${GITHUB_USERNAME}/lab07")      #Ссылка пакета
set(CPACK_RPM_CHANGELOG_FILE \${CMAKE_CURRENT_SOURCE_DIR}/ChangeLog.md)  #Делаем привязку ChangeLog.md с CHANGELOG_FILE пакета
set(CPACK_RPM_PACKAGE_RELEASE 1)
EOF
```

```ShellSession
$ cat >> CPackConfig.cmake <<EOF      #Устанавливаем параметры для CPACK_DEBIAN_PACKAGE
set(CPACK_DEBIAN_PACKAGE_NAME "libprint-dev")   #Задание имени пакета
set(CPACK_DEBIAN_PACKAGE_HOMEPAGE "https://${GITHUB_USERNAME}.github.io/lab07")   #Ссылка на страницу пакета
set(CPACK_DEBIAN_PACKAGE_PREDEPENDS "cmake >= 3.0")
set(CPACK_DEBIAN_PACKAGE_RELEASE 1)
EOF
```

```ShellSession
$ cat >> CPackConfig.cmake <<EOF      #Подключение CPack

include(CPack)
EOF
```
#### Работа с CMakeLists.txt
```ShellSession
$ cat >> CMakeLists.txt <<EOF

include(CPackConfig.cmake)
EOF
```
#### Работа с README.md
```ShellSession
$ gsed -i 's/lab07/lab08/g' README.md    #Внесение изменений в файл README.md
```
#### Выполняем команды для настройки локального репозитория для дальнейшей отправки в удаленный репозиторий восьмой лабораторной работы
```ShellSession
$ git add .     #Добавляем все отредактированные файлы в подтвержденные
$ git commit -m"added cpack config"       #Создаем коммит с сообщением
$ git push origin master            #Выгружаем локальный репозиторий в удаленный репозиторий восьмой лабораторной
```
#### Работа с Travis
```ShellSession
$ travis login --auto       #Авторизуемся своим GITHUB аккаунтом
$ travis enable         #Включаем репозиторий в Travis
```
#### Работа с CMake
```ShellSession
$ cmake -H. -B_build
$ cmake --build _build        #создаем бинарное дерево проекта
$ cd _build       #Меняем директорию на _build
$ cpack -G "TGZ"          #Создание директорий проекта при помощи TGZ,RPM,DEB,NSIS,DragNDrop
$ cpack -G "RPM"
$ cpack -G "DEB"
$ cpack -G "NSIS"
$ cpack -G "DragNDrop"
$ cd ..                     #Выход из данной директории
```

```ShellSession

$ cmake -H. -B_build -DCPACK_GENERATOR="TGZ"
$ cmake --build _build --target package
```

```ShellSession
$ mkdir artifacts           #Создание каталога artifacts
$ mv _build/*.tar.gz artifacts        #Перемещение проектов *.tar.gz из директории _build в artifacts
$ tree artifacts                  #Команда tree графически выводит в консоли структуру нашего проекта
```

## Report

```ShellSession
$ cd ~/workspace/labs/
$ export LAB_NUMBER=08
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [DMG](https://cmake.org/cmake/help/latest/module/CPackDMG.html)
- [DEB](https://cmake.org/cmake/help/latest/module/CPackDeb.html)
- [RPM](https://cmake.org/cmake/help/latest/module/CPackRPM.html)
- [NSIS](https://cmake.org/cmake/help/latest/module/CPackNSIS.html)

```
Copyright (c) 2017 Братья Вершинины
```
