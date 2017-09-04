Golang 是通过命名的首字母大小写来区分访问权限的, 首字母大写相当于其他语言的public, 不同包可以访问, 首字母小写或以下划线开头的相当于private, 只能同包内访问

```go
// visibility/test.go
package visibility
 
import "fmt"
 
const PI = 3.145
const pi = 3.14
const _PI = 3.14
 
var P int = 1
var p int = 1
 
func private_function()  {
        fmt.Println("only used in this package!")
}
 
func Public_fuction() {
        fmt.Println("used in anywhere!")
}


// main.go
package main
 
import (
         "visibility"
        "fmt"
)
 
func main() {
        visibility.Public_fuction() //used in anywhere!
        //visibility.private_function() //不能访问私有函数，无法通过编译
        fmt.Println(visibility.P) //1
        //fmt.Println(visibility.p) //不能访问私有变量，无法通过编译
        fmt.Println(visibility.PI) //3.14
        //fmt.Println(visibility.pi) //不能访问私有常量，无法通过编译
        //fmt.Println(visibility._PI) //不能访问私有常量，无法通过编译
}
```

[参考原文](https://www.studygolang.com/articles/6212)
