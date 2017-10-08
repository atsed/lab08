[![Build Status](https://travis-ci.org/GolubDobra/lab06.svg?branch=master)](https://travis-ci.org/GolubDobra/lab06)
## Laboratory work VI

Данная лабораторная работа посвещена изучению фреймворков для тестирования на примере **Catch**

```ShellSession
$ open https://github.com/philsquared/Catch
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab06** на сервисе **GitHub**
- [x] 2. Выполнить инструкцию учебного материала
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

#### Установка значений для сервиса **GitHub**

```ShellSession
$ export GITHUB_USERNAME=poljkee2010 # Устанавливаем значение переменной окружения GITHUB_USERNAME
```

#### Инициализируем директорию **lab06**

```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab05 lab06
Клонирование в «lab06»…
$ cd lab06 
$ git remote remove origin 
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab06         
```

```ShellSession
$ mkdir tests       				#Создаем каталог tests
$ wget https://github.com/philsquared/Catch/releases/download/v1.9.3/catch.hpp -O tests/catch.hpp    #Подключаем библиотеку для модульного тестирования на языке С++ catch.hpp
$ cat > tests/main.cpp <<EOF                    #Изменяем файл main.cpp
#define CATCH_CONFIG_MAIN
#include "catch.hpp"
EOF
```

#### Добавляем изменения в **CMakeLists.txt**

```ShellSession
$ sed -i '' '/option(BUILD_EXAMPLES "Build examples" OFF)/a\    #Записываем option(BUILD_TESTS "Build tests" OFF) в файл CMakeLists.txt
option(BUILD_TESTS "Build tests" OFF)
' CMakeLists.txt
$ cat >> CMakeLists.txt <<EOF

if(BUILD_TESTS)
	enable_testing()
	file(GLOB \${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
	add_executable(check \${\${PROJECT_NAME}_TEST_SOURCES})
	target_link_libraries(check \${PROJECT_NAME} \${DEPENDS_LIBRARIES})
	add_test(NAME check COMMAND check "-s" "-r" "compact" "--use-colour" "yes") 
endif()
EOF
```

#### Добавляем изменения в **test1.cpp**

```ShellSession
$ cat >> tests/test1.cpp <<EOF
#include "catch.hpp"
#include <print.hpp>

TEST_CASE("output values should match input values", "[file]") {
  std::string text = "hello";
  std::ofstream out("file.txt");
  
  print(text, out);
  out.close();
  
  std::string result;
  std::ifstream in("file.txt");
  in >> result;
  
  REQUIRE(result == text);
}
EOF
```

#### Собираем проект

```ShellSession
$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install -DBUILD_TESTS=ON
$ cmake --build _build 						# Запускаем сборку в каталоге _build
$ cmake --build _build --target test #Сборка test 
```

#### Работаем с сервисом **Travis**

```ShellSession
$ sed -i '' 's/lab05/lab06/g' README.md                                             # Добавляем изменения в файле README.md
$ sed -i '' 's/\(DCMAKE_INSTALL_PREFIX=_install\)/\1 -DBUILD_TESTS=ON/' .travis.yml # Добавляем изменения в файле .travis.yml
$ sed -i '' '/cmake --build _build --target install/a\
- cmake --build _build --target test
' .travis.yml
```

```ShellSession
$ travis lint # display warnings for a .travis.yml
Warnings for .travis.yml:
[x] value for addons section is empty, dropping
[x] in addons section: unexpected key apt, dropping
```

#### Работаем в **GitHub** 

```ShellSession
$ git add .                     # Отследить изменения всех файлов
$ git commit -m"added tests"    # Сохранить изменения
$ git push origin master        # Загрузка файлов на сервер
```

#### Работаем с сервисом **Travis**

```ShellSession
$ travis login --auto 
$ travis enable 
```

```ShellSession
$ mkdir artifacts                                 #Создаем каталог
$ screencapture -T 20 artifacts/screenshot.jpg    #Делаем скриншот экрана и кидаем в каталог artifacts
<Command>-T
$ open https://github.com/${GITHUB_USERNAME}/lab06
```

## Report

```ShellSession
$ cd ~/workspace/labs/
$ export LAB_NUMBER=06
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [Boost.Tests](http://www.boost.org/doc/libs/1_63_0/libs/test/doc/html/)
- [Google Test](https://github.com/google/googletest)

```
Copyright (c) 2017 Братья Вершинины
```
