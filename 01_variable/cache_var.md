# 缓存变量

> [!NOTE]  
> 与普通变量不同，缓存变量的值可以缓存到CMakeCache.txt文件中，当再次运行cmake时，可以从中获取上一次的值，而不是重新去评估。所以缓存变量的作用域是全局的。

> CMake定义缓存变量

```php
set(varName value... CACHE type "docstring" [FORCE])
```