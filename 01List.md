**List** 链表



**抽象数据类型** （Abstract Data Type ，**ADT**），是一种从学术研究的层面抽象的方法结果集。

(百度百科)

**抽象数据类型**（**ADT**）		是一个实现包括储存数据元素的**存储结构**以及实现基本操作的算法。

​				在这个数据抽象思想中，**数据类型**的定义和它的实现是分开的，这在**软件设计**中是一个重要的概念。

​			    这使得只研究和使用它的结构而不用考虑它的实现细节成为可能。



链表的增、删、改、查、排序、  销毁、

```c++
mylist.h

#ifndef __MYLIST__
#define __MYLIST__

typedef struct _Node
{
	int _data;
	struct _Node * _next;
}Node;

Node * createList();                    //创建链表
void traverseList(Node *head);               //遍历
void insertList(Node * head, int data);   //插入
int lenList(Node* head);             //长度
Node *searchList(Node * head, int findData);     //   查询节点
void deleteNodeinList(Node *head, Node *pfind);   //   删除节点
void SortpopList(Node *head,int len);     //   排序     冒泡法排序
void destroyList(Node *head);        //  销毁链表
void reverseList(Node *head);		//反转

#endif 
```



链表的实现；

```c++
mylist.cpp


#include"mylist.h"
#include<iostream>

Node * createList()    //创建链表
{
	Node *head = new Node;
	head->_next = nullptr;
	return head;
}

void traverseList(Node *head)           //遍历
{
	head = head->_next;
	while (head)
	{
		std::cout << head->_data << std::endl;
		head = head->_next;
	}
}

void insertList(Node * head, int data)           //插入    （以头插法为例）
{
	Node *cur = new Node;
	cur->_data = data;

	cur->_next = head->_next;
	head->_next = cur;

}

int lenList(Node* head)          //长度
{
	int len = 0;
	head = head->_next;
	while (head)
	{
		head = head->_next;
		len++;
	}
	return len;
}


Node *searchList(Node * head, int findData)    //   查询节点
{
	head = head->_next;
	while (head)
	{
		if(head->_data == findData)
			break;
		head = head->_next;
	}
	return head;

}
void deleteNodeinList(Node *head, Node *pfind)    //   删除节点
{
	while (head->_next != pfind)
		head = head->_next;

		head->_next = pfind->_next;
		delete pfind;
}

void SortpopList(Node *head,int len)       //   排序     冒泡法排序
{
	Node *cur = nullptr;
	int temp;
	for (int i = 0; i < len - 1; i++)
	{
		cur = head->_next;
		for (int j = 0; j < len - 1 - i; j++)
		{
			if (cur->_data > cur->_next->_data)
			{
				temp = cur->_data;
				cur->_data = cur->_next->_data;
				cur->_next->_data = temp;
			}
			cur = cur->_next;
		}
	}
}

/*
void SortpopList(Node *head, int len)            // 排序   指针排序
{
	Node *p = nullptr, *q = nullptr, *sh = nullptr;
	Node *tmp = nullptr;

	for (int i = 0; i < len - 1; i++)
	{
	   	sh = head;
		p = sh->_next;
		q = p->_next;
		 for (int j = 0; j < len - i - 1; j++)
		 {
			 if (p->_data >q->_data)
			 {
				 sh->_next = q;
				 p->_next = q->_next;
				 q->_next = p;

				 tmp = p;
				 p = q;
				 q = tmp;
			 }
			 sh = sh->_next;
			 p = p->_next;
			 q = q->_next;
		 }
	}
}



*/

void destroyList(Node *head)    //  销毁链表
{
	Node *p = nullptr;
	while (head)
	{
		p = head->_next;
		delete head;
		head = p;
	}
}

void reverseList(Node *head)       //反转
{
	Node *p, *q;
	p = head->_next;
	head->_next = nullptr;
	while (p != nullptr)
	{
		q = p->_next;
		p->_next = head->_next;
		head->_next = p;
		p = q;
	}
}


```









下述产生的链表为作者自己手动输入；

遇到0停止输入；



```c
Node * createList()               //头插法创建链表
{
	Node *head = new Node;
	head->_next = nullptr;

	Node *cur = nullptr;
	std::cout << "please input data:" << std::endl;;
	int data;
	std::cin>>data;

	while (data)
	{
		cur = new Node;
		cur->_data = data;

		cur->_next = head->_next;
		head->_next = cur;

		std::cin>>data;
	}
	return head;
}
```





```C
Node * createList()          //尾插法创建链表
{
	Node * head = new Node;
	head->_next = nullptr;
	
	Node *cur = nullptr;
	Node * pt = head;

	int data;
	std::cout << "please input data:" <<std::endl;
	std::cin >> data;

	while (data)
	{
		cur = new Node;
		cur->_data = data;

		pt->_next = cur ;
		cur->_next = nullptr;
		pt = cur;
		std::cin >> data;
	}
	return head;

}

```



main函数：

```
main.cpp

#include<iostream>
#include"mylist.h"
using namespace std;


int main()
{
	Node *head = createList();
	//traverseList(head);


	//===========================================//
	
	for (int i = 0; i < 20; i++)
	{
		insertList(head, rand() % 20);
	}
	
	
	//=============================================//

	traverseList(head);


	cout << "//============================//" << endl;
	int len=lenList(head);
	cout << len << endl;
	
	cout << "//============================//" <<endl;
	
	insertList(head, 1000);
	traverseList(head);
	
	cout << "//============================//" << endl;


	Node* pfind = searchList(head, 1);
	if (pfind == nullptr)
	{
		cout << "finf none!" <<endl;
	}
	else
	{
		cout << "find " << pfind << endl;
	}
	cout << "//============================//" << endl;

	deleteNodeinList(head, pfind);
	traverseList(head);
	cout << "//============================//" << endl;

	SortpopList(head, len);
	traverseList(head);
	cout << "//============================//" << endl;

	reverseList(head);
	traverseList(head);
	cout << "//============================//" << endl;

	destroyList(head);

	system("pause");
	return 0;
}



```

