###Libebt简介
Libebt是一个轻量级异步网络库, 可以用于构建小型高性能的网络应用服务。

###安装/使用

1.环境要求

Libebt目前只支持Linux环境上使用，而且它的异步事件操作用使了linux的epoll接口， 所以需要linux内核2.6以上。

2.安装

下载源码包，切换源码目录：`make`。

###异步IO
```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/time.h>
#include <ebt.h>

int main(void)
{
	struct timeval tv = {5, 0};
	char buf[128] = "hello, world!";

	void (*cb)(short, void*);
	void (*tcb)(short, void*);	

	struct eb_t ebt = ebt_new(E_READ | E_WRITE | E_TIMER);

    //IO事件
	struct ev *ev_io = ev_read(0, cb, buf);
    //定时器
	struct ev *ev_t = ev_timer(&tv, tcb, buf);

	ev_attach(ebt, ev_io);
	ev_attach(ebt, ev_t);

	ebt_loop(ebt);

	ebt_free(ebt);

	return 0;
}

void cb(short num, void *arg)
{
	printf("num:%d, buf:%s\n", num, (char*)arg);
}

void tcb(short num, void *arg)
{
	printf("buf:%s\n", (char*)arg);
}
```
