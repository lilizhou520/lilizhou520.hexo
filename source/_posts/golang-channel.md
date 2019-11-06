---
title: golang channel
date: 2019-11-05 18:13:11
categories: 
- 后端
tags:
- golang
---
##### channel基本理念
* 默认的非缓冲类型channel 接收和发送数据都是阻塞的
* channel的机制是先进先出
* 有缓冲的channel，发送方会一直阻塞直到数据被拷贝到缓冲区；如果缓冲区已满，则发送方只能在接收方取走数据后才能从阻塞状态恢复

<!--more-->

##### 基本运用
* 创建无缓冲channel
`chreadandwrite :=make(chan int)      
`
* 创建只读channel
`
chonlyread := make(<-chan int)  
`
* 创建只写channel
`
chonlywrite := make(chan<- int) 
`
* struct{}类型的channel,
>它不能被写入任何数据，只有通过close()函数进行关闭操作，才能进行输出操作。。struct类型的channel不占用任何内存
```go
var sig = make(chan struct{}) 
close(sig)
<-sig
```


##### 案例
````go
#!/usr/bin/go
ch :=make(chan int)
ch <- 1
go func() {      
    <-ch      
    fmt.Println("1")
}()
fmt.Println("2")
````
**fatal error: all goroutines are asleep - deadlock!**

*因为我们创建的channel是无缓冲的，即同步的，赋值完成后来不及读取channel，程序就已经阻塞*

##### 解决方案
1. 给channel增加缓冲区，然后在程序的最后让主线程休眠一秒（？？
如果不休眠，程序会优先执行主线程，主线程执行完成后，程序会立即退出，没有多余的时间去执行子线程）
```go
#!/usr/bin/go
ch :=make(chan int,1)
ch <- 1
go func() {
    v := <-ch
    fmt.Println(v)
}()
time.Sleep(1 * time.Second)
fmt.Println("2")
```
2. 把ch<-1这一行代码放到子线程代码的后面，channel在主线程中被赋值后，主线程就会阻塞，直到channel的值在子线程中被取出
````go
#!/usr/bin/go
ch :=make(chan int,1)
go func() {
    v := <-ch
    fmt.Println(v)
}()
ch <- 1
fmt.Println("2")
```



