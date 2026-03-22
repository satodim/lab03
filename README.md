#Laboratory work 3
## Tutorial
### $ Выполним уже привычные нам команды, экспортирует имя пользователя, сменим дирректорию, активируем скрипты

```sh
$ export GITHUB_USERNAME=<имя_пользователя>

$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
$ source scripts/activate
```
### Клонируем вторую лабу в новый репозиторий и перепривяжем origin

```sh
$ git clone https://github.com/${GITHUB_USERNAME}/lab02.git projects/lab03
$ cd projects/lab03
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab03.git
```
### Компилируем файл *print.cpp* получаем *print.o* и проеверяем его существование в дирректории
```sh
 g++ -std=c++11 -I./include -c sources/print.cpp
$ ls print.o
```
### Выводим строки с символами *print*, архивируем *print.o* и получаем *print.a*, выводим его тип
```sh
$ nm print.o | grep print
$ ar rvs print.a print.o
$ file print.a
```
### Делаем почти то же самое с *example1*, линкуем его с *print.a* и выводим результат
```sh
++ -std=c++11 -I./include -c examples/example1.cpp
$ ls example1.o
$ g++ example1.o print.a -o example1
$ ./example1 && echo
```
*Вывод*:
```sh
hello
```
### Также линкуем *exzmple2* с *print.a* и выводим *log.txt* в терминал, перед этим с помощью nm выводим таблицу символов объектного файла
```sh
$ g++ -std=c++11 -I./include -c examples/example2.cpp
$ nm example2.o
$ g++ example2.o print.a -o example2
$ ./example2
$ cat log.txt && echo
```
### Очищаем артефакты сборки
``` sh
$ g++ -std=c++11 -I./include -c examples/example2.cpp
$ nm example2.o
$ g++ example2.o print.a -o example2
$ ./example2
$ cat log.txt && echo
```
*Вывод*: 
```sh 
hello
```
### Создаем и заполняем *CmakeLists.txt* с помощью *cat*
``` sh$ cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.4)
project(print)
EOF

$ cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF

$ cat >> CMakeLists.txt <<EOF
add_library(print STATIC \${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)
EOF

$ cat >> CMakeLists.txt <<EOF
include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/include)
EOF
```

### Генерируем систему сборки и загружаем ее в _build
``` sh

$ cmake -H. -B_build
$ cmake --build _build
```
*Вывод1*:
```sh
CMake Deprecation Warning at CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


-- Configuring done (0.0s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vboxuser/satodim/workspace/workspace/projects/lab03/_build
```
*Вывод2*:
```sh
[ 50%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[100%] Linking CXX static library libprint.a
[100%] Built target print
```
### Дополняем *CmakeLists.tst*
```sh
$ cat >> CMakeLists.txt <<EOF

add_executable(example1 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example1.cpp)
add_executable(example2 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example2.cpp)
EOF

$ cat >> CMakeLists.txt <<EOF

target_link_libraries(example1 print)
target_link_libraries(example2 print)
EOF
```
### Делаем сначала полную сборку, а потом по очереди *print*, *example1*, *example2*
``` sh
$ cmake --build _build
$ cmake --build _build --target print
$ cmake --build _build --target example1
$ cmake --build _build --target example2
```
*Вывод потчи везде одинаковый*:
```sh
-- Configuring done (0.0s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vboxuser/satodim/workspace/workspace/projects/lab03/_build
[ 33%] Built target print
[ 50%] Building CXX object CMakeFiles/example1.dir/examples/example1.cpp.o
[ 66%] Linking CXX executable example1
[ 66%] Built target example1
[ 83%] Building CXX object CMakeFiles/example2.dir/examples/example2.cpp.o
[100%] Linking CXX executable example2
[100%] Built target example2
```
*и*:
```sh
[100%] Built target print
```
### Выводим подробную информацию о файле *libprint.a* и запусскаем 2 примера, после чего удаляем *log.txt*
```sh
$ ls -la _build/libprint.a
$ _build/example1 && echo
hello
$ _build/example2
$ cat log.txt && echo
hello
$ rm -rf log.txt
```
*Вывод (1)*:
```sh
rw-rw-r-- 1 vboxuser vboxuser 2246 Mar 22 11:49 _build/libprint.a
```
*Вывод (2)*:
```sh 
hellohello
```
*Вывод (3)*:
``` sh
hellohello
```
### Копируем с удаленного репозитория лабу в tmp и перемещаем от туда CmakeLists.tst, потом удаляем tmp
``` sh
$ git clone https://github.com/tp-labs/lab03 tmp
$ mv -f tmp/CMakeLists.txt .
$ rm -rf tmp
```
*вывод:*
```sh
Cloning into 'tmp'...
remote: Enumerating objects: 91, done.
remote: Counting objects: 100% (30/30), done.
remote: Compressing objects: 100% (9/9), done.
remote: Total 91 (delta 23), reused 21 (delta 21), pack-reused 61 (from 1)
Receiving objects: 100% (91/91), 1.02 MiB | 476.00 KiB/s, done.
Resolving deltas: 100% (41/41), done.
```
### Смотрим содержимое файла CmakeLists.txt и конфигурируем проект с помощью cmake, после собираем и устанавливаем проект и выводим структуру _install
```sh
$ cat CMakeLists.txt
$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
$ cmake --build _build --target install
$ tree _install
```
*Вывод:*
```sh
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_EXAMPLES "Build examples" OFF)

project(print)

add_library(print STATIC ${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)

target_include_directories(print PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)

if(BUILD_EXAMPLES)
  file(GLOB EXAMPLE_SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/examples/*.cpp")
  foreach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
    get_filename_component(EXAMPLE_NAME ${EXAMPLE_SOURCE} NAME_WE)
    add_executable(${EXAMPLE_NAME} ${EXAMPLE_SOURCE})
    target_link_libraries(${EXAMPLE_NAME} print)
    install(TARGETS ${EXAMPLE_NAME}
      RUNTIME DESTINATION bin
    )
  endforeach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
endif()

install(TARGETS print
    EXPORT print-config
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
)

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION include)
install(EXPORT print-config DESTINATION cmake)
```

*Вывод 2:*
```sh
-- Configuring done (0.0s)
-- Generating done (0.0s)
-- Build files have been written to: /home/vboxuser/satodim/workspace/workspace/projects/lab03/_build
```
*Вывод 3:*
``` sh
[100%] Built target print
Install the project...
-- Install configuration: ""
-- Installing: /home/vboxuser/satodim/workspace/workspace/projects/lab03/_install/lib/libprint.a
-- Installing: /home/vboxuser/satodim/workspace/workspace/projects/lab03/_install/include
-- Installing: /home/vboxuser/satodim/workspace/workspace/projects/lab03/_install/include/print.hpp
-- Installing: /home/vboxuser/satodim/workspace/workspace/projects/lab03/_install/cmake/print-config.cmake
-- Installing: /home/vboxuser/satodim/workspace/workspace/projects/lab03/_install/cmake/print-config-noconfig.cmake
```
*Вывод 4:*
``` sh
├── cmake
│   ├── print-config.cmake
│   └── print-config-noconfig.cmake
├── include
│   └── print.hpp
└── lib
    └── libprint.a

4 directories, 4 files
```
### Коммитим и пушим на гитхаб
```sh
$ git add CMakeLists.txt
$ git commit -m"added CMakeLists.txt"
$ git push origin master
```
*Вывод :*
``` sh
Username for 'https://github.com': satodim
Password for 'https://satodim@github.com': 
Enumerating objects: 32, done.
Counting objects: 100% (32/32), done.
Delta compression using up to 2 threads
Compressing objects: 100% (30/30), done.
Writing objects: 100% (32/32), 14.70 KiB | 7.35 MiB/s, done.
Total 32 (delta 3), reused 25 (delta 1), pack-reused 0
remote: Resolving deltas: 100% (3/3), done.
To https://github.com/satodim/lab03.git
 * [new branch]      main -> main
```
## Homework
### Задание 1
*Вам поручили перейти на систему автоматизированной сборки CMake. Исходные файлы находятся в директории formatter_lib. В этой директории находятся файлы для статической библиотеки formatter. Создайте CMakeList.txt в директории formatter_lib, с помощью которого можно будет собирать статическую библиотеку formatter.*

### Выполнение
