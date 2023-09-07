# 列表

## list命令

```php
# 读取
list(LENGTH <list> <out-var>)
list(GET <list> <element index> [<index> ...] <out-var>)
list(JOIN <list> <glue> <out-var>)
list(SUBLIST <list> <begin> <length> <out-var>)

# 搜索
list(FIND <list> <value> <out-var>)

# 修改
list(APPEND <list> [<element>...])
list(FILTER <list> {INCLUDE | EXCLUDE} REGEX <regex>)
list(INSERT <list> <index> [<element>...])
list(POP_BACK <list> [<out-var>...])
list(POP_FRONT <list> [<out-var>...])
list(PREPEND <list> [<element>...])
list(REMOVE_ITEM <list> <value>...)
list(REMOVE_AT <list> <index>...)
list(REMOVE_DUPLICATES <list>)
list(TRANSFORM <list> <ACTION> [...])

# 排序
list(REVERSE <list>)
list(SORT <list> [...])
```

## list命令详解

### 读取

```php
list(LENGTH <list> <out-var>)
```
读`list`的长度输出到`out_val`中

```php
list(GET <list> <element index> [<index> ...] <out-var>)
```
将`list`中的第`index`个元素输出`out-val`中，可以输出多个，-1表示倒数第一个元素，-2表示倒数第二个，以此类推。

```php
list(JOIN <list> <glue> <out-var>)
```
将`list`中的元素用`glue`连接起来输出到`out-val`中，如果长度`length`为-1或列表的长度小于`begin` + `length`，那么将列表中从`begin`索引开始的剩余元素返回。

```php
list(SUBLIST <list> <begin> <length> <out-var>)
```
将`list`中从`begin`开始的`length`个元素输出到`out-val`中，组成新的list

### 搜索

```php
list(FIND <list> <value> <out-var>)
```
寻找`list`中的第一个`value`值，并将其下表输出到`out-val`中，没有输出-1

### 修改

```php
list(APPEND <list> [<element>...])
```
将元素`element`追加到列表`list`

```php
list(FILTER <list> {INCLUDE | EXCLUDE} REGEX <regex>)
```
根据模式的匹配结果，将不符合正则表达式`regex`的元素（`INCLUDE`选项）从列表中删除或者将符合正则表达式`regex`的元素从列表中排除（`EXCLUDE`选项）。

```php
list(INSERT <list> <index> [<element>...])
```
将元素`element`插入到列表`list`中`index`的位置

```php
list(POP_BACK <list> [<out-var>...])
```
`out-var`如果未指定输出变量，则仅仅是将原列表的最后一个元素移除。如果指定了输出变量，则会将最后一个元素移入到该变量，并将元素从原列表中移除。如果指定了多个输出变量，则依次将原列的最后一个元素移入到输出变量中，如果输出变量个数大于列表的长度，那么超出部分的输出变量未定义。

```php
list(POP_FRONT <list> [<out-var>...])
```
`out-var`如果未指定输出变量，则仅仅是将原列表的第一个元素移除。如果指定了输出变量，则会将第一个元素移入到该变量，并将元素从原列表中移除。如果指定了多个输出变量，则依次将原列的第一个元素移入到输出变量中，如果输出变量个数大于列表的长度，那么超出部分的输出变量未定义。

```php
list(PREPEND <list> [<element>...])
```
将元素`element`插入到列表`list`0的位置

```php
list(REMOVE_ITEM <list> <value>...)
```
将所有的`value`从列表`list`中移除

```php
list(REMOVE_AT <list> <index>...)
```
将`index`位置的元素从`list`中移除

```php
list(REMOVE_DUPLICATES <list>)
```
移除列表`list`中的重复元素

```php
list(TRANSFORM <list> <ACTION> [...])
```
选项`ACTION`用于指定应用到列表元素的动作，动作必须在如下中选择一个：`APPEND/PREPEND/TOULOWER/TOUPPER/STRIP/GENEX_STRIP/REPLACE`；选项`SELECTOR`用于指定哪些列表元素会被选择，并执行动作，`SELECTOR`只能从如下中选择一个：`AT/FOR/REGEX`；选项`OUTPUT_VARIABLE`用于指定新的输出变量。`TRANSFORM`命令不会改变列表中元素的个数。

> `ACTION`选项

* `APPEND，PREPEND`：在列表的每个元素后/前插入指定的值。  
`list (TRANSFORM <list> <APPEND|PREPEND> <value> [OUTPUT_VARIABLE <output variable>])`

* `TOUPPER, TOLOWER`：将列表的每一个元素转换成大写/小写。   
`list(TRANSFORM <list> <TOLOWER|TOUPPER> [OUTPUT_VARIABLE <output variable>])`

* `STRIP`：去除列表中每一个元素的头尾空格。
`list(TRANSFORM <list> STRIP [OUTPUT_VARIABLE <output variable>])`

* `GENEX_STRIP`：裁剪列表中任何为产生表达式的元素从下面的例子可以看出，虽然列表中是产生表达式的元素被清除了，但是实际列表的长度没有变化，产生表达式的位置被空元素占据了（TRANSFROM命令不会改变列表元素的个数）。   
`list(TRANSFORM <list> GENEX_STRIP [OUTPUT_VARIABLE <output variable>])`   
```php
# CMakeLists.txt
cmake_minimum_required (VERSION 3.12.2)
project (list_cmd_test)
set (list_test a b c d $<STREQUAL:"a","a"> $<STREQUAL:"a","b">)
list (LENGTH list_test len)
message (">>> TRANSFORM list: ${list_test}, len=${len}")
list (TRANSFORM list_test GENEX_STRIP OUTPUT_VARIABLE list_test_out)
list (TRANSFORM list_test GENEX_STRIP) 
list (LENGTH list_test len_new)
message (">>> TRANSFORM-STRIP: ${list_test} --- ${list_test_out}, new len = ${len_new}")
```
```term
>>> TRANSFORM list: a;b;c;d;$<STREQUAL:"a","a">;$<STREQUAL:"a","b">, len = 6
>>> TRANSFORM-STRIP: a;b;c;d;; --- a;b;c;d;;, new len = 6
```

* `REPLACE`：尽可能多的查找（用正则表达式`regular_expression`匹配）并替换（用`replace_expression`替换）列表中符合条件的元素。   
`list(TRANSFORM <list> REPLACE <regular_expression> <replace_expression> [OUTPUT_VARIABLE<output variable>])`

> `SELECTOR`选项
* `AT`：指定元素的索引（可以是多个）。   
`list(TRANSFORM <list> <ACTION> AT <index> [<index> ...] [OUTPUT_VARIABLE <output variable>])`

* `FOR`：指定元素的范围和迭代的步长。   
`list(TRANSFORM <list> <ACTION> FOR <start> <stop> [<step>] [OUTPUT_VARIABLE <output variable>])`

* REGEX：指定正则表达式，符合正则表达式的元素才会被处理。   
`list(TRANSFORM <list> <ACTION> REGEX <regular_expression>[OUTPUT_VARIABLE <output variable>])`

### 排序
```php
list(REVERSE <list>)
```
反转列表

```php
list(SORT <list> [COMPARE <compare>] [CASE <case>] [ORDER <order>])
```
对列表进行排序   
* `COMPARE`：指定排序方法。有如下几种值可选：
  * `STRING`:按照字母顺序进行排序，为默认的排序方法；
  * `FILE_BASENAME`：如果是一系列路径名，会使用basename进行排序；
  * `NATURAL`：使用自然数顺序排序。
* `CASE`：指明是否大小写敏感。有如下几种值可选：
  * `SENSITIVE`:按照大小写敏感的方式进行排序，为默认值；
  * `INSENSITIVE`：按照大小写不敏感方式进行排序。
* `ORDER`：指明排序的顺序。有如下几种值可选：
  * `ASCENDING`:按照升序排列，为默认值；
  * `DESCENDING`：按照降序排列。



```php
cmake_minimum_required(VERSION 3.26 FATAL_ERROR)

project(test)

set(in_val aaa bbb ccc ddd)

# 读取
list(LENGTH in_val out_val)
message("长度: ${out_val}")

list(GET in_val 1 2 out_val)
message("获取: ${out_val}")

list(JOIN in_val - out_val)
message("拼接: ${out_val}")

list(SUBLIST in_val 0 2 out_val)
message("获取子列表: ${out_val}")

# 搜索
list(FIND in_val ddd out_val)
message("搜索: ${out_val}")

# 修改
list(APPEND in_val eee fff)
message("添加元素: ${in_val}")

set(filter_val a b c d e 1 2 3 4 5)
message("过滤前：${filter_val}")
list(FILTER filter_val INCLUDE REGEX [a-z])
message("INCLUDE过滤：${filter_val}")
list(APPEND filter_val 1 2 3 4 5)
list(FILTER filter_val EXCLUDE REGEX [a-z])
message("EXCLUDE过滤：${filter_val}")

list(INSERT in_val 5 111)
message("插入元素: ${in_val}")

list(POP_BACK in_val out_val_1 out_val_2)
message("第一次移除的元素：${out_val_1}")
message("第二次移除的元素：${out_val_2}")
message("连续两次移除最后一个元素：${in_val}")

list(POP_FRONT in_val out_val_1 out_val_2)
message("第一次移除的元素：${out_val_1}")
message("第二次移除的元素：${out_val_2}")
message("连续两次移除第一个元素：${in_val}")

list(PREPEND in_val AAA BBB)
message("PREPEND操作后：${in_val}")

list(PREPEND in_val AAA BBB)
message("移除前：${in_val}")
list(REMOVE_ITEM in_val AAA BBB)
message("REMOVE_ITEM移除AAA BBB：${in_val}")
list(PREPEND in_val AAA BBB AAA BBB)
list(REMOVE_AT in_val 4 5 6)
message("REMOVE_ITEM移除4,5,6位置：${in_val}")

# 排序
set (list_test 3 1 1.1 10.1 3.4 9)
list (SORT list_test)
message ("以字母顺序，按照大小写敏感方式，升序排列: ${list_test}")
list (SORT list_test COMPARE NATURAL ORDER DESCENDING)
message ("以自然顺序，降序排列: ${list_test}")
```

```term
zhaohaifei@asdfasdfsa:/mnt/d/WSL/CMakeLearn/build$ cmake ..
长度: 4
获取: bbb;ccc
拼接: aaa-bbb-ccc-ddd
获取子列表: aaa;bbb
搜索: 3
添加元素: aaa;bbb;ccc;ddd;eee;fff
过滤前：a;b;c;d;e;1;2;3;4;5
INCLUDE过滤：a;b;c;d;e
EXCLUDE过滤：1;2;3;4;5
插入元素: aaa;bbb;ccc;ddd;eee;111;fff
第一次移除的元素：fff
第二次移除的元素：111
连续两次移除最后一个元素：aaa;bbb;ccc;ddd;eee
第一次移除的元素：aaa
第二次移除的元素：bbb
连续两次移除第一个元素：ccc;ddd;eee
PREPEND操作后：AAA;BBB;ccc;ddd;eee
移除前：AAA;BBB;AAA;BBB;ccc;ddd;eee
REMOVE_ITEM移除AAA BBB：ccc;ddd;eee
REMOVE_ITEM移除4,5,6位置：AAA;BBB;AAA;BBB
以字母顺序，按照大小写敏感方式，升序排列: 1;1.1;10.1;3;3.4;9
以自然顺序，降序排列: 10.1;9;3.4;3;1.1;1
-- Configuring done (0.0s)
-- Generating done (0.0s)
-- Build files have been written to: /mnt/d/WSL/CMakeLearn/build
```