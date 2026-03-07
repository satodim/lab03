# Лабораторная работа №2

Данная лабораторная работа посвещена изучению систем контроля версий на примере **Git**.

## Tutorial

Проинициализируем нужные для работы переменные, перейдем в рабочее пространство и активируем ранее подготовленные скрипты.
```sh
export GITHUB_USERNAME=Mimocake
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
hint: is subject to change. To configure the initial branch name to use in all
hint: of your new repositories, which will suppress this warning, call:
hint: 
hint: 	git config --global init.defaultBranch <name>
hint: 
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint: 
hint: 	git branch -m <name>
Initialized empty Git repository in /home/univer/Desktop/timp/Mimocake/workspace/projects/lab02t/.git/
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
        name = Mimocake
        email = <моя почта>@yandex.ru
```
Все установилось правильно, поэтому следующей командой привяжем локальную директорию с созданным удаленно репозиторием:
```sh
git remote add origin https://github.com/${GITHUB_USERNAME}/lab02t.git
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
Unpacking objects: 100% (3/3), 1.45 KiB | 744.00 KiB/s, done.
From https://github.com/Mimocake/lab02t
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
[main 19701b1] added README.md
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
```

И теперь можно запушить локальный коммит на удаленный репозиторий
```sh
git push origin main
```

```
Username for 'https://github.com': Mimocake
Password for 'https://Mimocake@github.com': 
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 4 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 307 bytes | 307.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/Mimocake/lab02t.git
   2b5f135..19701b1  main -> main
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
Unpacking objects: 100% (3/3), 993 bytes | 496.00 KiB/s, done.
From https://github.com/Mimocake/lab02t
 * branch            main       -> FETCH_HEAD
   19701b1..4fc5ac2  main       -> origin/main
Updating 19701b1..4fc5ac2
Fast-forward
 .gitignore | 4 ++++
 1 file changed, 4 insertions(+)
 create mode 100644 .gitignore
```

На всякий случай убедимся, что все действительно загрузилось:
```sh
ls -a
```

```
.  ..  .git  .gitignore  LICENSE  README.md
```
Действительно, `.gitignore` загрузился

Посмотрим на историю репозитория 
```sh
git log
```

```
commit 4fc5ac20c9dc9ecc8fb5493645726145eed30953 (HEAD -> main, origin/main)
Author: Roman Chekrygin <88584860+Mimocake@users.noreply.github.com>
Date:   Thu Mar 5 12:56:58 2026 +0300

    Create .gitignore

commit 19701b13454bd7a64edbd07011e16323424d3f8f
Author: Mimocake <rchekr@yandex.ru>
Date:   Thu Mar 5 12:37:38 2026 +0300

    added README.md

commit 2b5f1353c2c0a8d60a8d693bb0a9bebecfc874c3
Author: Roman Chekrygin <88584860+Mimocake@users.noreply.github.com>
Date:   Thu Mar 5 12:15:22 2026 +0300

    Initial commit
```
Видно, что первый коммит был сделан мною на гитхабе, второй тоже мною (но формально другим мною) в консоли, и третий опять на гитхабе

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

И напоследок можно еще раз ввести `git log` для проверки

```
Author: Mimocake <rchekr@yandex.ru>
Date:   Thu Mar 5 13:20:32 2026 +0300

    added sources

commit 4fc5ac20c9dc9ecc8fb5493645726145eed30953
Author: Roman Chekrygin <88584860+Mimocake@users.noreply.github.com>
Date:   Thu Mar 5 12:56:58 2026 +0300

    Create .gitignore

commit 19701b13454bd7a64edbd07011e16323424d3f8f
Author: Mimocake <rchekr@yandex.ru>
Date:   Thu Mar 5 12:37:38 2026 +0300

    added README.md

commit 2b5f1353c2c0a8d60a8d693bb0a9bebecfc874c3
Author: Roman Chekrygin <88584860+Mimocake@users.noreply.github.com>
Date:   Thu Mar 5 12:15:22 2026 +0300

    Initial commit
```

Как можем видеть, все закоммитилось, и на гитхабе все файлы тоже видно.

# Homework

## Часть 1

### Шаг 1
*Создайте пустой репозиторий на сервисе github.com*

Создали **этот** репозиторий

### Шаг 2
*Выполните инструкцию по созданию первого коммита на странице репозитория, созданного на предыдущем шаге.*

```sh
git init
git branch -m main
git remote add origin https://github.com/Mimocake/TIMP-lab02
git pull origin master
```

Вывод последней команды:

```
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (3/3), 1.45 KiB | 745.00 KiB/s, done.
From https://github.com/Mimocake/TIMP-lab02
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
[main 19baeaf] added hello_world.cpp
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
git commit -a -m "modified hello_world.cpp"
```

Повторно `git add` не пишем, так как `hello_world.cpp` уже находится в репозитории, но добавим флаг `-a`, который означает коммит всех измененных файлов, чтобы этот файл смог закоммититься.

### Шаг 8
*Запуште изменения в удалёный репозиторий.*

```sh
git push origin main
```

```
Username for 'https://github.com': Mimocake
Password for 'https://Mimocake@github.com': 
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 4 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 745 bytes | 745.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/Mimocake/TIMP-lab02
   f776454..076186a  main -> main
```

### Шаг 9
*Проверьте, что история коммитов доступна в удалённом репозитории.*

```sh
git log
```

```
commit 076186a0e27ecb60e1bcebc2bb46c88470f7d187 (HEAD -> main, origin/main)
Author: Mimocake <rchekr@yandex.ru>
Date:   Fri Mar 6 17:05:21 2026 +0300

    modified hello_world.cpp

commit 19baeafa88f4dbfa28775a5b4043ca0e67b73fb0
Author: Mimocake <rchekr@yandex.ru>
Date:   Fri Mar 6 17:00:58 2026 +0300

    added hello_world.cpp

commit f776454370e700c28831b42490d171c3bc559b96
Author: Roman Chekrygin <88584860+Mimocake@users.noreply.github.com>
Date:   Fri Mar 6 16:44:28 2026 +0300

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
[patch1 5faa54d] removed using namespace std
 1 file changed, 3 insertions(+), 4 deletions(-)
```

```sh
git push origin patch1
```

```
Username for 'https://github.com': Mimocake
Password for 'https://Mimocake@github.com': 
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 397 bytes | 397.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
remote: 
remote: Create a pull request for 'patch1' on GitHub by visiting:
remote:      https://github.com/Mimocake/TIMP-lab02/pull/new/patch1
remote: 
To https://github.com/Mimocake/TIMP-lab02
 * [new branch]      patch1 -> patch1
```

### Шаг 4

*Проверьте, что ветка `patch1` доступна в удалённом репозитории.*

```sh
git log
```

```
commit 5faa54d014ddd9a527142ffbb57589e783a30a11 (HEAD -> patch1, origin/patch1)
Author: Mimocake <rchekr@yandex.ru>
Date:   Sat Mar 7 11:25:21 2026 +0300

    removed using namespace std

commit 076186a0e27ecb60e1bcebc2bb46c88470f7d187 (origin/main, main)
Author: Mimocake <rchekr@yandex.ru>
Date:   Fri Mar 6 17:05:21 2026 +0300

    modified hello_world.cpp

commit 19baeafa88f4dbfa28775a5b4043ca0e67b73fb0
Author: Mimocake <rchekr@yandex.ru>
Date:   Fri Mar 6 17:00:58 2026 +0300

    added hello_world.cpp

commit f776454370e700c28831b42490d171c3bc559b96
Author: Roman Chekrygin <88584860+Mimocake@users.noreply.github.com>
Date:   Fri Mar 6 16:44:28 2026 +0300

    Initial commit
```

Как видно, в последнем коммите фигурирует `origin/patch1`.

### Шаг 5

Я сделал pull-request удаленно на гитхабе. Чтобы убедиться, что все сработало, можно конечно посмотреть его на гитхабе, но в отчет вставлять фото неудобно, поэтому можно в этом убедиться с помощью утилиты `gh`. Просто вывести информацию о pull-request'е у меня не получилось из-за какой-то ошибки, но сработал вывод в формате json.

```sh
gh pr view --json number,title,author
```

И получаем такой вывод
```
{
  "author": {
    "login": "Mimocake"
  },
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
[patch1 92d60fc] added comments
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
Writing objects: 100% (3/3), 507 bytes | 507.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/Mimocake/TIMP-lab02
   5faa54d..92d60fc  patch1 -> patch1
```

### Шаг 8

*Проверьте, что новые изменения есть в созданном на шаге 5 pull-request*

```sh
gh pr view --json number,title,author,commits
```

Теперь выведем также и коммиты в пулл-реквесте (к сожалению, на шаге 5 я это не сделал, но, надеюсь, это не страшно)
```
{
  "author": {
    "login": "Mimocake"
  },
  "commits": [
    {
      "authoredDate": "2026-03-07T08:25:21Z",
      "authors": [
        {
          "email": "rchekr@yandex.ru",
          "id": "MDQ6VXNlcjg4NTg0ODYw",
          "login": "Mimocake",
          "name": "Mimocake"
        }
      ],
      "committedDate": "2026-03-07T08:25:21Z",
      "messageBody": "",
      "messageHeadline": "removed using namespace std",
      "oid": "5faa54d014ddd9a527142ffbb57589e783a30a11"
    },
    {
      "authoredDate": "2026-03-07T11:29:19Z",
      "authors": [
        {
          "email": "rchekr@yandex.ru",
          "id": "MDQ6VXNlcjg4NTg0ODYw",
          "login": "Mimocake",
          "name": "Mimocake"
        }
      ],
      "committedDate": "2026-03-07T11:29:19Z",
      "messageBody": "",
      "messageHeadline": "added comments",
      "oid": "92d60fc7065047e1c9f97ff2d77a2c8f8de92525"
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
Unpacking objects: 100% (1/1), 908 bytes | 908.00 KiB/s, done.
From https://github.com/Mimocake/TIMP-lab02
 * branch            main       -> FETCH_HEAD
   076186a..e122c12  main       -> origin/main
Updating 076186a..e122c12
Fast-forward
 hello_world.cpp | 11 +++++++----
 1 file changed, 7 insertions(+), 4 deletions(-)
```

### Шаг 11

*С помощью команды `git log` просмотрите историю в локальной версии ветки **main**.*

```
commit e122c12ba299dd8389343b290416fc0f2e1f2820 (HEAD -> main, origin/main)
Merge: 076186a 92d60fc
Author: Roman Chekrygin <88584860+Mimocake@users.noreply.github.com>
Date:   Sat Mar 7 15:21:26 2026 +0300

    Merge pull request #1 from Mimocake/patch1
    
    шаг 9

commit 92d60fc7065047e1c9f97ff2d77a2c8f8de92525 (origin/patch1, patch1)
Author: Mimocake <rchekr@yandex.ru>
Date:   Sat Mar 7 14:29:19 2026 +0300

    added comments

commit 5faa54d014ddd9a527142ffbb57589e783a30a11
Author: Mimocake <rchekr@yandex.ru>
Date:   Sat Mar 7 11:25:21 2026 +0300

    removed using namespace std

commit 076186a0e27ecb60e1bcebc2bb46c88470f7d187
Author: Mimocake <rchekr@yandex.ru>
Date:   Fri Mar 6 17:05:21 2026 +0300

    modified hello_world.cpp

commit 19baeafa88f4dbfa28775a5b4043ca0e67b73fb0
Author: Mimocake <rchekr@yandex.ru>
Date:   Fri Mar 6 17:00:58 2026 +0300

    added hello_world.cpp

commit f776454370e700c28831b42490d171c3bc559b96
Author: Roman Chekrygin <88584860+Mimocake@users.noreply.github.com>
Date:   Fri Mar 6 16:44:28 2026 +0300

    Initial commit
```

### Шаг 12

*Удалите локальную ветку `patch1`.*

```sh
git branch -d patch1
```

```
Deleted branch patch1 (was 92d60fc).
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
[patch2 0c61fbe] changed code style
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
Writing objects: 100% (3/3), 399 bytes | 399.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
remote: 
remote: Create a pull request for 'patch2' on GitHub by visiting:
remote:      https://github.com/Mimocake/TIMP-lab02/pull/new/patch2
remote: 
To https://github.com/Mimocake/TIMP-lab02
 * [new branch]      patch2 -> patch2
```

Также сделаем pull-request и, как и в прошлый раз, проверим его создание с помощью утилиты `gh`:

```sh
gh pr view --json number,title,author,commits
```

```
{
  "author": {
    "login": "Mimocake"
  },
  "commits": [
    {
      "authoredDate": "2026-03-07T16:31:41Z",
      "authors": [
        {
          "email": "rchekr@yandex.ru",
          "id": "MDQ6VXNlcjg4NTg0ODYw",
          "login": "Mimocake",
          "name": "Mimocake"
        }
      ],
      "committedDate": "2026-03-07T16:31:41Z",
      "messageBody": "",
      "messageHeadline": "changed code style",
      "oid": "0c61fbe7eda89afec7299c556e6f3c4f32d4af45"
    }
  ],
  "number": 2,
  "title": "part 3"
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
[main a053462] translated comments
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
Writing objects: 100% (3/3), 443 bytes | 443.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/Mimocake/TIMP-lab02
   e122c12..a053462  main -> main
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
    "login": "Mimocake"
  },
  "commits": [
    {
      "authoredDate": "2026-03-07T16:31:41Z",
      "authors": [
        {
          "email": "rchekr@yandex.ru",
          "id": "MDQ6VXNlcjg4NTg0ODYw",
          "login": "Mimocake",
          "name": "Mimocake"
        }
      ],
      "committedDate": "2026-03-07T16:31:41Z",
      "messageBody": "",
      "messageHeadline": "changed code style",
      "oid": "0c61fbe7eda89afec7299c556e6f3c4f32d4af45"
    }
  ],
  "mergeable": "CONFLICTING",
  "number": 2,
  "title": "part 3"
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

У меня оно и так уже было up-to-date, поэтому я получил следующий вывод:

```
From https://github.com/Mimocake/TIMP-lab02
 * branch            main       -> FETCH_HEAD
Already up to date.
```

Дальше переключаемся на другую ветку и пытаемся ее перебазировать:

```sh
git checkout patch2
git rebase main
```

На что гит выведет следующее сообщение:
```
Auto-merging hello_world.cpp
CONFLICT (content): Merge conflict in hello_world.cpp
error: could not apply 0c61fbe... changed code style
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply 0c61fbe... changed code style
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
>>>>>>> 0c61fbe (changed code style)
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
[detached HEAD cceff5a] changed code style and translated comments
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
Writing objects: 100% (3/3), 466 bytes | 466.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/Mimocake/TIMP-lab02
 + 0c61fbe...cceff5a patch2 -> patch2 (forced update)
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
    "login": "Mimocake"
  },
  "commits": [
    {
      "authoredDate": "2026-03-07T16:31:41Z",
      "authors": [
        {
          "email": "rchekr@yandex.ru",
          "id": "MDQ6VXNlcjg4NTg0ODYw",
          "login": "Mimocake",
          "name": "Mimocake"
        }
      ],
      "committedDate": "2026-03-07T18:10:13Z",
      "messageBody": "",
      "messageHeadline": "changed code style and translated comments",
      "oid": "cceff5a8b18ac71a7e12a995b186f2048bc4ff5a"
    }
  ],
  "mergeable": "MERGEABLE",
  "number": 2,
  "title": "part 3"
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
commit a37547d1e626f4f6e41e9f77499c67655d1522b5 (HEAD -> main, origin/main)
Merge: a053462 cceff5a
Author: Roman Chekrygin <88584860+Mimocake@users.noreply.github.com>
Date:   Sat Mar 7 21:27:39 2026 +0300

    Merge pull request #2 from Mimocake/patch2
    
    part 3 step 9

commit cceff5a8b18ac71a7e12a995b186f2048bc4ff5a (origin/patch2, patch2)
Author: Mimocake <rchekr@yandex.ru>
Date:   Sat Mar 7 19:31:41 2026 +0300

    changed code style and translated comments
```

### P.s.

Ну и также я решил после pull-request'а удалить ветку `patch2` для красоты.
