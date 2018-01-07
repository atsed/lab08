[![Build Status](https://travis-ci.org/desta-study/lab08.svg?branch=master)](https://travis-ci.org/desta-study/lab08)

## Laboratory work VIII

Данная лабораторная работа посвещена изучению средств пакетирования на примере **CPack**

```ShellSession
$ open https://cmake.org/Wiki/CMake:CPackPackageGenerators
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab08** на сервисе **GitHub**
- [x] 2. Выполнить инструкцию учебного материала
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

Делаем первоначальные настройки
```ShellSession
$ export GITHUB_USERNAME=desta-study # Выставляем переменную окружения
$ export GITHUB_EMAIL=kall-lightman@yandex.ru # Выставляем переменную окружения
$ alias edit=vi # Команда edit присваиваем значение команды vi
$ alias gsed=sed # for *-nix system, так же присваиваем команде gstd команду sed
$ cd ${GITHUB_USERNAME}/workspace #Перезодим в папку
$ pushd . #Запомнить текущий каталог и перейти в указанный
~/desta-study/workspace ~/desta-study/workspace
$ source scripts/activate #Активация скрипта
```

Копируем и подключаем нужный репозиторий
```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab07 lab08 # Клонируем файлы из lab07 в lab08
Cloning into 'projects/lab08'...
remote: Counting objects: 122, done.
remote: Compressing objects: 100% (80/80), done.
remote: Total 122 (delta 29), reused 122 (delta 29), pack-reused 0
Receiving objects: 100% (122/122), 564.93 KiB | 704.00 KiB/s, done.
Resolving deltas: 100% (29/29), done.
Checking connectivity... done.

$ cd lab08 # Заходим в дирректорию lab08
$ git remote remove origin # Отключаемся от lab07
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab08 # Подключаемся к lab08
```

Редактируем файл CMakeLists.txt: вносим параметры
```ShellSession
# Внесение изменений в CMakeLists.txt
# Здесь вносим данные о версии проекта print
$ gsed -i '/project(print)/a\
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

Создаём и редактируем файлы
```ShellSession
$ touch DESCRIPTION && edit DESCRIPTION # Создаем и редактируем файл DESCRIPTION
$ touch ChangeLog.md # # Создание файла ChangeLog.md
$ DATE=`date` cat > ChangeLog.md <<EOF # Вносим изменения в файл
* ${DATE} ${GITHUB_USERNAME} <${GITHUB_EMAIL}> 0.1.0.0
- Initial RPM release
EOF
```

Работа с CPackConfig.cmake
```ShellSession
$ cat > CPackConfig.cmake <<EOF # Подключаем библиотеку для CPack
include(InstallRequiredSystemLibraries)
EOF
```

Продолжение работы с файлом CPackConfig.cmake 

```ShellSession
$ cat >> CPackConfig.cmake <<EOF # Переносим значения переменных из файла CMakeLists.txt
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

Указываем путь к файлам LICENSE и README.md

```ShellSession
$ cat >> CPackConfig.cmake <<EOF

set(CPACK_RESOURCE_FILE_LICENSE \${CMAKE_CURRENT_SOURCE_DIR}/LICENSE)
set(CPACK_RESOURCE_FILE_README \${CMAKE_CURRENT_SOURCE_DIR}/README.md)
EOF
```
Сборка RPM пакета
Работа с CPackConfig.cmake
```ShellSession
# Внесение изменений в CPackConfig.cmake
# Устанавливаем параметры для CPACK_RPM_PACKAGE
$ cat >> CPackConfig.cmake <<EOF

set(CPACK_RPM_PACKAGE_NAME "print-devel")
set(CPACK_RPM_PACKAGE_LICENSE "MIT")
set(CPACK_RPM_PACKAGE_GROUP "print")
set(CPACK_RPM_PACKAGE_URL "https://github.com/${GITHUB_USERNAME}/lab07")
set(CPACK_RPM_CHANGELOG_FILE \${CMAKE_CURRENT_SOURCE_DIR}/ChangeLog.md)
set(CPACK_RPM_PACKAGE_RELEASE 1)
EOF
```
Cборка DEB пакета
Работа с CPackConfig.cmake
```ShellSession
# Внесение изменений в CPackConfig.cmake
# Устанавливаем параметры для CPACK_DEBIAN_PACKAGE
$ cat >> CPackConfig.cmake <<EOF

set(CPACK_DEBIAN_PACKAGE_NAME "libprint-dev")
set(CPACK_DEBIAN_PACKAGE_HOMEPAGE "https://${GITHUB_USERNAME}.github.io/lab07")
set(CPACK_DEBIAN_PACKAGE_PREDEPENDS "cmake >= 3.0")
set(CPACK_DEBIAN_PACKAGE_RELEASE 1)
EOF
```

Подключаем CPack (к CPackConfig.cmake)
```ShellSession
# Внесение изменений в CPackConfig.cmake
# Подключение CPack
$ cat >> CPackConfig.cmake <<EOF

include(CPack)
EOF
```

Подключаем CPackConfig.cmake (к CMakeLists.txt)
```ShellSession
# Внесение изменений в CMakeLists.txt
# Подключение CPackConfig.cmake
$ cat >> CMakeLists.txt <<EOF

include(CPackConfig.cmake)
EOF 
```

Заменяем все данные из lab07 на данные lab08 README.md
```ShellSession
$ gsed -i 's/lab07/lab08/g' README.md # Внесение изменений в файл README.md
```

Выполняем команды для настройки локального репозитория для дальнейшей отправки в удаленный репозиторий восьмой лабораторной работы
```ShellSession
$ git add . # Подтверждаем внесённые изменения
$ git commit -m"added cpack config" # Коммитим

[master 8aae849] added cpack config
 5 files changed, 38 insertions(+), 1 deletion(-)
 create mode 100644 CPackConfig.cmake
 create mode 100644 ChangeLog.md
 create mode 100644 DESCRIPTION

$ git push origin master # Отправляем на сайт

Username for 'https://github.com': desta-study
Password for 'https://desta-study@github.com': 
Counting objects: 70, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (43/43), done.
Writing objects: 100% (70/70), 482.66 KiB | 0 bytes/s, done.
Total 70 (delta 20), reused 60 (delta 16)
remote: Resolving deltas: 100% (20/20), done.
To https://github.com/desta-study/lab08
 * [new branch]      master -> master

```

Активируем репозиторий в travis
```ShellSession
$ travis login --auto # Логинимся
$ travis enable # Активируем
```

Работа с cmake
```ShellSession
$ cmake -H. -B_build # Собираем с помощью cmake

-- The C compiler identification is GNU 5.4.0
-- The CXX compiler identification is GNU 5.4.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/desta-study/desta-study/workspace/projects/lab08/_build

$ cmake --build _build # Создаём папку _build с изменениями

Scanning dependencies of target print
[ 50%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[100%] Linking CXX static library libprint.a
[100%] Built target print

$ cd _build # Входим в дирректорию
# Создание директорий проекта при помощи TGZ,RPM,DEB,NSIS,DragNDrop
$ cpack -G "TGZ" # Генерируем пакеты

CPack: Create package using TGZ
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print
CPack: Create package
CPack: - package: /home/desta-study/desta-study/workspace/projects/lab08/_build/print-0.1.0.0-Linux.tar.gz generated.

$ cpack -G "RPM"

CPack: Create package using RPM
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print
CPack: Create package
CMake Error at /usr/share/cmake-3.5/Modules/CPackRPM.cmake:1069 (message):
  RPM package requires rpmbuild executable
Call Stack (most recent call first):
  /usr/share/cmake-3.5/Modules/CPackRPM.cmake:1787 (cpack_rpm_generate_package)

$ cpack -G "DEB" 

CPack: Create package using DEB
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print
CPack: Create package
CPack: - package: /home/desta-study/desta-study/workspace/projects/lab08/_build/print-0.1.0.0-Linux.deb generated.

$ cpack -G "NSIS"
$ cpack -G "DragNDrop"
$ cd .. # Выходим на одну назад
```
Собираем пакет TGZ через cmake
```ShellSession
# -H. устанавливаем каталог в который сгенерируется файл CMakeLists.txt
# -B_build указывает директорию для собираемых файлов
# -D - заменяет команду set
$ cmake -H. -B_build -DCPACK_GENERATOR="TGZ" # Сборка из cmake пакета

-- Configuring done
-- Generating done
-- Build files have been written to: /home/desta-study/desta-study/workspace/projects/lab08/_build

# --build _build создает бинарное дерево проекта

[100%] Built target print
Run CPack packaging tool...
CPack: Create package using TGZ
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print
CPack: Create package
CPack: - package: /home/desta-study/desta-study/workspace/projects/lab08/_build/print-0.1.0.0-Linux.tar.gz generated.

# --target указывает необходимые для обработки цели
$ cmake --build _build --target package # Указываем необходимые для обработки цели
```

Создаём и перемещаем..
```ShellSession
$ mkdir artifacts # Создаём дирректорию artifacts
$ mv _build/*.tar.gz artifacts # Перемещение проектов *.tar.gz из директории _build в artifacts
$ tree artifacts # Графически выводим в консоль структуру проекта

artifacts
├── print-0.1.0.0-Linux.tar.gz
└── screenshot.png

0 directories, 2 files

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
