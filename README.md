# Лабораторная работа №2

Данная лабораторная работа посвещена изучению систем контроля версий на примере **Git**.

## Tutorial

Проинициализируем нужные для работы переменные, перейдем в рабочее пространство и активируем ранее подготовленные скрипты.
```sh
export GITHUB_USERNAME=AlexandrKonyaev
export GITHUB_EMAIL=<адрес_почтового_ящика>
export GITHUB_TOKEN=<сгенирированный_токен>
alias edit=nano

cd ${GITHUB_USERNAME}/workspace
source scripts/activate
```

Теперь создадим папку `.config`, в ней файл конфигурации `hub` с необходимыми переменными:

```sh
mkdir ~/.config
cat > ~/.config/hub <<EOF
github.com:
- user: ${GITHUB_USERNAME}
  oauth_token: ${GITHUB_TOKEN}
  protocol: https
EOF
git config --global hub.protocol https
```

Также последней командой установим протокол `https` для работы с гитхабом.

Создаём папку для второй лабы и переходим туда
```sh
mkdir projects/lab02t && cd projects/lab02t
```
Создаем новый репозиторий
```sh
git init
```

Вывод предупреждает о том, что текущая ветка называется `master`:
```
hint: Using 'master' as the name for the initial branch. This default branch name
hint: will change to "main" in Git 3.0. To configure the initial branch name
hint: to use in all of your new repositories, which will suppress this warning,
hint: call:
hint:
hint: 	git config --global init.defaultBranch <name>
hint:
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint:
hint: 	git branch -m <name>
hint:
hint: Disable this message with "git config set advice.defaultBranchName false"
Initialized empty Git repository in /home/home/lab02/.git/
```
В то время как на гитхабе главная ветка называется `main`, поэтому во избежание создания новой ветки переименуем локальную ветку `master` в `main`

```sh
git branch -m main
```

Установим юзернейм и почту для гита:
```sh
git config --global user.name ${GITHUB_USERNAME}
git config --global user.email ${GITHUB_EMAIL}
```

И проверим, что все пошло по плану:
```sh
git config -e --global
```

```                      
[hub]
        protocol = https
[user]
        name = AlexandrKonyaev
        email = 7758880@gmail.com
```
Все установилось правильно, поэтому следующей командой привяжем локальную директорию с созданным удаленно репозиторием:
```sh
git remote add origin https://github.com/AlexanderKonyaev/TIMP-lab02tut
```

И загрузим все, что есть в этом репозитории, на наш компьютер
```sh
git pull origin main
```
*Примечание: ветка на гитхабе называется `main`, а не `master`, поэтому предыдущая команда отличается от предложенной в tutorial.*

Вывод предыдущей команды:
```sh
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (3/3), 1.44 KiB | 738.00 KiB/s, done.
From https://github.com/AlexanderKonyaev/TIMP-lab02tut
 * branch            main       -> FETCH_HEAD
 * [new branch]      main       -> origin/main
```
 
Создадим `README.md`
```sh
touch README.md
```
 
Теперь посмотрим на статусы файлов в папке
```sh
git status
```
 
```
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	README.md

nothing added to commit but untracked files present (use "git add" to track)
```

Видно, что новосозданный README пока что никак не участвует в репозитории, поэтому мы можем его добавить и сделать новый коммит:
```sh
git add README.md
git commit -m "added README.md"
```

```
[main 147bfaf] added README.md:
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
```

И теперь можно запушить локальный коммит на удаленный репозиторий
```sh
git push origin main
```

```
Username for 'https://github.com': AlexandrKonyaev
Password for 'https://AlexandrKonyaev@github.com': 
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 4 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 321 bytes | 160.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To https://github.com/AlexanderKonyaev/TIMP-lab02tut
   be2a22a..147bfaf  main -> main
```

И на гитхабе появился README, значит мы молодцы.

Теперь удаленно создадим `.gitignore` и загрузим к нам.
```sh
git remote pull origin
```

```
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (3/3), 983 bytes | 491.00 KiB/s, done.
From https://github.com/AlexanderKonyaev/TIMP-lab02tut
 * branch            main       -> FETCH_HEAD
   147bfaf..c9aae32  main       -> origin/main
Updating 147bfaf..c9aae32
Fast-forward
 .gitignore | 4 ++++
 1 file changed, 4 insertions(+)
 create mode 100644 .gitignore
```

Посмотрим на историю репозитория 
```sh
git log
```

```
commit c9aae320c21960ee41b9cdcd6e18f4f3723e6c19 (HEAD -> main, origin/main)
Author: Flexander Konyaev <7758880@gmail.com>
Date:   Fri Jun 5 20:01:00 2026 +0300

    Create .gitignore

commit 147bfaf236537b1b7f92c1b2124a353116024d21
Author: AlexandrKonyaev <7758880@gmail.com>
Date:   Fri Jun 5 19:31:36 2026 +0300

    added README.md:

commit be2a22a7b04b17de585158570b46c8a5e5701c6a
Author: Flexander Konyaev <7758880@gmail.com>
Date:   Fri Jun 5 19:39:27 2026 +0300

    Initial commit
```

Ну и теперь создадим несколько `cpp` файлов

```sh
mkdir sources
mkdir include
mkdir examples

cat > sources/print.cpp <<EOF
#include <print.hpp>

void print(const std::string& text, std::ostream& out)
{
  out << text;
}

void print(const std::string& text, std::ofstream& out)
{
  out << text;
}
EOF

cat > include/print.hpp <<EOF
#include <fstream>
#include <iostream>
#include <string>

void print(const std::string& text, std::ofstream& out);
void print(const std::string& text, std::ostream& out = std::cout);
EOF

cat > examples/example1.cpp <<EOF
#include <print.hpp>

int main(int argc, char** argv)
{
  print("hello");
}
EOF

cat > examples/example2.cpp <<EOF
#include <print.hpp>

#include <fstream>

int main(int argc, char** argv)
{
  std::ofstream file("log.txt");
  print(std::string("hello"), file);
}
EOF
```

Коммитим и пушим их:
```sh
git add .
git commit -m"added sources"
git push origin master
```

```
[main 6b736b2] added sources
 5 files changed, 33 insertions(+)
 create mode 100644 examples/example1.cpp
 create mode 100644 examples/example2.cpp
 create mode 100644 include/print.hpp
 create mode 100644 sources/print.cpp
Username for 'https://github.com': AlexandrKonyaev
Password for 'https://AlexandrKonyaev@github.com': 
Enumerating objects: 12, done.
Counting objects: 100% (12/12), done.
Delta compression using up to 4 threads
Compressing objects: 100% (8/8), done.
Writing objects: 100% (10/10), 1.01 KiB | 259.00 KiB/s, done.
Total 10 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To https://github.com/AlexanderKonyaev/TIMP-lab02tut
   c9aae32..6b736b2  main -> main
```

Как можем видеть, все закоммитилось, и на гитхабе все файлы тоже видно.

# Homework

## Часть 1

### Шаг 1
*Создайте пустой репозиторий на сервисе github.com*

Создали этот репозиторий

### Шаг 2
*Выполните инструкцию по созданию первого коммита на странице репозитория, созданного на предыдущем шаге.*

```sh
git init
git branch -m main
git remote add origin https://github.com/Mimocake/TIMP-lab02
git pull origin main
```

Вывод последней команды:

```
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (3/3), 1.44 KiB | 736.00 KiB/s, done.
From https://github.com/AlexanderKonyaev/TIMP-lab02
 * branch            main       -> FETCH_HEAD
 * [new branch]      main       -> origin/main
```

### Шаг 3
*Создайте файл `hello_world.cpp` в локальной копии репозитория (который должен был появиться на шаге 2). Реализуйте программу Hello world на языке C++ используя плохой стиль кода. Например, после заголовочных файлов вставьте строку `using namespace std;`.*

Этот файл:
```cpp
#include <iostream>
using namespace std;

int main() {
	cout << "Hello world!" << name << endl;
	return 0;
}
```

### Шаг 4
*Добавьте этот файл в локальную копию репозитория.*
```sh
git add hello_world.cpp
```

### Шаг 5
*Закоммитьте изменения с осмысленным сообщением.*
```sh
git commit -m "added hello_world.cpp"
```

```
[main 3d420c7] added hello_world.cpp
 1 file changed, 7 insertions(+)
 create mode 100644 hello_world.cpp
```
### Шаг 6
*Изменитьте исходный код так, чтобы программа через стандартный поток ввода запрашивалось имя пользователя. А в стандартный поток вывода печаталось сообщение Hello world from @name, где @name имя пользователя.*

Новая версия программы
```cpp
#include <iostream>
using namespace std;

int main() {
	string name;
	cin >> name;
	cout << "Hello world from " << name << endl;
	return 0;
}
```

### Шаг 7
*Закоммитьте новую версию программы. Почему не надо добавлять файл повторно `git add`?*

```sh
git commit -a -m "modified hello_world"
```

Повторно `git add` не пишем, так как `hello_world.cpp` уже находится в репозитории, но добавим флаг `-a`, который означает коммит всех измененных файлов, чтобы этот файл смог закоммититься.

### Шаг 8
*Запуште изменения в удалёный репозиторий.*

```sh
git push origin main
```

```
Username for 'https://github.com': AlexandrKonyaev
Password for 'https://AlexandrKonyaev@github.com': 
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 4 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 755 bytes | 251.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To https://github.com/AlexanderKonyaev/TIMP-lab02
   4981ac4..f4104bf  main -> main
```

### Шаг 9
*Проверьте, что история коммитов доступна в удалённом репозитории.*

```sh
git log
```

```
commit f4104bfe4eab9dd4c9e624fc006e30f2142577af (HEAD -> main, origin/main)
Author: AlexandrKonyaev <7758880@gmail.com>
Date:   Sat Jun 6 09:02:02 2026 +0300

    modified hello_world

commit 3d420c73b2bffb5938cd9ee1313af1ed62228e94
Author: AlexandrKonyaev <7758880@gmail.com>
Date:   Sat Jun 6 08:50:11 2026 +0300

    added hello_world.cpp

commit 4981ac41bde1621f8b1b9c64209cf4a50e3b4513
Author: Flexander Konyaev <7758880@gmail.com>
Date:   Sat Jun 6 08:47:20 2026 +0300

    Initial commit
```

## Часть 2

### Шаг 1

*В локальной копии репозитория создайте локальную ветку `patch1`.*

```sh
git checkout -b patch1
```

### Шаг 2

*Внесите изменения в ветке `patch1` по исправлению кода и избавления от `using namespace std;`.*

Нам нужно только изменить код, никаких `git add` и тп делать не надо.

Новый код:
```cpp
#include <iostream>

int main() {
	std::string name;
	std::cin >> name;
	std::cout << "Hello world from " << name << std::endl;
	return 0;
}
```

### Шаг 3

***commit**, **push** локальную ветку в удалённый репозиторий.*

``` sh
git commit -a -m "removed using namespace std"
```

```
[patch1 e17641c] removed using namespace std
 1 file changed, 3 insertions(+), 4 deletions(-)
```

```sh
git push origin patch1
```

```
Username for 'https://github.com': AlexandrKonyaev
Password for 'https://AlexandrKonyaev@github.com': 
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 403 bytes | 403.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
remote: 
remote: Create a pull request for 'patch1' on GitHub by visiting:
remote:      https://github.com/AlexanderKonyaev/TIMP-lab02/pull/new/patch1
remote: 
To https://github.com/AlexanderKonyaev/TIMP-lab02
 * [new branch]      patch1 -> patch1
```

### Шаг 4

*Проверьте, что ветка `patch1` доступна в удалённом репозитории.*

```sh
git log
```

```
commit e17641c9f83b7c785502cc47c1d055ac5cee2991 (HEAD -> patch1, origin/patch1)
Author: AlexandrKonyaev <7758880@gmail.com>
Date:   Sat Jun 6 09:06:38 2026 +0300

    removed using namespace std

commit f4104bfe4eab9dd4c9e624fc006e30f2142577af (origin/main, main)
Author: AlexandrKonyaev <7758880@gmail.com>
Date:   Sat Jun 6 09:02:02 2026 +0300

    modified hello_world

commit 3d420c73b2bffb5938cd9ee1313af1ed62228e94
Author: AlexandrKonyaev <7758880@gmail.com>
Date:   Sat Jun 6 08:50:11 2026 +0300

    added hello_world.cpp

commit 4981ac41bde1621f8b1b9c64209cf4a50e3b4513
Author: Flexander Konyaev <7758880@gmail.com>
Date:   Sat Jun 6 08:47:20 2026 +0300

    Initial commit
```

Как видно, в последнем коммите фигурирует `origin/patch1`.

### Шаг 5

Я сделал pull-request удаленно на гитхабе. Чтобы убедиться, что все сработало, можно конечно посмотреть его на гитхабе, но в отчет вставлять фото неудобно, поэтому можно в этом убедиться с помощью утилиты `gh`.

```sh
gh pr view --json number,title,author
```

И получаем такой вывод
```
{
  "author": {
    "id": "U_kgDODpQR6A",
    "is_bot": false,
    "login": "AlexanderKonyaev",
    "name": "Flexander Konyaev"
  },
  "commits": [
    {
      "authoredDate": "2026-06-06T06:06:38Z",
      "authors": [
        {
          "email": "7758880@gmail.com",
          "id": "U_kgDODpQR6A",
          "login": "AlexanderKonyaev",
          "name": "AlexandrKonyaev"
        }
      ],
      "committedDate": "2026-06-06T06:06:38Z",
      "messageBody": "",
      "messageHeadline": "removed using namespace std",
      "oid": "e17641c9f83b7c785502cc47c1d055ac5cee2991"
    }
  ],
  "number": 1,
  "title": "removed using namespace std"
}
```

### Шаг 6

*В локальной копии в ветке `patch1` добавьте в исходный код комментарии.*

```cpp
// Подключаем библиотеку ввода-вывода
#include <iostream>

int main() {
	// Создаем строку для имени
	std::string name;
	// Принимаем имя
	std::cin >> name;
	// Печатаем
	std::cout << "Hello world from " << name << std::endl;
	return 0;
}
```

### Шаг 7

*commit, push*

```sh
git commit -a -m "added comments"
```

```
[patch1 41980e7] added comments
 1 file changed, 4 insertions(+)
```

```sh
git push origin patch1
```

```
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 512 bytes | 512.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To https://github.com/AlexanderKonyaev/TIMP-lab02
   e17641c..41980e7  patch1 -> patch1
```

### Шаг 8

*Проверьте, что новые изменения есть в созданном на шаге 5 pull-request*

```sh
gh pr view --json number,title,author,commits
```
```
{
  "author": {
    "id": "U_kgDODpQR6A",
    "is_bot": false,
    "login": "AlexanderKonyaev",
    "name": "Flexander Konyaev"
  },
  "commits": [
    {
      "authoredDate": "2026-06-06T06:06:38Z",
      "authors": [
        {
          "email": "7758880@gmail.com",
          "id": "U_kgDODpQR6A",
          "login": "AlexanderKonyaev",
          "name": "AlexandrKonyaev"
        }
      ],
      "committedDate": "2026-06-06T06:06:38Z",
      "messageBody": "",
      "messageHeadline": "removed using namespace std",
      "oid": "e17641c9f83b7c785502cc47c1d055ac5cee2991"
    },
    {
      "authoredDate": "2026-06-06T06:14:47Z",
      "authors": [
        {
          "email": "7758880@gmail.com",
          "id": "U_kgDODpQR6A",
          "login": "AlexanderKonyaev",
          "name": "AlexandrKonyaev"
        }
      ],
      "committedDate": "2026-06-06T06:14:47Z",
      "messageBody": "",
      "messageHeadline": "added comments",
      "oid": "41980e7d508ba8b03930bb5d35c1ff9b5a7ad2d1"
    }
  ],
  "number": 1,
  "title": "removed using namespace std"
}
```

Нетрудно заметить, что тут 2 коммита, то есть, действительно, оба изменения записались в pull-request.

### Шаг 9

*В удалённый репозитории выполните слияние PR `patch1 -> master` и удалите ветку `patch1` в удаленном репозитории.*

Сделали это на гитхабе

### Шаг 10

*Локально выполните **pull**.*

```sh
git checkout main
git pull origin main
```

```
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (1/1), 903 bytes | 903.00 KiB/s, done.
From https://github.com/AlexanderKonyaev/TIMP-lab02
 * branch            main       -> FETCH_HEAD
   f4104bf..f04e993  main       -> origin/main
Updating f4104bf..f04e993
Fast-forward
 hello_world.cpp | 11 +++++++----
 1 file changed, 7 insertions(+), 4 deletions(-)

```

### Шаг 11

*С помощью команды `git log` просмотрите историю в локальной версии ветки **main**.*

```
commit f04e99395a361f3261f4e5cfdb75b8b331368777 (HEAD -> main, origin/main)
Merge: f4104bf 41980e7
Author: Flexander Konyaev <7758880@gmail.com>
Date:   Sat Jun 6 09:16:27 2026 +0300

    Merge pull request #1 from AlexanderKonyaev/patch1
    
    removed using namespace std

commit 41980e7d508ba8b03930bb5d35c1ff9b5a7ad2d1 (origin/patch1, patch1)
Author: AlexandrKonyaev <7758880@gmail.com>
Date:   Sat Jun 6 09:14:47 2026 +0300

    added comments

commit e17641c9f83b7c785502cc47c1d055ac5cee2991
Author: AlexandrKonyaev <7758880@gmail.com>
Date:   Sat Jun 6 09:06:38 2026 +0300

    removed using namespace std

commit f4104bfe4eab9dd4c9e624fc006e30f2142577af
Author: AlexandrKonyaev <7758880@gmail.com>
Date:   Sat Jun 6 09:02:02 2026 +0300

    modified hello_world

commit 3d420c73b2bffb5938cd9ee1313af1ed62228e94
Author: AlexandrKonyaev <7758880@gmail.com>
Date:   Sat Jun 6 08:50:11 2026 +0300

    added hello_world.cpp

commit 4981ac41bde1621f8b1b9c64209cf4a50e3b4513
Author: Flexander Konyaev <7758880@gmail.com>
Date:   Sat Jun 6 08:47:20 2026 +0300

    Initial commit
```

### Шаг 12

*Удалите локальную ветку `patch1`.*

```sh
git branch -d patch1
```

```
Deleted branch patch1 (was 41980e7).
```

## Часть 3

### Шаг 1

*Создайте новую локальную ветку `patch2`.*

```sh
git checkout -b patch2
```

### Шаг 2

*Измените code style с помощью утилиты `clang-format`. Например, используя опцию `-style=Mozilla`.*

```sh
clang-format -style=Mozilla -i hello_world.cpp 
```

Вот так стал выглядеть код:
```cpp
// Подключаем библиотеку ввода-вывода
#include <iostream>

int
main()
{
  // Создаем строку для имени
  std::string name;
  // Принимаем имя
  std::cin >> name;
  // Печатаем
  std::cout << "Hello world from " << name << std::endl;
  return 0;
}
```

### Шаг 3

*commit, push, создайте pull-request `patch2 -> master`*

```sh
git commit -a -m "changed code style"
```

```
[patch2 f89732e] changed code style
 1 file changed, 10 insertions(+), 8 deletions(-)
```

```sh
git push origin patch2
```

```
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 407 bytes | 407.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
remote: 
remote: Create a pull request for 'patch2' on GitHub by visiting:
remote:      https://github.com/AlexanderKonyaev/TIMP-lab02/pull/new/patch2
remote: 
To https://github.com/AlexanderKonyaev/TIMP-lab02
 * [new branch]      patch2 -> patch2
```

Также сделаем pull-request и, как и в прошлый раз, проверим его создание с помощью утилиты `gh`:

```sh
gh pr view --json number,title,author,commits
```

```
{
  "author": {
    "id": "U_kgDODpQR6A",
    "is_bot": false,
    "login": "AlexanderKonyaev",
    "name": "Flexander Konyaev"
  },
  "commits": [
    {
      "authoredDate": "2026-06-06T06:21:33Z",
      "authors": [
        {
          "email": "7758880@gmail.com",
          "id": "U_kgDODpQR6A",
          "login": "AlexanderKonyaev",
          "name": "AlexandrKonyaev"
        }
      ],
      "committedDate": "2026-06-06T06:21:33Z",
      "messageBody": "",
      "messageHeadline": "changed code style",
      "oid": "f89732e2c60e2788af806c325275f607ed5f3af5"
    }
  ],
  "number": 2,
  "title": "changed code style"
}
```

### Шаг 4

*В ветке `main` в удаленном репозитории измените комментарии, например, расставьте знаки препинания, переведите комментарии на другой язык.*

Переключимся на векту `main`

```sh
git checkout main
```

И переведем комментарии на английский:
```cpp
// including standard IO library
#include <iostream>

int main() {
	// creating string for name
	std::string name;
	// recieving name
	std::cin >> name;
	// printing "hello world from name"
	std::cout << "Hello world from " << name << std::endl;
	return 0;
}
```

Ну и по классике:

```sh
git commit -a -m "translated comments"
```

```
[main 5eb44ad] translated comment
 1 file changed, 4 insertions(+), 4 deletions(-)
```

```sh
git push origin main
```

```
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 450 bytes | 450.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To https://github.com/AlexanderKonyaev/TIMP-lab02
   f04e993..5eb44ad  main -> main
```

### Шаг 5

Теперь переключимся на ветку `patch2`
```sh
git checkout patch2
```

И проверим наш pull-request, в этот раз посмотрим в том числе и на параметр `mergeable`
```sh
gh pr view --json number,title,author,commits,mergeable
```

```
{
  "author": {
    "id": "U_kgDODpQR6A",
    "is_bot": false,
    "login": "AlexanderKonyaev",
    "name": "Flexander Konyaev"
  },
  "commits": [
    {
      "authoredDate": "2026-06-06T06:21:33Z",
      "authors": [
        {
          "email": "7758880@gmail.com",
          "id": "U_kgDODpQR6A",
          "login": "AlexanderKonyaev",
          "name": "AlexandrKonyaev"
        }
      ],
      "committedDate": "2026-06-06T06:21:33Z",
      "messageBody": "",
      "messageHeadline": "changed code style",
      "oid": "f89732e2c60e2788af806c325275f607ed5f3af5"
    }
  ],
  "mergeable": "CONFLICTING",
  "number": 2,
  "title": "changed code style"
}
```

И как можно заметить, этот параметр равен `CONFLICTING`, то есть действительно присутствуют конфликты.

### Шаг 6

*Для этого локально выполните `pull` + `rebase` (точную последовательность команд, следует узнать самостоятельно). Исправьте конфликты.*

Для начала на всякий случай подтянем ветку `main`, чтобы она была синхронизированна с удалённым репозиторием.

```sh
git checkout main
git pull origin main
```

Дальше переключаемся на другую ветку и пытаемся ее перебазировать:

```sh
git checkout patch2
git rebase main
```

На что гит выведет следующее сообщение:
```
Switched to branch 'patch2'
Auto-merging hello_world.cpp
CONFLICT (content): Merge conflict in hello_world.cpp
error: could not apply f89732e... changed code style
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
hint: Disable this message with "git config set advice.mergeConflict false"
Could not apply f89732e... # changed code style

```

А `hello_world.cpp` стал вот таким:

```cpp                                                                       
// including standard IO library
#include <iostream>

<<<<<<< HEAD
int main() {
        // creating string for name
        std::string name;
        // recieving name
        std::cin >> name;
        // printing "hello world from name"
        std::cout << "Hello world from " << name << std::endl;
        return 0;
=======
int
main()
{
  // Создаем строку для имени
  std::string name;
  // Принимаем имя
  std::cin >> name;
  // Печатаем
  std::cout << "Hello world from " << name << std::endl;
  return 0;
>>>>>>> f89732e (changed code style)
}
```

Исправим конфликт, совместив английские комментарии с другим стилем кода:
```sh
// including standard IO library
#include <iostream>

int
main()
{
  // creating string for name
  std::string name;
  // recieving name
  std::cin >> name;
  // printing "hello world from name"
  std::cout << "Hello world from " << name << std::endl;
  return 0;
}
```

Теперь пометим этот файл как переделанный с помощью команды 
```sh
git add hello_world.cpp
```

И продолжим перебазирование:
```sh
git rebase --continue
```

Других конфликтов не возникло. Гит предложил ввести название этого перебазирования и потом вывел это:
```
[detached HEAD 16546a2] changed code style
 1 file changed, 10 insertions(+), 8 deletions(-)
Successfully rebased and updated refs/heads/patch2.
```

### Шаг 7

*Сделайте force push в ветку `patch2`*

```sh
git push --force origin patch2
```

```
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 452 bytes | 452.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To https://github.com/AlexanderKonyaev/TIMP-lab02
 + f89732e...16546a2 patch2 -> patch2 (forced update)
```

### Шаг 8

*Убедитесь, что в pull-request пропали конфликтны.*

Делаем это уже привычным для нас образом

```sh
gh pr view --json number,title,author,commits,mergeable
```

```
{
  "author": {
    "id": "U_kgDODpQR6A",
    "is_bot": false,
    "login": "AlexanderKonyaev",
    "name": "Flexander Konyaev"
  },
  "commits": [
    {
      "authoredDate": "2026-06-06T06:21:33Z",
      "authors": [
        {
          "email": "7758880@gmail.com",
          "id": "U_kgDODpQR6A",
          "login": "AlexanderKonyaev",
          "name": "AlexandrKonyaev"
        }
      ],
      "committedDate": "2026-06-06T06:28:45Z",
      "messageBody": "",
      "messageHeadline": "changed code style",
      "oid": "16546a2087df08bc2a5b2d8cf9b065bade24fc6d"
    }
  ],
  "mergeable": "MERGEABLE",
  "number": 2,
  "title": "changed code style"
}
```

Теперь переменная `mergeable` равна `MERGEABLE`.

### Шаг 9

*Вмержите pull-request `patch2` -> `master`.*

Сделали это на гитхабе.

Теперь можно посмотреть последние 2 коммита и убедиться, что слияние прошло успешно:
```sh
git checkout main
git log -2
```

```
commit 5eb44ad34fc70f2d6e52a8e7f608ed01ef6279a0 (HEAD -> main, origin/main)
Author: AlexandrKonyaev <7758880@gmail.com>
Date:   Sat Jun 6 09:24:39 2026 +0300

    translated comment

commit f04e99395a361f3261f4e5cfdb75b8b331368777
Merge: f4104bf 41980e7
Author: Flexander Konyaev <7758880@gmail.com>
Date:   Sat Jun 6 09:16:27 2026 +0300

    Merge pull request #1 from AlexanderKonyaev/patch1
    
    removed using namespace std
```


