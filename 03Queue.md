**Queue**  队列

​		队列分为链式队列和静态队列；静态队列一般用数组来实现，但此时的队列必须是循环队列，否则会造成巨大的内存浪费；链式队列是用链表来实现队列的。

​        FIFO   先进先出



BFS   广度优先搜索（广度优先遍历)

​			借助  队列  和  栈  来完成记录的功能。

DFS    深度优先搜索（深度优先遍历）





队列链式存储	(带头节点版本)

```c
#include<iostream>

using namespace std;
//队列链式存储
//带头节点版本

struct QNode
{
	int data;
	struct QNode *next;
};
//链式存储中，只放两个头尾指针
struct Queue
{
	struct QNode *front;
	struct QNode *rear;

};

//带头节点
void initQueue(struct Queue *q)
{
	q->front = q->rear = (struct QNode *)malloc(sizeof(struct QNode));
	q->rear->next = nullptr;
}

int isQueueEmpty(struct Queue *q)
{
	return q->rear == q->front;
}

void enQueue(struct Queue *q, int dat)
{
	struct QNode *cur = (struct QNode *)malloc(sizeof(struct QNode));

	cur->data = dat;
	cur->next = q->rear->next;
	q->rear->next = cur;
	q->rear = cur;

}

int deQueue(struct Queue *q)
{
	if (q->front->next == q->rear)
	{
		//只有一个元素
		int t = q->front->next->data;
		free(q->rear);
		q->rear = q->front;
		return t;
	}
	else
	{
		//有多个元素
		int t = q->front->next->data;
		struct QNode * pdel = q->front->next;
		q->front->next = q->front->next->next;
		free(pdel);
		return t;
	}
}

void clearQueue(struct Queue *q)
{
	struct QNode *t = q->front;
	struct QNode *cur;
	while (t)
	{
		cur = t->next;
		free(t);
		t = cur;
	}
	q->front = q->rear = nullptr;

}

int main()
{
	struct Queue q;
	initQueue(&q);

	for (int i = 0; i < 10; i++)
	{
		enQueue(&q, i);
	}

	while (!isQueueEmpty(&q))
	{
		printf("%d  ", deQueue(&q));
	}

	clearQueue(&q);

	return 0;
}
```







//队列链式存储	（不带头节点）

```c
myqueue.h

#ifndef __MYQUEUE_H__
#define __MYQUEUE_H__

//队列链式存储	（不带头节点）

typedef struct _QNode
{
	char _data;
	struct _QNode *_next;
}QNode;

typedef struct _Queue
{
	QNode *_front;
	QNode *_rear;

}Queue;

void initQueue(Queue *pq);
int isQueueEmpty(Queue *pq);
void enQueue(Queue *pq, char data);
char deQueue(Queue *pq);
void clearQueue(Queue *pq);

#endif

```





实现  myqueue.cpp

```c
#include"myqueue.h"
#include<stdlib.h>

void initQueue(Queue *pq)
{
	pq->_front = pq->_rear = nullptr;
}
int isQueueEmpty(Queue *pq)
{
	if (pq->_front == pq->_rear && pq->_front == nullptr)
		return 1;
	else
		return 0;
}
void enQueue(Queue *pq, char data)
{
	if (isQueueEmpty(pq))
	{
		pq->_front = (QNode*)malloc(sizeof(QNode));
		pq->_front->_data = data;
		pq->_rear = pq->_front;
		pq->_rear->_next = nullptr;
	}
	else
	{
		QNode *cur = (QNode*)malloc(sizeof(QNode));
		cur->_data = data;
		cur->_next = nullptr;
		pq->_rear->_next = cur;//=======//
		pq->_rear = cur;


	}

}
char deQueue(Queue *pq)
{
	char data;
	if (pq->_front == pq->_rear)
	{
		data = pq->_front->_data;
		free(pq->_front);
		pq->_rear = pq->_front = nullptr;
	}
	else
	{
		data = pq->_front->_data;
		QNode *t = pq->_front;
		pq->_front = pq->_front->_next;
		free(t);
	}
	return data;
}
void clearQueue(Queue *pq)
{
	if (isQueueEmpty(pq))
		return;
	QNode *t;
	while (pq->_front)
	{
		t = pq->_front;
		pq->_front = pq->_front->_next;
		free(t);
	}
}
```





main.cpp



```c
#include<iostream>
#include"myqueue.h"
using namespace std;


int main()
{
	Queue q;
	initQueue(&q);

	for (int i = 0; i < 10; i++)
	{
		enQueue(&q, i);
	}

	while (!isQueueEmpty(&q))
	{
		printf("%d  ", deQueue(&q));
	}

	clearQueue(&q);
	return 0;
}
```

