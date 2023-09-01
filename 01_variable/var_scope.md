# 变量作用域

在CMake中，通常只有在使用`add_subdirectory()`命令或者定义函数的时候产生新的作用域。自CMake 3.25开始，可以使用`block()`在任意位置产生新的作用域。

CMake的缓存变量和环境变量都是全局的，只有CMake普通变量的作用域受到不同CMake命令的影响。

我们在定义CMake普通变量的时候，如果没有`PARENT_SCOPE`选项，那该变量的作用域就在当前的CMakeLists.txt中或者在当前的函数，或者当前的`block()`中。

1. `set()`不指定变量作用域时，该变量的作用域就在当前的CMakeLists.txt中或者在当前的函数
2. 内层嵌套函数会依次向外搜索指定被引用的变量。

实例：
```php
set(HELLO "HELLO")

function(insert_foo)
    message("内嵌函数内部引用外部变量:${HELLO} ${WORLD}") 
endfunction(insert_foo)


function(foo)
    set(WORLD "WORLD")
    message("函数内部引用外部变量:${HELLO} ${WORLD}")
    insert_foo()
endfunction()

foo()

message("HELLO ${HELLO} ${WORLD}")
```

结果：
```term
[cmake] -- Configuring on/for Linux
[cmake] -- Configuring on/for LITTLE_ENDIAN
[cmake] 函数内部引用外部变量:HELLO WORLD
[cmake] 内嵌函数内部引用外部变量:HELLO WORLD
[cmake] HELLO HELLO 
[cmake] -- Configuring done (0.0s)
[cmake] -- Generating done (0.1s)
[cmake] -- Build files have been written to: /mnt/d/WSL/test/build
```

> add_subdirectory(subdir)

这个作用域叫做目录作用域。子目录的CMakeLists.txt会将父目录的所有变量拷贝到当前CMakeLists.txt中，当前CMakeLists.txt中的变量的作用域仅在当前子目录有效。  
目录作用域有两个特点：向下有效，值拷贝

例子：

* 目录结构
```term
├── CMakeLists.txt
└── subDir
├───├── CMakeLists.txt
└───└── Makefile
```
* 父目录下的CMakeLists.txt
```php
set(parent_var1 "parent_var1")
message("dir-parent_var1:${parent_var1}")
add_subdirectory(subdir)
message("dir-parent_var1:${parent_var1}")
```
* 子目录下的CMakeLists.txt
```php
set(subdir_var1 "subdir_var1")
message("subdir-subdir_var1:${subdir_var1}")
message("subdir-parent_var1:${parent_var1}")
set(parent_var1 "修改后的parent_var1")
message("subdir-parent_var1:${parent_var1}")
```

结果：
```term
dir-parent_var1:parent_var1
subdir-subdir_var1:subdir_var1
subdir-parent_var1:parent_var1
subdir-parent_var1:修改后的parent_var1
dir-parent_var1:parent_var1
-- Configuring done (0.0s)
-- Generating done (0.1s)
-- Build files have been written to: /mnt/d/WSL/CMakeLearn/build
```