# 字符串

## `string()`命令

```php
# 字符串查找和替换
  string(FIND <string> <substring> <out-var> [...])
  string(REPLACE <match-string> <replace-string> <out-var> <input>...)
  string(REGEX MATCH <match-regex> <out-var> <input>...)
  string(REGEX MATCHALL <match-regex> <out-var> <input>...)
  string(REGEX REPLACE <match-regex> <replace-expr> <out-var> <input>...)

# 操作字符串
  string(APPEND <string-var> [<input>...])
  string(PREPEND <string-var> [<input>...])
  string(CONCAT <out-var> [<input>...])
  string(JOIN <glue> <out-var> [<input>...])
  string(TOLOWER <string> <out-var>)
  string(TOUPPER <string> <out-var>)
  string(LENGTH <string> <out-var>)
  string(SUBSTRING <string> <begin> <length> <out-var>)
  string(STRIP <string> <out-var>)
  string(GENEX_STRIP <string> <out-var>)
  string(REPEAT <string> <count> <out-var>)

# 字符串比较
  string(COMPARE <op> <string1> <string2> <out-var>)

# 计算字符串的 hash 值
  string(<HASH> <out-var> <input>)

# 生成字符串
  string(ASCII <number>... <out-var>)
  string(HEX <string> <out-var>)
  string(CONFIGURE <string> <out-var> [...])
  string(MAKE_C_IDENTIFIER <string> <out-var>)
  string(RANDOM [<option>...] <out-var>)
  string(TIMESTAMP <out-var> [<format string>] [UTC])
  string(UUID <out-var> ...)

# json 相关的字符串操作
  string(JSON <out-var> [ERROR_VARIABLE <error-var>]
         {GET | TYPE | LENGTH | REMOVE}
         <json-string> <member|index> [<member|index> ...])
  string(JSON <out-var> [ERROR_VARIABLE <error-var>]
         MEMBER <json-string>
         [<member|index> ...] <index>)
  string(JSON <out-var> [ERROR_VARIABLE <error-var>]
         SET <json-string>
         <member|index> [<member|index> ...] <value>)
  string(JSON <out-var> [ERROR_VARIABLE <error-var>]
         EQUAL <json-string1> <json-string2>)
```

## `string()`命令功能讲解

### 查找

> 字符查找

```php
string(FIND inputString subString outVar [REVERSE])
```

* 在 inputString 中查找 subString，将查找到的索引存在 outVar 中，索引从 0 开始。
* 如果没有 REVERSE 选项，则保存第一个查找到的索引，否则保存最后一个查找到的索引。
* 如果没有找到则保存 -1。
* 需要注意的是，`string(FIND)`将所有字符串都作为 ASCII 字符，outVar 中存储的索引也会以字节为单位计算，因此包含多字节字符的字符串可能会导致意想不到的结果。

例子：
```php
string(FIND abcdefabcdef def fwdIndex)
string(FIND abcdefabcdef def revIndex REVERSE)
message("fwdIndex = ${fwdIndex}\n"
        "revIndex = ${revIndex}")
```

运行结果：
```term
zhaohaifei@asdfasdfsa:/mnt/d/WSL/CMakeLearn/build$ cmake ..
fwdIndex = 3
revIndex = 9
-- Configuring done (0.0s)
-- Generating done (0.0s)
-- Build files have been written to: /mnt/d/WSL/CMakeLearn/build
```
### 替换

```php
string(REPLACE matchString replaceWith outVar input...)
```

* 将`input`中所有匹配`matchString`的都用`replaceWith`替换，并将结果保存到 `outVar`中。
* 如果有多个`input`，它们是直接连接在一起的，没有任何分隔符。
  * 这有时可能会有问题，所以通常建议只提供一个`input`字符串。

例子：
```php
set(in_val "1234567zzz7654321")
message("in_val  = ${in_val}")
string(REPLACE "zzz" "898" out_val ${in_val})
message("out_val = ${out_val}")
```

运行结果：
```term
zhaohaifei@asdfasdfsa:/mnt/d/WSL/CMakeLearn/build$ cmake ..
in_val  = 1234567zzz7654321
out_val = 12345678987654321
-- Configuring done (0.0s)
-- Generating done (0.0s)
-- Build files have been written to: /mnt/d/WSL/CMakeLearn/build
```

### 正则表达式查找和替换

```php
string(REGEX MATCH    regex outVar input...)
string(REGEX MATCHALL regex outVar input...)
string(REGEX REPLACE  regex replaceWith outVar input...)
```

* `input`字符串同样会在开始匹配正则表达式前进行串联。
* `MATCH`只查找第一个匹配的字符串，并保存到`outVar`中。
* `MATCHALL`会查找所有匹配的字符串，并保存到`outVar` 中，如果匹配到多个，`outVar`将是一个列表。
* `REPLACE`会将每一个匹配到的字符串用`replaceWith`替换后，将替换后的完整字符串放到`outVar`中。

例子：
```php
string(REGEX MATCH    "[ace]"           matchOne abcdefabcdef)
string(REGEX MATCHALL "[ace]"           matchAll abcdefabcdef)
string(REGEX REPLACE  "([de])" "X\\1Y"  replVar1 abc def abcdef)
string(REGEX REPLACE  "([de])" [[X\1Y]] replVar2 abcdefabcdef)
message("matchOne = ${matchOne}\n"
        "matchAll = ${matchAll}\n"
        "replVar1 = ${replVar1}\n"
        "replVar2 = ${replVar2}")
```

运行结果
```term
zhaohaifei@asdfasdfsa:/mnt/d/WSL/CMakeLearn/build$ cmake ..
matchOne = a
matchAll = a;c;e;a;c;e
replVar1 = abcXdYXeYfabcXdYXeYf
replVar2 = abcXdYXeYfabcXdYXeYf
-- Configuring done (0.0s)
-- Generating done (0.0s)
-- Build files have been written to: /mnt/d/WSL/CMakeLearn/build
```

### 截取字符串

```php
string(SUBSTRING input index length outVar)
```
* 将`input`字符串从`index`处截取`length`长度放到`outVar`中。
* 如果`length`为*\-1*的话，将从`index`到`input`结尾的字符串串保存到`outVar`中。

### 其他常用操作

```php
string(LENGTH  input outVar)
string(TOLOWER input outVar)
string(TOUPPER input outVar)
string(STRIP   input outVar)
```

* `LENGTH`获取`input`字符串的长度，保存到`outVar`中。
* `TOLOWER`将`input`字符串转换成小写保存到`outVar`中。
* `TOUPPER`将`input`字符串转换成大写保存到`outVar`中。
* `STRIP`将`input`字符串的头尾空格去掉。