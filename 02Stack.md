**Stack  ** 栈

1、栈存储的内容

栈中存放任意类型的变量，但必须是auto类型修饰的，即自动类型的局部变量。也就是前面我们接触使用最多的变量。

2、栈存储的特点

随用随开、用完即消。内存的分配和销毁系统自动完成，不需要人工干预。

（FILO   先进后出      LIFO  后进先出）

3、栈溢出

​		局部变量过多，过大， 或 递归层数太多。

现实意义：      

```
递归的问题，本质就是函数压栈。如果利用栈工具，递归的操作，就可以完全可以用循环加栈的组合方式来实现 
```

top始终向待压入位置

线式存储实现

```C
mystack.h

#ifndef MYSTACK_H
#define MYSTACK_H

typedef struct _Stack
{
    int _len;
    int _top;
    char *_space;  //游标
}Stack;

void initStack(Stack *s,int size);

int isStackFull(Stack *s);
int isStackEmpty(Stack *s);

void push(Stack *s,char ch);

char pop(Stack *s);
void clearStack(Stack *s);
void resetSatck(Stack *s);
#endif // MYSTACK_H
```

实现

mystack.c

```C
#include"mystack.h"


void initStack(Stack *s,int size)
{
    s->_top = 0;
    s->_len = size;
    s->_space = (char *)malloc(sizeof(char)*s->_len);
}

int isStackFull(Stack *s)
{
//    if(s->_top==s->_len)
//        return true;
//    else
//        return false;

    return s->_top == s->_len;
}

int isStackEmpty(Stack *s)
{
    return s->_top == 0;
}

void push(Stack *s,char ch)
{
    s->_space[s->_top++]=ch;
}

char pop(Stack *s)
{
    return s->_space[--s->_top];
}

void clearStack(Stack *s)  //清空   销毁栈
{
    free(s->_space);
}

void resetSatck(Stack *s)
{
    s->_top=0;
}
```

主函数

main.c

```c
#include <stdio.h>
#include"mystack.h"


int main(int argc, char *argv[])
{
    Stack s;
    initStack(&s,100);

    char ch;
    for( ch='a';ch<='z';ch++)
    {
        if(!isStackFull(&s))
            push(&s,ch);
    }

    // resetSatck(&s);

    while(!isStackEmpty(&s))
        printf("%c\t",pop(&s));

    return 0;
}

```



链式存储实现

		设计时选择表头，作为栈顶指针，而不是表尾(单向链表(不含头节点))。不同于线式存储，不需要作判满操作 



```C++ 
myStackList.h

#ifndef __MYSTACKLIST__
#define __MYSTACKLIST__

typedef struct _SNode
{
	char _data;
	struct _SNode * _next;
}SNode;

typedef struct _Stack
{
	SNode * top;

}Stack;

void initStack(Stack *ps);

int isStackEmpty(Stack *ps);

int isStackFull(Stack *ps);

void push(Stack *ps, char ch);

char pop(Stack *ps);

void clearStack(Stack *ps);

void resetStack(Stack *ps);



#endif
```





实现

```C ++
myStackList.cpp

#include"myStackList.h"
#include<iostream>



void initStack(Stack *ps)
{
	ps->top = new SNode;
	ps->top->_next = nullptr;
}

int isStackEmpty(Stack *ps)
{
	return ps->top->_next == nullptr;
}

int isStackFull(Stack *ps)
{
	return 0;
}

void push(Stack *ps, char ch)
{
	SNode *cur = new SNode;
	cur->_data = ch;

	cur->_next = ps->top->_next;
	ps->top->_next = cur;


}

char pop(Stack *ps)
{
	SNode *t = ps->top->_next;
	char ch = t->_data;

	ps->top->_next = t->_next;
	free(t);
	return ch;
}
void resetStack(Stack *ps)
{
	while (!isStackEmpty(ps))
		pop(ps);
}

void clearStack(Stack *ps)
{
	resetStack(ps);
	free(ps->top);
}

```

主函数

main.cpp

```C++
#include<iostream>
#include"myStackList.h"

int main()
{
	Stack s;
	initStack(&s);

	for (char ch = 'A'; ch <= 'M'; ch++)
	{
		push(&s, ch);
	}

	while (!isStackEmpty(&s))
	{
		std::cout << pop(&s)<<"  ";
	}

	clearStack(&s);

	system("pause");
	return 0;
}
```































