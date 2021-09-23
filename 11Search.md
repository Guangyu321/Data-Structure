**Search**    **查找**

[TOC]

查找的前提    **排序**；

​	

## **qsort的compare函数**

​		  功能：使用快速排序例程进行排序 

​		 头文件：stdlib.h 
​		 用法：void **qsort**( void base, size_t num, size_t width, int (__cdecl *compare )(const void , const void *) ); 
​		**qsort**　**参数：** 

​							1、base 待排序数组**首地址 **

​							2、num 数组中待排序元素**数量**

​							3、width 各元素的**占用空间大小**

​							4、compare 指向函数的指针，用于确定**排序的顺序**

一维数组

```c++
int compare(const void* a,const void* b)
{
    return *(int*)a-*(int*)b;
}
```

二维数组

```c
//a[1000][2]
qsort(a,1000,sizeof(int)*2,comp);
int compare(const void* a,const void* b) 
{
   return((int*)a)[0]-((int*)b)[0];
}
```

字符串

```c
int compare(const void* p1,const void* p2)
{
   return strcmp((char*)p2,(char*)p1);
}
```

结构体1

```c
结构体1

struct Node
{
    double data;                //
    int other;
}s[100];

int compare(const void* p1,const void* p2)
{
    return (*(Node*)p2).data>(*（Node*）p1).data?1:-1;
}
```

结构体2

```c++
结构体2

struct Node
{
    int x;
    int y;
}s[100];
//按照x从小到大排序，当x相等时按y从大到小排序
int compare(const void*p1,const void*p2)
{
    struct Node*c=(Node*)p1;
    struct Node*d=(Node*)p2;
    if(c->x != d->x)
        return c->x - d->x;
    else 
        return d->y - c->y;
}
```

结构体3

```c++
struct Node{
    int data;
    char str[100];
}s[100];
//按照结构体中字符串str的字典序排序
int compare(const void*p1, const void*p2)
{
    return strcmp((*(Node*)p1).str,(*(Node*)p2).str);
}
qsort(s,100,sizeof(s[0]),compare);
```

**总结**

​		void指针转换为类型指针，取指针指向的值比较。



## **Sort头文件模板库**

#include <algorithm>

​	sort(s.begin(), s.end());                                    **//从小到大排列顺序**
​	sort(s.begin(), s.end(), **greater<int>()**);	 **//从大到小排列顺序**

```c++
	
	// 用默认的 operator< 排序                      //从小到大排列顺序
	std::sort(s.begin(), s.end());
	for (auto a : s)
    {
		std::cout << a << " ";
	}
	std::cout << '\n';

	// 用标准库比较函数对象排序
	std::sort(s.begin(), s.end(), std::greater<int>());   //从大到小排列顺序
	for (auto a : s) 
    {
		std::cout << a << " ";
	}
	std::cout << '\n';

```



## **二分查找 **

​				线路排查之二分。 

![](C:\Users\chen\Desktop\C语言\C笔记\插图\二分查找.jpg)

**代码**

```c++
#include<iostream>

//二分查找法

using namespace std;


//======================================================================//
int compare(const void *a,const void *b)          //比较函数非常重要
{
	//return *(int *)a - *(int *)b ;
	return *(int *)a > *(int *) b ? 1 : 0;
}

int compare(const void *a,const void *b)          
{
	return *(char *)a - *(char *) b ;
}
int compare(const void *a,const void *b)         //double 与int 与 char  不同
{												//注意这里是用比较大小的方法，来返回正负
	return *(double *)a > *(double *) b ? 1 : -1 ;
}
//====================================================================//


int binsearch(int *Arr, int low, int high, int find)   //迭代版
{
	int mid;
	while (low <= high)
	{
		mid = (low + high) / 2;
		if (Arr[mid] == find)
			return mid;
		else if (find > Arr[mid])
			low = mid + 1;
		else
			high = mid + 1;

	}
	return -1;
}

int binsearch2(int *Arr, int low, int high, int find)   //递归版
{
	int mid;
	while (low <= high)
	{
		mid = (low + high) / 2;
		if (Arr[mid] == find)
			return mid;
		else if (find > Arr[mid])
			return binsearch2(Arr, mid + 1, high, find);
		else
			return binsearch2(Arr, low, mid - 1, find);
	}
	return -1;
}


int main()
{
	int arr[10] = { 1, 3, 5, 7, 9, 2, 4, 6, 8, 10 };
	qsort(arr, 10, sizeof(int), compare);
	for (int i = 0; i<10; i++)
	{
		cout << arr[i] << "  ";	
	}
	
	putchar(10);
	cout << "please input search data :";
	int find;
	cin >> find;
	
	int val = binsearch2(arr, 0, 9, find);	
	cout << find << "  index  is  " << val << endl;
	
	return 0;
}

```

