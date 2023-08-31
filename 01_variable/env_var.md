# 环境变量

cmake 定义环境变量和获取环境变量的值用法和普通变量类似，只需要多加一个 ENV 标识符。

> CMake定义环境变量

```php
set(ENV{PATH} "$ENV{PATH}:/opt/myDir")

message(STATUS "PATH=$ENV{PATH}")
```