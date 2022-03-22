## Homework

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.

1. Создаём CMakeLists.txt для библиотеки formatter в директории ~/formatter_lib
```bash
$ cat >CMakeLists.txt <<EOF
> cmake_minimum_required(VERSION 3.10)
> 
> set(CMAKE_CXX_STANDARD 11)
> set(CMAKE_CXX_STANDARD_REQUIRED True)
> 
> project(formatter)
> 
> add_library(formatter STATIC formatter.cpp formatter.h)
> EOF
```

2. Собираем библиотеку formatter

```bash
$ cmake -H. -B_build && cmake --build _build
-- The C compiler identification is GNU 9.4.0
-- The CXX compiler identification is GNU 9.4.0
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
-- Build files have been written to: /home/super_pro_hack_man/jo22i/workspace/reports/lab03/formatter_lib/_build
Scanning dependencies of target formatter
[ 50%] Building CXX object CMakeFiles/formatter.dir/formatter.cpp.o
[100%] Linking CXX static library libformatter.a
[100%] Built target formatter
```

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.

1. Создаём CMakeLists.txt для библиотеки formatter_ex в директории ~/formatter_ex_lib

```bash
$ cat > CMakeLists.txt <<EOF
> cmake_minimum_required(VERSION 3.10)
> 
> project(formatter_ex)
> 
> set(CMAKE_CXX_STANDARD 11)
> set(CMAKE_CXX_STANDARD_REQUIRED True)
> 
> include_directories("../formatter_lib")
> 
> add_library(formatter_ex STATIC formatter_ex.cpp formatter_ex.h)
> 
> target_link_libraries(formatter_ex formatter)
> EOF
```

2. Собираем библиотеку formatter_ex

```bash
$ cmake -H. -B_build && cmake --build _build
-- The C compiler identification is GNU 9.4.0
-- The CXX compiler identification is GNU 9.4.0
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
-- Build files have been written to: /home/super_pro_hack_man/jo22i/workspace/reports/lab03/formatter_ex_lib/_build
Scanning dependencies of target formatter_ex
[ 50%] Building CXX object CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o
[100%] Linking CXX static library libformatter_ex.a
[100%] Built target formatter_ex
```

### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*

1. Создаём CMakeLists.txt для hello_world в директории ~/hello_world_application

```bash
$ cat >CMakeLists.txt <<EOF
> cmake_minimum_required(VERSION 3.10)
> 
> project(hello_world)
> 
> set(CMAKE_CXX_STANDARD 11)
> set(CMAKE_CXX_STANDARD_REQUIRED True)
> 
> include_directories("../formatter_ex_lib")
> include_directories("../formatter_lib")
> 
> add_library(formatter STATIC "../formatter_lib/formatter.cpp" "../formatter_lib/formatter.h")
```

2. Собираем файл hello_world

```bash
$ cmake -H. -B_build && cmake --build _build
-- The C compiler identification is GNU 9.4.0
-- The CXX compiler identification is GNU 9.4.0
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
-- Build files have been written to: /home/super_pro_hack_man/jo22i/workspace/reports/lab03/hello_world_application/_build
Scanning dependencies of target formatter
[ 16%] Building CXX object CMakeFiles/formatter.dir/home/super_pro_hack_man/jo22i/workspace/reports/lab03/formatter_lib/formatter.cpp.o
[ 33%] Linking CXX static library libformatter.a
[ 33%] Built target formatter
Scanning dependencies of target formatter_ex
[ 50%] Building CXX object CMakeFiles/formatter_ex.dir/home/super_pro_hack_man/jo22i/workspace/reports/lab03/formatter_ex_lib/formatter_ex.cpp.o
[ 66%] Linking CXX static library libformatter_ex.a
[ 66%] Built target formatter_ex
Scanning dependencies of target hello_world
[ 83%] Building CXX object CMakeFiles/hello_world.dir/hello_world.cpp.o
[100%] Linking CXX executable hello_world
[100%] Built target hello_world
```

3. Перемещаемся в директорию ~/solver_lib и создаём CMakeLists.txt для статической библиотеки solver

```bash
$ cat >CMakeLists.txt <<EOF
> cmake_minimum_required(VERSION 3.10)
> 
> project(solver)
> 
> set(CMAKE_CXX_STANDARD 11)
> set(CMAKE_CXX_STANDARD_REQUIRED True)
> 
> add_library(solver STATIC solver.cpp solver.h)
> EOF
```

4. Для корректной работы CMake-а и совместимости со стандартом С++17 изменяем файл solver.cpp

```bash
#include "solver.h"

#include <stdexcept>
#include <math.h>

void solve(float a, float b, float c, float& x1, float& x2)
{
    float d = (b * b) - (4 * a * c);

    if (d < 0)
    {
        throw std::logic_error{"error: discriminant < 0"};
    }

    x1 = (-b - sqrt(d)) / (2 * a);
    x2 = (-b + sqrt(d)) / (2 * a);
}
```

5. Собираем библиотеку solver

```bash
$ cmake -H. -B_build && cmake --build _build
-- The C compiler identification is GNU 9.4.0
-- The CXX compiler identification is GNU 9.4.0
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
-- Build files have been written to: /home/super_pro_hack_man/jo22i/workspace/reports/lab03/solver_lib/_build
Scanning dependencies of target solver
[ 50%] Building CXX object CMakeFiles/solver.dir/solver.cpp.o
[100%] Linking CXX static library libsolver.a
[100%] Built target solver
```

6. Перемещаемся в директорию ~/solver_application и создаём CMakeLists.txt для исполняемого файла solver_app

```bash
$ cat >CmakeLists.txt <<EOF
> cmake_minimum_required(VERSION 3.10)
> 
> project(solver_app)
> 
> include_directories("../formatter_lib")
> include_directories("../formatter_ex_lib")
> include_directories("../solver_lib")
> 
> add_library(formatter STATIC "../formatter_lib/formatter.cpp" "../formatter_lib/formatter.h")
> add_library(formatter_ex STATIC "../formatter_lib_ex/formatter_ex.cpp" "../formatter_ex_lib/formatter_ex.h")
> add_library(solver STATIC "../solver_lib/solver.cpp" "../solver_lib/solver.h")
> 
> add_executable(solver_app equation.cpp)
> 
> target_link_libraries(solver_app formatter_ex formatter solver)
> EOF
```

7. Собираем файл solver_app

```bash
$ cmake -H. -B_build && cmake --build _build
-- The C compiler identification is GNU 9.4.0
-- The CXX compiler identification is GNU 9.4.0
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
-- Build files have been written to: /home/super_pro_hack_man/jo22i/workspace/reports/lab03/solver_application/_build
Scanning dependencies of target solver
[ 12%] Building CXX object CMakeFiles/solver.dir/home/super_pro_hack_man/jo22i/workspace/reports/lab03/solver_lib/solver.cpp.o
[ 25%] Linking CXX static library libsolver.a
[ 25%] Built target solver
Scanning dependencies of target formatter_ex
[ 37%] Building CXX object CMakeFiles/formatter_ex.dir/home/super_pro_hack_man/jo22i/workspace/reports/lab03/formatter_ex_lib/formatter_ex.cpp.o
[ 50%] Linking CXX static library libformatter_ex.a
[ 50%] Built target formatter_ex
Scanning dependencies of target formatter
[ 62%] Building CXX object CMakeFiles/formatter.dir/home/super_pro_hack_man/jo22i/workspace/reports/lab03/formatter_lib/formatter.cpp.o
[ 75%] Linking CXX static library libformatter.a
[ 75%] Built target formatter
Scanning dependencies of target solver_app
[ 87%] Building CXX object CMakeFiles/solver_app.dir/equation.cpp.o
[100%] Linking CXX executable solver_app
[100%] Built target solver_app
```
