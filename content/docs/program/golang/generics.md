---
title: "Golang 的泛型"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

# golang 泛型

## 类型限制

用来限制泛型中传递进去的类型，用来过滤掉不满足该所需要类型的类型。

如一个 `Min` 的泛型函数，期待传递进去的类型可能为 `Ordered` 类。

```go
type Unsigned interface {
	~uint | ~uint8 | ~uint16 | ~uint32 | ~uint64 | ~uintptr
}
type Signed interface {
	~int | ~int8 | ~int16 | ~int32 | ~int64
}
type Integer interface {
	Signed | Unsigned
}
type Float interface {
	~float32 | ~float64
}
type Ordered interface {
	Integer | Float | ~string
}
```

`interface{int|string|bool}` 为新添加的语法，表示其中任意一个都可以满足这个 `interface`. `~string` 表示底层类型为 `string` 的所有类型。

从上面的 `interface` 可以看出，其实每一个 `interface` 都定义了一组类型，如 `Float` 定义了 `float32` 和 `float64`.

但目前由于 `golang` 的泛型实现的并不完整，所以 `interface{int|string|bool}` 这样的用户目前只支持没有方法的接口，而像 `interface{string|io.Reader}` 这样的写法是不支持的。

### 补充
在看完 golang 的泛型后，同时去看了下 rust 中的泛型的用法，rust 中使用 `trait` 来限制泛型的行为，类似于 golang 中的 `interface` 都是用来定义一组接口的做法。区别在于 rust 中的 `trait` 可以通过 `trait bound` 进行组合使用，目前的 golang 中的泛型并没有实现。

# 引用
- https://pkg.go.dev/golang.org/x/exp/constraints#Integer
- https://github.com/golang/go/issues/49054
- https://github.com/golang/go/issues/45346#issuecomment-862505803
