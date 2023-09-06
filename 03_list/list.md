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
```