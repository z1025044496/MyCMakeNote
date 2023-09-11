# 数学计算

## 命令格式
```php
math(EXPR outVar mathExpr [OUTPUT_FORMAT format])
```

通过CMake变量结合数学运算符组成`mathExpr`，然后计算结果会保存到`outVar`中。   
OUTPUT_FORMAT 是可选参数，代表输出结果的格式，可以是 HEXADECIMAL：输出 16 进制结果，DECIMAL：输出 10 进制结果。

例子：
```php
set(x 3)
set(y 7)
math(EXPR zDec "(${x}+${y}) * 2")
message("decimal = ${zDec}")

# Requires CMake 3.13 or later for HEXADECIMAL
math(EXPR zHex "(${x}+${y}) * 2" OUTPUT_FORMAT HEXADECIMAL)
message("hexadecimal = ${zHex}")
```

运行结果：
```term
zhaohaifei@asdfasdfsa:/mnt/d/WSL/CMakeLearn/build$ cmake ..
decimal = 20
hexadecimal = 0x14
-- Configuring done (0.0s)
-- Generating done (0.0s)
-- Build files have been written to: /mnt/d/WSL/CMakeLearn/build
```