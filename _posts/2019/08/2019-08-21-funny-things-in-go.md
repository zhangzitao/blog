---
categories: [tech, Dev]
tags: [go]
---

### Type conversion

The const cannot be convered to other type explicitly, but varible can.

```go
const a = -1.23
var b = a
// error: 常量1.23不能被截断舍入到一个整数。
var x = int32(a)
// error: float64类型值不能被隐式转换到int32（类型推断错误）。
var y int32 = b
// ok: z == -1，变量z的类型被推断为int32。
//     z的小数部分将被舍弃。
var z = int32(b)
```
