#Laboratory work 3
## Tutorial
### $ Выполним уже привычные нам команды, экспортирует имя пользователя, сменим дирректорию, активируем скрипты

```sh
$ export GITHUB_USERNAME=<имя_пользователя>

$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
$ source scripts/activate
```
# Клонируем вторую лабу в новый репозиторий и перепривяжем origin

```sh
$ git clone https://github.com/${GITHUB_USERNAME}/lab02.git projects/lab03
$ cd projects/lab03
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab03.git
```
# 
