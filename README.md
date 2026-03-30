# lab03 Панин ИУ8-23
## Сначала напишем корневой CMakeLists
```
$ subl CMakeLists.txt

cmake_minimum_required(VERSION 3.4)
project(lab03)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_subdirectory(formatter_lib)
add_subdirectory(formatter_ex_lib)
add_subdirectory(solver_lib)
add_subdirectory(hello_world)
add_subdirectory(solver)
```

## Задание 1
Вам поручили перейти на систему автоматизированной сборки CMake. Исходные файлы находятся в директории formatter_lib. В этой директории находятся файлы для статической библиотеки formatter. Создайте CMakeList.txt в директории formatter_lib, с помощью которого можно будет собирать статическую библиотеку formatter.
### Напишем CMakeLists.txt для formatter_lib
```
$ cd formatter_lib
$ subl CMakeLists.txt

cmake_minimum_required(VERSION 3.4)
project(formatter_lib)

add_library(formatter STATIC formatter.cpp)

target_include_directories(formatter PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
```
### Соберем эту библиотеку
```
$ cmake -H. -B_build

-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/vanish/vanishhhhh/workspace/projects/lab03/_build

$ cmake --build _build --target formatter

[ 50%] Building CXX object formatter_lib/CMakeFiles/formatter.dir/formatter.cpp.o
[100%] Linking CXX static library libformatter.a
[100%] Built target formatter
```

## Задание 2
У компании "Formatter Inc." есть перспективная библиотека, которая является расширением предыдущей библиотеки. Т.к. вы уже овладели навыком созданием CMakeList.txt для статической библиотеки formatter, ваш руководитель поручает заняться созданием CMakeList.txt для библиотеки formatter_ex, которая в свою очередь использует библиотеку formatter.

### Напишем CMakeLists.txt для formatter_ex_lib
```
$ cd formatter_ex_lib
$ subl CMakeLists.txt

cmake_minimum_required(VERSION 3.4)
project(formatter_ex_lib)

add_library(formatter_ex STATIC formatter_ex.cpp)

target_include_directories(formatter_ex PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

target_link_libraries(formatter_ex PUBLIC formatter)
```
### Соберем эту библиотеку
```
$ cmake --build _build --target formatter_ex

Consolidate compiler generated dependencies of target formatter
[ 50%] Built target formatter
[ 75%] Building CXX object formatter_ex_lib/CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o
[100%] Linking CXX static library libformatter_ex.a
[100%] Built target formatter_ex
```

## Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек. Чтобы продемонстрировать как работать с библиотекой formatter_ex, вам необходимо создать два CMakeList.txt для двух простых приложений:

hello_world, которое использует библиотеку formatter_ex;
solver, приложение которое испольует статические библиотеки formatter_ex и solver_lib.

### Напишем CMakeLists.txt для hello_world
```
$ cd hello_world
$ subl CMakeLists.txt

cmake_minimum_required(VERSION 3.4)
project(hello_world)

add_executable(hello_world hello_world.cpp)

target_link_libraries(hello_world formatter_ex)

```
### Проверим работу hello_world
```
$ cmake --build _build --target hello_world

[ 33%] Built target formatter
Consolidate compiler generated dependencies of target formatter_ex
[ 66%] Built target formatter_ex
[ 83%] Building CXX object hello_world/CMakeFiles/hello_world.dir/hello_world.cpp.o
[100%] Linking CXX executable hello_world
[100%] Built target hello_world

$ ./_build/hello_world/hello_world

-------------------------
hello, world!
-------------------------
```

### Напишем CMakeLists.txt сначала для solver_lib, а затем для solver
```
$ cd solver_lib
$ subl CMakeLists.txt

cmake_minimum_required(VERSION 3.4)
project(solver_lib)

add_library(solver_lib STATIC solver.cpp)

target_include_directories(solver_lib PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
```
```
$ cd solver
$ subl CMakeLists.txt

cmake_minimum_required(VERSION 3.4)
project(solver)

add_executable(solver equation.cpp)

target_link_libraries(solver formatter_ex solver_lib)
```
### Теперь проверим работу solver
```
$ cmake --build _build --target solver

Consolidate compiler generated dependencies of target solver_lib
[ 25%] Built target solver_lib
[ 50%] Built target formatter
[ 75%] Built target formatter_ex
[ 87%] Building CXX object solver/CMakeFiles/solver.dir/equation.cpp.o
[100%] Linking CXX executable solver
[100%] Built target solver

$ echo "1 5 6" | ./_build/solver/solver

-------------------------
x1 = -3.000000
-------------------------
-------------------------
x2 = -2.000000
-------------------------
```
