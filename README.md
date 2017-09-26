## Laboratory work IV

Данная лабораторная работа посвещена изучению систем контроля версий на примере **Git**.

```bash
$ open https://git-scm.com  # Открыть полезный сайт
```

## Tasks

- [X] 1. Создать публичный репозиторий с названием **lab03** и с лиценцией **MIT**
- [X] 2. Ознакомиться со ссылками учебного материала
- [X] 3. Выполнить инструкцию учебного материала
- [X] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_USERNAME=DespiteDeath # Устанавливаем значение переменной окружения GITHUB_USERNAME
$ export GITHUB_EMAIL=12l12z12k@gmail.com # Устанавливаем значение переменной окружения
$ alias edit=subl # Выбираем текстовый редактор, в котором будем работать
```

```ShellSession
$ mkdir lab03 && cd lab03 # Создаем директорию, меняем директорию
$ git init # Инициализируем существующее хранилище
$ git config --global user.name ${GITHUB_USERNAME} # Объявлем юзернейм
$ git config --global user.email ${GITHUB_EMAIL} # Объявляем почту
$ git config -e --global # Редактируем 
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab03 #
$ git pull origin master # Объединяем репозиторий
$ touch README.md # Добавляем README
$ git status # Показываем статус
$ git add README.md # Добавляем файл
$ git commit -m"added README.md" # Коммитим
$ git push origin master # Пушим
```

Добавить на сервисе **GitHub** в репозитории **lab03** файл **.gitignore**
со следующем содержимом:

```ShellSession
*build*/ 
*install*/
*.swp
```

```ShellSession
$ git pull origin master # Объединяем репозиторий
$ git log # Смотрим логи
```

```ShellSession
$ mkdir sources # Создаем папку sources
$ mkdir include # Создаем папку include
$ mkdir examples # Создаем папку examples 
```

Cоздаем и записываем новый файл (print.cpp) в папке sources

```ShellSession
$ cat > sources/print.cpp <<EOF
#include <print.hpp>

void print(const std::string& text, std::ostream& out) {
  out << text;
}

void print(const std::string& text, std::ofstream& out) {
  out << text;
}
EOF
```
Cоздаем и записываем новый файл (print.hpp) в папке include

```ShellSession
$ cat > include/print.hpp <<EOF 
#include <string>
#include <fstream>
#include <iostream>

void print(const std::string& text, std::ostream& out = std::cout);
void print(const std::string& text, std::ofstream& out);
EOF
```
Cоздаем и записываем новый файл (example1.сpp) в папке examples

```ShellSession
$ cat > examples/example1.cpp <<EOF
#include <print.hpp>

int main(int argc, char** argv) {
  print("hello");
}
EOF
```
Cоздаем и записываем новый файл (example2.сpp) в папке examples

```ShellSession
$ cat > examples/example2.cpp <<EOF
#include <fstream>
#include <print.hpp>

int main(int argc, char** argv) {
  std::ofstream file("log.txt");
  print(std::string("hello"), file);
}
EOF
```

```ShellSession
$ edit README.md # Редактируем файл
```

Отправляем последние  изменения на GitHub сервер
```ShellSession
$ git status # Показывает рабочий статус дерева
$ git add . # Добавить содержимое
$ git commit -m"added sources" # Коммитим
$ git push origin master # Пушим 
```

## Report

```ShellSession
$ cd ~/workspace/labs/ 
$ export LAB_NUMBER=03 
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER} 
$ mkdir reports/lab${LAB_NUMBER} 
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md 
$ cd reports/lab${LAB_NUMBER} 
$ edit REPORT.md 
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [GitHub](https://github.com)
- [Bitbucket](https://bitbucket.org)
- [Gitlab](https://about.gitlab.com)
- [LearnGitBranching](http://learngitbranching.js.org/)

```
Copyright (c) 2017 Братья Вершинины
```
