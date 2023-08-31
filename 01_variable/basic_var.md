# 普通变量

> CMake变量

* CMake 将所有的变量的值视为字符串
* 当给出变量的值不包含空格的时候，可以不使用引号，但建议都加上引号，不然一些奇怪的问题很难调试。
* CMake 使用空格或者分号作为字符串的分隔符
* CMake 中想要获取变量的值，和shell脚本一样，采用`${var}`形式。
* 使用 CMake 变量前，不需要这个变量已经定义了，如果使用了未定义的变量，那么它的值是一个空字符串。
  * 默认情况下使用未定义的变量不会有警告信息，但是可以通过cmake的`-warn-uninitialized`选项启用警告信息。
  * 使用未定义的变量非常常见，如果出现问题也不一定是因为变量未定义导致的，所以cmake的`-warn-uninitialized`选项用处很有限。
* 变量的值可以包含换行，也可以包含引号，不过需要转义。

> CMake定义变量
```php
set(varName value... [PARENT_SCOPE])
```

* 第一个参数是必须的，代表要定义的变量的变量名。
* 第二个参数也是必须的，代表要定义的变量的值，根据上述 CMake 变量的约定，我们知道这里的 value 应该是一个字符串，而且不管有没有空格，都建议用引号引起来。二个参数也可以是一个以空格或者分号隔开的字符串，这样定义的变量将是一个列表
* 第三个参数是一个可选参数，意思是定义的这个变量的作用域属于父作用域

> 例子

```php
set(CPR_TOP_DIR "/root/workspace/code/cpr")

set(CPR_BASE_SOURCE "a.cpp;b.cpp")

set(CPR_CORE_SOURCE core1.cpp core2.cpp)

set(CPR_BUILD_CMD [[
#!/bin/bash

cmake -S . -B build
cmake --build build
]])

set(shellScript [=[
#!/bin/bash
[[ -n "${USER}" ]] && echo "Have USER"
]=])
```

> 取消变量定义

```php
set(myVar)
unset(myVar)
```