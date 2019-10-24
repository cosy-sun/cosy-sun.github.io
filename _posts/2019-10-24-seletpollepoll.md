---
layout: post
---
<font size = 5>
<B>
    <center>select, poll, epoll</center>
   
</B>
</font>

<font size = 3>
<B>

linux提供了select, poll, epoll接口来实现io复用

- [select](./文件描述符.md)
- poll,与select 不同, 通过一个pollfd数组向内核传递需要关注的事件, 没有描述符的限制,对应内核中的sys_poll, 对每个描述符进行poll, 相比处理fdset, poll效率高, 返回后, 检查每个元素的revents值, 判断事件是否发生,
- epoll, 不是通过轮询,而是通过在等待的描述符上注册回调函数, 当事件发生之后, 回调函数负责把发生的事件存储在就绪事件链表中,最后写到用户空间,

    epoll返回后, 该参数指向的缓冲区中即为发生的事件, 对缓冲区中每个元素进行处理,即可, 不需要向select, poll那样进行轮询检查,
   
</B>
</font>