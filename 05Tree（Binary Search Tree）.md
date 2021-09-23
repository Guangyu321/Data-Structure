**搜索二叉树**	（BST）

二叉搜索树(BST，Binary Search Tree），也称二叉排序树或二叉查找树。 

二叉树搜索树不同于一般的二叉树，它是一种特殊的二叉树，如果对这棵树进行**中序遍历**，会得到一个递增的队列。



1、定义

二叉搜索树：一棵二叉树，可以为空；如果不为空，满足以下性质：

​	1，非空左子树的所有键值小于其根节点的键值；

​	2，非空右子树的所有键值大于其根节点的键值；

​	3，左、右子树都是二叉搜索树



2、插入（  迭代版  递归版  ）

​		二叉查找树的插入过程如下：
​			❶若当前的二叉查找树为空， 则插入的元素为根节点，
​			❷若插入的元素值小于根节点值， 则将元素插入到左子树中
​			❸若插入的元素值不小于根节点值， 则将元素插入到右子树中。 

3、查找（  迭代版  递归版  ）

​		查找的效率决定于树的高度，亦可以用递归实现。 

4、树的最小值节点

​		一直寻找左子树的叶节点

5、树的最大值节点

​		一直寻找右子树的叶节点

6、当前节点的父节点

7、二叉树的节点删除

​		二叉查找树的删除，分三种情况进行处理： 

​				➢第一种情况： 若为叶子节点则直接删除。**（根叶子要单独处理）** 

​				➢第二种情况： 若该节点，有一个节点，左或是右。因为只有一个节点，直接令祖父节点指向孙子节

​											点。**（若该节点为根要单独处理）**

​				➢第三种情况：若该节点含有两个子节点，一般的删除策略是用其右子树的最小结点代替待删除结点的

​										   数据，然后递归删除那个右子树最小结点。 即将第三种情况转化为第二种情况。如下图

​										   所示，要删除结点 2，则找到结点 2 的右子树的最小结点 3 并将其数据赋值给结点 2，然										   后在删除结点 3。 

​										   ➢第三种情况的**特殊情况分析** （待删除节点（a）的右子树的最小节点，即为其右子树的

​											节点）

8、销毁树



main.cpp

```c++
#include<iostream>
#include<stdio.h>
using namespace std;

typedef struct _TreeNode
{
	int _data;
	struct _TreeNode *_left;
	struct _TreeNode *_right;

}TreeNode;

TreeNode * initBInSearch()                           //创建（空树）
{
	return nullptr;
}

//=====================插入=======================//
void insertBinSearchTree(TreeNode **root, int data)      //迭代版
{
	TreeNode *tmp, *st = *root;
	if ((*root) == nullptr)
	{
		(*root) = (TreeNode*)malloc(sizeof(TreeNode));
		(*root)->_data = data;
		(*root)->_right = (*root)->_left = nullptr;
	}
	else
	{
		while (1)
		{
			if (data > (st)->_data)
			{
				if ((st)->_right == nullptr)
				{
					tmp = (TreeNode *)malloc(sizeof(TreeNode));
					tmp->_data = data;
					tmp->_left = tmp->_right = nullptr;
					(st)->_right = tmp;
					break;
				}
				else
					(st) = (st)->_right;
			}
			else
			{
				if ((st)->_left == nullptr)
				{
					tmp = (TreeNode*)malloc(sizeof(TreeNode));
					tmp->_data = data;
					tmp->_left = tmp->_right = nullptr;
					(st)->_left = tmp;
					break;
				}
				else
					(st) = (st)->_left;
			}
		}
	}
}

void insertBstRecusive(TreeNode ** root, int data)    //递归版
{
	if ((*root) == nullptr)
	{
		(*root) = (TreeNode*)malloc(sizeof(TreeNode));
		(*root)->_data = data;
		(*root)->_left = (*root)->_right = nullptr;
	}
	else if (data > (*root)->_data)
	{
		insertBstRecusive(&(*root)->_right, data);
	}
	else
	{
		insertBstRecusive(&(*root)->_left, data);
	}
}


//=========中序遍历===============//
void midOlderTraverseBst(TreeNode *root)
{
	if (root)
	{
		midOlderTraverseBst(root->_left);
		printf("  %d  ", root->_data);
		midOlderTraverseBst(root->_right);
	}
}

//=============查找=========================//
TreeNode * searchBinSearchTree(TreeNode *t, int pfind)  //迭代版
{
	while (t)
	{
		if (t->_data == pfind)
			return t;
		else if (pfind > t->_data)
			t = t->_right;
		else
			t = t->_left;
	}
	return nullptr;
}
//
TreeNode * searchBstRecursive(TreeNode * t, int pfind)//递归版
{
	if (!t)
		return nullptr;
	if (pfind == t->_data)
	{
		return t;
	}
	else if (pfind > t->_data)
		return searchBstRecursive(t->_right, pfind);
	else
		return searchBstRecursive(t->_left, pfind);
}

//===========树的最小节点========================//
TreeNode * getMinNodeBSTree(TreeNode *root)
{
	if (root)
	{
		while (root->_left)
		{
			root = root->_left;
		}
		return root;
	}
	return nullptr;
}

//===========树的最大节点========================//
TreeNode * getMaxNodeBSTree(TreeNode *root)
{
	if (root)
	{
		while (root->_right)
		{
			root = root->_right;
		}
		return root;
	}
	return nullptr;
}

//==================当前节点的父节点=====================//
TreeNode *getParentBinSearchTree(TreeNode *root, TreeNode *cur)
{
	static TreeNode *parent=nullptr ;//递归中的静态变量
	if (root)
	{
		if (root->_left == cur || root->_right == cur)
			parent = root;

		getParentBinSearchTree(root->_left, cur);
		getParentBinSearchTree(root->_right, cur);
	}
	return parent;
}

//===================二叉树的节点删除=========================//
void deleteNodeBinSearchTree(TreeNode **root, TreeNode *pfind)
{
	TreeNode *t = *root;
	TreeNode *parent;

	if (t == nullptr || pfind == nullptr)
		return;

	if (pfind->_left == nullptr&&pfind->_right == nullptr)
	{
		if (pfind == t)
		{
			free(t);
			*root = nullptr;
			return;
		}

		parent = getParentBinSearchTree(t, pfind);

		if (parent)
		{
			if (parent->_left == pfind)
			{
				free(parent->_left);
				parent->_left = nullptr;
					
			}
			else
			{
				free(parent->_right);
				parent->_right = nullptr;
			}
		}
	}
	else if (pfind->_left == nullptr&&pfind->_right != nullptr)
	{
		if (t == pfind)
		{
			*root = t->_right;
			free(t);
			return;
		}

		parent = getParentBinSearchTree(t, pfind);

		if (parent->_left == pfind)
		{
			parent->_left = pfind->_right;
			free(pfind);
		}
		else
		{
			parent->_right = pfind->_right;
			free(pfind);
		}
	}
	else if (pfind->_left != nullptr&&pfind->_right == nullptr)
	{
		if (t == pfind)
		{
			*root = t->_left;
			free(t);
			return;
		}

		parent = getParentBinSearchTree(t, pfind);
		if (parent->_right == pfind)
		{
			parent->_right = pfind->_left;
			free(pfind);
		}
		else
		{
			parent->_left = pfind->_left;
			free(pfind);
		}
	}
	else
	{
		TreeNode *minRight =getMinNodeBSTree(pfind->_right);
		pfind->_data = minRight->_data;
		parent = getParentBinSearchTree(t, minRight);//*********

		if (parent->_right == minRight)  
			parent->_right = nullptr;
		else
			parent->_left = nullptr;
		cout << "minRight: " << minRight->_data << endl;
		
		//parent->_left = nullptr;
		free(minRight);
		
	}
}

//==========================销毁树==========================//
void destroyBinSearchTree(TreeNode *root)
{
	if (root != nullptr)
	{
		destroyBinSearchTree(root->_left);
		destroyBinSearchTree(root->_right);
	}
	free(root);
}
int main()
{
	TreeNode *root = initBInSearch();

	//insertBinSearchTree(&root, 30);
	//insertBinSearchTree(&root, 8);
	//insertBinSearchTree(&root, 15);
	//insertBinSearchTree(&root, 36);
	//insertBinSearchTree(&root, 100);
	//insertBinSearchTree(&root, 32);

	insertBstRecusive(&root, 30);
	insertBstRecusive(&root, 8);
	insertBstRecusive(&root, 15);
	insertBstRecusive(&root, 36);
	insertBstRecusive(&root, 100);
	insertBstRecusive(&root, 32);
	/*
	insertBstRecusive(&root, 4);
	insertBstRecusive(&root, 2);
	insertBstRecusive(&root, 6);
	insertBstRecusive(&root, 14);
	insertBstRecusive(&root, 16);
	insertBstRecusive(&root, 31);
	insertBstRecusive(&root, 33);
	insertBstRecusive(&root, 98);
	insertBstRecusive(&root, 105);
	insertBstRecusive(&root, 97);
	insertBstRecusive(&root, 99);
	insertBstRecusive(&root, 103);
	insertBstRecusive(&root, 106);
	*/

	midOlderTraverseBst(root);
	
	putchar(10);
	//TreeNode *find = searchBinSearchTree(root, 100);
	TreeNode *find = searchBstRecursive(root,36);
	if (find != nullptr)
		printf(" \n  find = %d  ", find->_data);
	else
		printf(" \n find none!   ");

	//TreeNode *min = getMinNodeBSTree(root);
	//printf("\n  min node = %d  ", min->_data);
	TreeNode *max = getMaxNodeBSTree(root);
	printf("\n  max node = %d  ", max->_data);

	//TreeNode *parent = getParentBinSearchTree(root, min);
	//printf("\n  min parent = %d  ", parent->_data);
	TreeNode *parent = getParentBinSearchTree(root, max);
	printf("\n  max parent = %d \n\n\n\n ", parent->_data);


	deleteNodeBinSearchTree(&root, find);
	midOlderTraverseBst(root);

	//destroyBinSearchTree(root);

	return 0;
}
```



