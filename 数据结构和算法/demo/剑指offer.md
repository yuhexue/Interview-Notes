二维数组中的查找

题目描述：在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

思路：

```C++
class Solution
{
    public:
        bool Find(int target, vector<vector<int> > array)
        {
            if(array.size() == 0)
                return false;
            if(array[0].size() == 0)
                return false;
            int rowNumber = 0;
            int colNumber = array[0].size()-1;
            while(rowNumber < array.size() && colNumber >= 0)
            {
                if(target < array[rowNumber][colNumber])
                    --colNumber;
                else if(target > array[rowNumber][colNumber])
                    ++rowNumber;
                else
                    return true;
            }
            return false;
        }
};
```

替换空格

题目描述：请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

思路：修正长度

```C++
class Solution
{
    public:
        void replaceSpace(char *str,int length)
        {
            if(str == NULL && length <= 0)
                return ;
            int len = 0,cn = 0,i;
            while(str[i++] != '\0')
            {
                ++len;
                if(str[i] == ' ')
                    ++cn;
            }
            int nlen = len + 2 * cn;
            int index = len, nindex = nlen;
            while(index >= 0 && nindex > index)
            {
                if(str[index] == ' ')
                {
                    str[nindex--] = '0';
                    str[nindex--] = '2';
                    str[nindex--] = '%';
                    index--;
                }
                else
                    str[nindex--] = str[index--];
            }
        }
};
```

从尾到头打印一个链表

题目描述：输入一个链表，按链表值从尾到头的顺序返回一个ArrayList。

思路：反转链表，栈等

```C++
/**
*  struct ListNode {
*        int val;
*        struct ListNode *next;
*        ListNode(int x) :
*              val(x), next(NULL) {
*        }
*  };
*/
class Solution
{
    public:
        vector<int> printListFromTailToHead(ListNode* head)
        {
            vector<int> ret;
            if(!head)
                return ret;
            while(head)
            {
                ret.push_back(head->val);
                head = head->next;
            }
            reverse(ret.begin(),ret.end());
            return ret;
        }
};
```

重建二叉树

题目描述：输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

思路：

```C++
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution
{
    public:
        unordered_map<int, int> m;
        TreeNode* reConstructBinaryTree(vector<int> pre,vector<int> vin)
        {
            if(pre.empty())
                return NULL;
            for(int i = 0; i < vin.size(); i++)
                m[vin[i]] = i; 
            return build(vin, 0, vin.size() - 1,pre, 0, pre.size() - 1);
        }
 
        TreeNode* build(vector<int>& inorder, int s0, int e0,vector<int>& preorder, int s1, int e1)
        {
            if(s0 > e0 || s1 > e1)
                return NULL;
             
            TreeNode* root = new TreeNode(preorder[s1]);  
            int mid = m[preorder[s1]]; 
            int num = mid - s0;  
            root->left = build(inorder, s0, mid - 1, preorder, s1 + 1, s1 + num );
            root->right = build(inorder, mid + 1, e0, preorder, s1 + num + 1, e1);
            return root;
        }
};
```

### 用两个栈实现队列

题目描述：用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

思路：

始终维护s1作为**存储空间**，以s2作为**临时缓冲区**。

- 入队时，将元素压入s1。
- 出队时，将s1的元素逐个“倒入”（弹出并压入）s2，将s2的顶元素弹出作为出队元素，之后再将s2剩下的元素逐个“倒回”s1。

改进：

但是每次倒来倒去的还是效率不太好，因此我们思考出了如下的变种

始终维护s1作为**输入栈**，以s2作为**输出栈**

- 入队时，将元素压入s1。

- 出队时，判断s2是否为空，如不为空，则直接弹出顶元素；如为空，则将s1的元素逐个“倒入”s2，把最后一个元素弹出并出队。 

  两个栈的装填顺序是没有问题的，避免了反复“倒”栈，仅在需要时才“倒”一次。

```C++
class Solution
{
    public:
        void push(int node)
        {
            stack1.push(node);
        }
        int pop()
        {
            if(stack2.empty())
            {
                while(!stack1.empty())    //反向作用
                {
                    stack2.push(stack1.top());
                    stack1.pop();
                }           
            }
            int ret = stack2.top();
            stack2.pop();
            return ret;
        }
    private:
        stack<int> stack1;
        stack<int> stack2;
};
```

旋转数组的最小数字

题目描述：把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非减排序的数组的一个旋转，输出旋转数组的最小元素。 例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。 NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

思路：

```C++
class Solution
{
    public:
        int minNumberInRotateArray(vector<int> rotateArray)
        {
            int len = rotateArray.size();
            if(len == 0)
                return 0;
            if(len == 1)
                return rotateArray[0];
            if(len == 2)
                return min(rotateArray[0],rotateArray[1]);
            int left = 0,right = rotateArray.size()-1;
            while(left < right-1)
            {
                if(rotateArray[left]<rotateArray[right])
                    return rotateArray[left];
                int mid = left + (right-left)/2;
                if(rotateArray[left] < rotateArray[mid])
                    left = mid;
                else if(rotateArray[left] > rotateArray[mid])
                    right = mid;
                else
                    left++;
            }
            return min(rotateArray[left],rotateArray[right]);
        }
};
```

斐波那契数列

题目描述：大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）。n<=39

思路：

```C++
class Solution
{
    public:
        int Fibonacci(int n)
        {
            if(n < 2)
                return n;
            int a = 0,b = 1,ret = 0;  //fn = fn-1 + fn-2;
             for(int i=1;i<n;i++)
            {
                ret = a + b;
                a = b;
                b = ret;  
            }
            return ret;
        }
};
```

跳台阶

题目描述：一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）。

思路：

```C++
class Solution
{
    public:
        int jumpFloor(int number)
        {
            if(number <= 1)
                return 1;
            int ret = 0;
            ret = jumpFloor(number-1) + jumpFloor(number-2);
            return ret;
        }
};
```

变态跳台阶

题目描述：一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

思路：

```C++
class Solution
{
    public:
        int jumpFloorII(int number)
        {
            if(number <= 1)
                return 1;
            int ret = 0;
            for(int i = 0;i<number;++i)
                ret += jumpFloorII(i);
            return ret;
        }
};
```

矩形覆盖

题目描述：我们可以用2*1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个小矩形无重叠地覆盖一个2n的大矩形，总共有多少种方法？

思路：

```C++
class Solution
{
    public:
        int rectCover(int number)
        {
            if(number<0)
                return 0;
            else if(number < 4)
                return number;
            int ret = 0;
            ret += rectCover(number-1)+rectCover(number-2);
            return ret;
        }
};
```

二进制中1的个数

题目描述：输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。

思路：

```C++
class Solution
{
    public:
        int NumberOf1(int n)
        {
            int count = 0;
            while(n)
            {
                ++count;
                n = (n - 1) & n;
            }
            return count;
        }
};
```

数值的整数次方：

题目描述：给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。

思路：

```C++
class Solution
{
    public:
        double Power(double base, int exponent)
        {
            if(exponent < 0)
            {
                exponent *= -1;
                base = 1/base;
            }
            if(exponent == 0)
                return 1.0;
            if(exponent == 1)
                return base;
            int cn = 1;
            double ret = base;
            while(cn*2 <= exponent)
            {
                ret *= ret;
                cn *= 2;
            }
            ret *= Power(base,exponent-cn);
            return ret;
        }
};
```

调整数组顺序使奇数位于偶数前面

题目描述：输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

思路：

```C++
class Solution
{
    public:
        void reOrderArray(vector<int> &array)
        {
            if(array.size() < 2)
                return ;
            deque<int> ret;
            int num = array.size();
            for(int i = 0;i < num;++i)
            {
                if(array[num - i - 1] % 2 == 1)
                    ret.push_front(array[num - i - 1]);
                if(array[i] % 2 == 0)
                    ret.push_back(array[i]);
            }
            array.assign(ret.begin(),ret.end());
            return ;
        }
};
```

反转链表

题目描述：输入一个链表，反转链表后，输出新链表的表头。

思路：

```C++
/*
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
            val(x), next(NULL) {
    }
};*/
class Solution
{
    public:
        ListNode* ReverseList(ListNode* pHead)
        {
            if(!pHead)
                return NULL;
            ListNode* pre = new ListNode(0);
            pre->next = pHead; 
            while(pHead->next)
            {
                ListNode* n = pHead->next; 
                pHead->next = n->next;
                n->next = pre->next;
                pre->next = n;
            }
            return pre->next;
        }
};
```

合并两个有序链表：

题目描述：输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

思路：

```C++
/*
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
            val(x), next(NULL) {
    }
};*/
class Solution
{
    public:
        ListNode* Merge(ListNode* pHead1, ListNode* pHead2)
        {
            ListNode* l1 = pHead1;
            ListNode* l2 = pHead2;
            if(!l1)
                return l2;
            if(!l2)
                return l1;
            ListNode dummy(0);
            ListNode* p = &dummy;
            while(l1 && l2)
            {
                if(l1->val < l2->val)
                {
                    p->next = l1;
                    p = p->next;
                    l1 = l1->next; 
                }
                else
                {
                    p->next = l2;
                    p = p->next;
                    l2 = l2->next;                     
                }
            }
            if(l1)
                p->next = l1;
            if(l2)
                p->next = l2;
            return dummy.next;  
        }
};
```

合并K个有序链表：

https://blog.csdn.net/gatieme/article/details/51097730



树的子结构：

题目描述：输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

思路：

```C++
/*
struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
    TreeNode(int x) :
            val(x), left(NULL), right(NULL) {
    }
};*/
class Solution
{
public:
            bool HasSubtree(TreeNode* pRoot1, TreeNode* pRoot2)
            {
bool result = false;
            if(pRoot1 != NULL && pRoot2 != NULL)
{
                    if(pRoot1->val == pRoot2->val){
                        result = DoesTree1HasTree2(pRoot1, pRoot2);
            }
                    if(!result)
{
                result = HasSubtree(pRoot1->left, pRoot2);
            }
            if(!result){
                result = HasSubtree(pRoot1->right, pRoot2);
            }
        }
        return result;
    }
private:
    bool DoesTree1HasTree2(TreeNode* pRoot1, TreeNode* pRoot2){
        if(pRoot2 == NULL){
            return true;
        }
        if(pRoot1 == NULL){
            return false;
        }
        if(pRoot1->val != pRoot2->val){
            return false;
        }
        return DoesTree1HasTree2(pRoot1->left, pRoot2->left) && DoesTree1HasTree2(pRoot1->right, pRoot2->right);
    }
};
```

二叉树的镜像：

题目描述：操作给定的二叉树，将其变换为源二叉树的镜像。

思路：

```C++
/*
struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
    TreeNode(int x) :
            val(x), left(NULL), right(NULL) {
    }
};*/
class Solution
{
    public:
        void Mirror(TreeNode *pRoot)
        {
            if(!pRoot || (!pRoot->left && !pRoot->right))
                return ;
            TreeNode* temp = pRoot->left;
            pRoot->left = pRoot->right;
            pRoot->right = temp;
            if(pRoot->left)
                Mirror(pRoot->left);
            if(pRoot->right)
                Mirror(pRoot->right);
        }
};
```

顺时针打印矩阵：

题目描述：输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.

思路：

```C++
class Solution
{
    public:
        vector<int> printMatrix(vector<vector<int> > matrix)
        {
            vector<int> ret;
            if(matrix.empty())
                return ret;
            int rowleft = 0,colleft = 0;
            int rowright = matrix.size()-1,colright = matrix[0].size()-1;
            bool flag = true;
            while(rowleft <= rowright && colleft <= colright)
            {
                if(flag)
                {
                    for(int i = colleft;i<=colright;++i)
                        ret.push_back(matrix[rowleft][i]);
                    rowleft++;
                    for(int i = rowleft;i<=rowright;++i)
                        ret.push_back(matrix[i][colright]);
                    colright--;
                    flag = false;
                }
                else
                {
                    for(int i = colright;i>=colleft;--i)
                        ret.push_back(matrix[rowright][i]);
                    rowright--;
                    for(int i = rowright;i>=rowleft;--i)
                        ret.push_back(matrix[i][colleft]);
                    colleft++;    
                    flag = true;
                } 
            }
            return ret;
        }
};
```

包含min()函数的栈：

题目描述：定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数（时间复杂度应为O（1））。

思路：

```C++
class Solution
{
    public:
        void push(int value)
        {
            Data.push(value);
            if(Min.empty() || Min.top() > value)
                Min.push(value);
        }
        void pop()
        {
            if(Data.top() == Min.top())
                Min.pop();
            Data.pop();
        }
        int top()
        {
            return Data.top();
        }
        int min()
        {
            return Min.top();
        }
    private:
        stack<int> Data;
        stack<int> Min;  
};
```

### 栈的压入弹出序列：

题目描述：输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）

思路：

```C++
class Solution
{
    public:
    bool IsPopOrder(vector<int> pushV,vector<int> popV)
    {
        if(pushV.empty())
            return false;
        stack<int> temp;
        int index = 0;
        for(int i = 0;i<pushV.size();++i)
        {
            temp.push(pushV[i]);
            while(index < popV.size() && temp.top() == popV[index])
            {
                temp.pop();
                index++;
            }
        }
        return temp.empty();
    }
};
```

从上往下打印二叉树：

题目描述：从上往下打印出二叉树的每个节点，同层节点从左至右打印。

思路：队列

```C++
/*
struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
    TreeNode(int x) :
            val(x), left(NULL), right(NULL) {
    }
};*/
class Solution
{
    public:
        vector<int> PrintFromTopToBottom(TreeNode* root)
        {
            vector<int> ret;
            if(!root)
                return ret;
            queue<TreeNode*> tree;
            tree.push(root);
            while(!tree.empty())
            {
                TreeNode* temp = tree.front();
                ret.push_back(temp->val);
                tree.pop();
                if(temp->left)
                    tree.push(temp->left);
                if(temp->right)
                   tree.push(temp->right);                  
            }
            return ret;
        }  
};
```

### 二叉搜索树的后序遍历序列：

题目描述：输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。

思路：

已知条件：后序序列最后一个值为root；二叉搜索树左子树值都比root小，右子树值都比root大。

1、确定root；

2、遍历序列（除去root结点），找到第一个大于root的位置，则该位置左边为左子树，右边为右子树；

3、遍历右子树，若发现有小于root的值，则直接返回false；

4、分别判断左子树和右子树是否仍是二叉搜索树（即递归步骤1、2、3）。

```C++
class Solution
{
    public:
        bool VerifySquenceOfBST(vector<int> sequence)
        {
            return travel(sequence,0,sequence.size()-1);
        }
        bool travel(vector<int> sequence,int start,int end)
        {
            if(sequence.empty() || start > end)
                return false;
            int root = sequence[end];
            int index1 = start,index2 = start;
            for(;index1<end;++index1)
            {
                if(sequence[index1] > root)
                    break;
            }
            for(index2=index1;index2<end;++index2)
            {
                if(sequence[index2] < root)
                    return false;
            }
            bool left = true;
            if(index1-1>start)
                left = travel(sequence,start,index1-1);
            bool right = true;
            if(index1 < end-1)
                right = travel(sequence,index1,end-1);
            return left&&right;
        }
};

//非递归 
//非递归也是一个基于递归的思想：
//左子树一定比右子树小，因此去掉根后，数字分为left，right两部分，right部分的
//最后一个数字是右子树的根他也比左子树所有值大，因此我们可以每次只看有子树是否符合条件
//即可，即使到达了左子树左子树也可以看出由左右子树组成的树还想右子树那样处理
 
//对于左子树回到了原问题，对于右子树，左子树的所有值都比右子树的根小可以暂时把他看出右子树的左子树
//只需看看右子树的右子树是否符合要求即可
class Solution {
public:
    bool VerifySquenceOfBST(vector<int> sequence) {
        int size = sequence.size();
        if(0==size)return false;
 
        int i = 0;
        while(--size)
        {
            while(sequence[i++]<sequence[size]);
            while(sequence[i++]>sequence[size]);
 
            if(i<size)return false;
            i=0;
        }
        return true;
    }
};
```

二叉树中和为某一路径的值：

题目描述：输入一颗二叉树的跟节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。(注意: 在返回值的list中，数组长度大的数组靠前)

思路：

```C++
/*
struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
    TreeNode(int x) :
            val(x), left(NULL), right(NULL) {
    }
};*/
class Solution
{
    public:
        vector<vector<int> > FindPath(TreeNode* root,int expectNumber)
        {
            if(!root)
                return result;
            tmp.push_back(root->val);
            if((expectNumber - root->val ) == 0 && root->left == NULL && root->right == NULL){
                result.push_back(tmp);
            }
            FindPath(root->left, expectNumber - root->val);
            FindPath(root->right, expectNumber - root->val);
            tmp.pop_back();
            return result;
        }
    private:
        vector<vector<int> > result;
        vector<int> tmp;
};
```

复杂链表的复制:

题目描述：输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）

思路：

```c++
/*
struct RandomListNode {
    int label;
    struct RandomListNode *next, *random;
    RandomListNode(int x) :
            label(x), next(NULL), random(NULL) {
    }
};
*/
class Solution {
public:
     
    //第一步，复制复杂指针的label和next
    void CloneNodes(RandomListNode* pHead){
        RandomListNode* pNode = pHead;
        while(pNode != NULL){
            RandomListNode* pCloned = new RandomListNode(0);
            pCloned->label = pNode->label;
            pCloned->next = pNode->next;
            pCloned->random = NULL;
             
            pNode->next = pCloned;
            pNode = pCloned->next;
        }
    }
     
    //第二步，处理复杂指针的random
    void ConnectSiblingNodes(RandomListNode* pHead){
        RandomListNode* pNode = pHead;
        while(pNode != NULL){
            RandomListNode* pCloned = pNode->next;
            if(pNode->random != NULL){
                pCloned->random = pNode->random->next;
            }
            pNode = pCloned->next;
        }
    }
     
    //第三步，拆分复杂指针
    RandomListNode* ReconnectNodes(RandomListNode* pHead){
        RandomListNode* pNode = pHead;
        RandomListNode* pClonedHead = NULL;
        RandomListNode* pClonedNode = NULL;
         
        if(pNode != NULL){
            pClonedHead = pClonedNode = pNode->next;
            pNode->next = pClonedNode->next;
            pNode = pNode->next;
        }
         
        while(pNode != NULL){
            pClonedNode->next = pNode->next;
            pClonedNode = pClonedNode->next;
            pNode->next = pClonedNode->next;
            pNode = pNode->next;
        }
        return pClonedHead;
    }
     
    RandomListNode* Clone(RandomListNode* pHead)
    {
        CloneNodes(pHead);
        ConnectSiblingNodes(pHead);
        return ReconnectNodes(pHead);
    }
};

```

### 二叉搜索树与双向链表：

题目描述：输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。

思路：

```C++
/*
struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
    TreeNode(int x) :
            val(x), left(NULL), right(NULL) {
    }
};*/
class Solution
{
    public:
        TreeNode* Convert(TreeNode* pRootOfTree)
        {
            TreeNode* pLastNodeInList = NULL;
            ConvertNode(pRootOfTree, &pLastNodeInList);
            TreeNode* pHeadOfList = pLastNodeInList;
            while(pHeadOfList != NULL && pHeadOfList->left != NULL)
                pHeadOfList = pHeadOfList->left;
            return pHeadOfList;
        }
        void ConvertNode(TreeNode* pNode, TreeNode** pLastNodeInList)
        {
            if(pNode == NULL)
                return;
            TreeNode* pCurrent = pNode;
            if(pCurrent->left != NULL)
                ConvertNode(pCurrent->left, pLastNodeInList);
            pCurrent->left = *pLastNodeInList;
            if(*pLastNodeInList != NULL)
                (*pLastNodeInList)->right = pCurrent;
            *pLastNodeInList = pCurrent;
            if(pCurrent->right != NULL)
                ConvertNode(pCurrent->right, pLastNodeInList);
        }
};
```

字符串的排列:

题目描述：输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。

思路：

```C++
class Solution
{
    public:
        vector<string> Permutation(string str)
        {
            vector<string> ret;
            if (str.empty())
                return ret;
            sort(str.begin(),str.end());
            ret.push_back(str);
            while (next_permutation(str.begin(), str.end()))
                ret.push_back(str);
            return ret;
        }
};
```

### 数组中出现次数超过一半的数字：

题目描述：数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。

思路：

```C++
class Solution
{
    public:
        int MoreThanHalfNum_Solution(vector<int> numbers)
        {
            if(numbers.empty())
                return 0;
            int target = numbers.size()/2;
            unordered_map<int,int> map;
            for(int i = 0;i<numbers.size();++i)
            {
                map[numbers[i]]++;
                if(map[numbers[i]] > target)
                    return numbers[i];
            }
            return 0;
        }
};
```

### 最小的K个数:

题目描述：输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。

思路：

```C++
class Solution
{
    public:
        vector<int> GetLeastNumbers_Solution(vector<int> input, int k)
        {         
            vector<int> ret;
            if(input.empty()||k>input.size())
                return ret;
            sort(input.begin(),input.end());
            for(int i = 0;i<k;++i)
                ret.push_back(input[i]);
            return ret;
        }
};
```

### 从1到n整数中1出现的次数:

题目描述：求出1~13的整数中1出现的次数,并算出100~1300的整数中1出现的次数？为此他特别数了一下1~13中包含1的数字有1、10、11、12、13因此共出现6次,但是对于后面问题他就没辙了。ACMer希望你们帮帮他,并把问题更加普遍化,可以很快的求出任意非负整数区间中1出现的次数（从1 到 n 中1出现的次数）。

思路：

```C++
    //主要思路：设定整数点（如1、10、100等等）作为位置点i（对应n的各位、十位、百位等等），分别对每个数位上有多少包含1的点进行分析
    //根据设定的整数位置，对n进行分割，分为两部分，高位n/i，低位n%i
    //当i表示百位，且百位对应的数>=2,如n=31456,i=100，则a=314,b=56，此时百位为1的次数有a/10+1=32（最高两位0~31），每一次都包含100个连续的点，即共有(a%10+1)*100个点的百位为1
    //当i表示百位，且百位对应的数为1，如n=31156,i=100，则a=311,b=56，此时百位对应的就是1，则共有a%10(最高两位0-30)次是包含100个连续点，当最高两位为31（即a=311），本次只对应局部点00~56，共b+1次，所有点加起来共有（a%10*100）+(b+1)，这些点百位对应为1
    //当i表示百位，且百位对应的数为0,如n=31056,i=100，则a=310,b=56，此时百位为1的次数有a/10=31（最高两位0~30）
    //综合以上三种情况，当百位对应0或>=2时，有(a+8)/10次包含所有100个点，还有当百位为1(a%10==1)，需要增加局部点b+1
    //之所以补8，是因为当百位为0，则a/10==(a+8)/10，当百位>=2，补8会产生进位位，效果等同于(a/10+1)
class Solution
{
    public:
        int NumberOf1Between1AndN_Solution(int n)
       {
            int count = 0;
            for(int i = 1; i <= n; i *= 10)
            {
                int a = n / i, b = n % i;
                count += (a + 8) / 10 * i + (a % 10 == 1) * (b + 1);
            }
            return count;
        }
};
```

### 把数组排成最小的数:

题目描述：输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。

思路：

```C++
class Solution
{
    public:
        string PrintMinNumber(vector<int> numbers)
        {
            string str;
            if(numbers.empty())
                return str;
            sort(numbers.begin(),numbers.end(),comp);
            for(auto num:numbers)
                str = str + to_string(num);
            return str;
        }
        static bool comp(int a,int b)
        {
            string ab = to_string(a) + to_string(b);
            string ba = to_string(b) + to_string(a);
            return ab < ba;
        }
};
```

第n个丑数：

题目描述：把只包含质因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含质因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。

思路：

```C++
class Solution
{
    public:
        int GetUglyNumber_Solution(int index)
        {
            int i2=0,i3=0,i5=0;
            vector<int> nums(1,1);
            while(nums.size()<index)
            {
                int temp = min(min(nums[i2]*2,nums[i3]*3),nums[i5]*5);
                nums.push_back(temp);
                if(temp == nums[i2]*2)
                    i2++;
                if(temp == nums[i3]*3)
                    i3++;
                if(temp == nums[i5]*5)
                    i5++;
            }
            return nums[index-1];       
        }
};
```

### 两个链表的第一个公共结点:

题目描述：输入两个链表，找出它们的第一个公共结点。

思路：

```C++
/*
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
            val(x), next(NULL) {
    }
};*/
class Solution
{
    public:
        ListNode* FindFirstCommonNode( ListNode* pHead1, ListNode* pHead2)
        {
            if(!pHead1 || !pHead2)
                return NULL;
            ListNode* p = pHead1;
            while(p->next)
                p = p->next;
            //将两个链表串起来
            p->next = pHead2;
            ListNode* fast = pHead1;
            ListNode* slow = pHead1;
            int flag = 1;
            while(fast->next && fast->next->next)
            {
                slow = slow->next;
                fast = fast->next->next;
                if(fast == slow)
                {
                    flag = 0;
                    break;
                }
            }
            if(flag)
            {
                p->next = NULL;
                return NULL;
            }
            //有环，得到环的起点
            fast = pHead1;
            while(fast != slow)
            {
                slow = slow->next;
                fast = fast->next;
            }
                //断开两个链表
            p->next = NULL;
            return fast;
        }
};
```

数字在排序数组中出现的次数:

题目描述：统计一个数字在排序数组中出现的次数。

思路：

```C++
class Solution
{
    public:
        int GetNumberOfK(vector<int> data ,int k)
        {
            vector<int> ret(2,-1);
            if(data.empty())
                return 0;
            int left = 0,right = data.size()-1;
            while(left<=right)
            {
                int mid = (left+right)/2;
                if(data[mid]>=k)
                    right = mid-1;
                else
                    left = mid+1;    
            }
            if(left < data.size() && data[left] == k)
                ret[0] = left;
            else
                return 0;
 
            right = data.size()-1;
            while(left<=right)
            {
                int mid = (left+right)/2;
                if(data[mid]<=k)
                    left = mid+1;
                else
                    right = mid-1;     
            }
            ret[1] = right;   //此处不用任何检查
            if(ret[1] == -1)
                return 0;
            return ret[1]-ret[0]+1;
        }
};
```

平衡二叉树:

题目描述：输入一棵二叉树，判断该二叉树是否是平衡二叉树。

思路：

```C++
class Solution
{
    public:
        bool IsBalanced_Solution(TreeNode* pRoot)
        {
            int ret = dfs(pRoot);
            return ret == -1 ? false:true;
        }
        int dfs(TreeNode* root)
        {
            if(!root)
                return 0;
            int left = dfs(root->left);
            if(left == -1)
                return -1;
            int right = dfs(root->right);
            if(right == -1)
                return -1;
            int dif = abs(left-right);
            if(dif > 1)
                return -1;
            else
                return max(left,right)+1;
        }
};
```

### 孩子们的游戏:

题目描述：每年六一儿童节,牛客都会准备一些小礼物去看望孤儿院的小朋友,今年亦是如此。HF作为牛客的资深元老,自然也准备了一些小游戏。其中,有个游戏是这样的:首先,让小朋友们围成一个大圈。然后,他随机指定一个数m,让编号为0的小朋友开始报数。每次喊到m-1的那个小朋友要出列唱首歌,然后可以在礼品箱中任意的挑选礼物,并且不再回到圈中,从他的下一个小朋友开始,继续0...m-1报数....这样下去....直到剩下最后一个小朋友,可以不用表演,并且拿到牛客名贵的“名侦探柯南”典藏版(名额有限哦!!^_^)。请你试着想下,哪个小朋友会得到这份礼品呢？(注：小朋友的编号是从0到n-1)

思路：

```C++
class Solution
{
    public:
        int LastRemaining_Solution(int n, int m)
        {
            if(n < 1 || m < 1)
                return -1;
            int last = 0;
            for(int i = 2; i <= n; i++)
                last = (last + m) % i;
            return last;
        }
};
```

### 链表中环的入口结点:

题目描述：给一个链表，若其中包含环，请找出该链表的环的入口结点，否则，输出null。

思路：请在纸上画图演算，设头节点为A，环入口节点为B，假设快慢指针在环中某点C相遇，则2(AB+BC)=AB+BC+CB+BC，即AB = BC。 

```C++
/*
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
        val(x), next(NULL) {
    }
};
*/
class Solution
{
    public:
        ListNode* EntryNodeOfLoop(ListNode* pHead)
        {
            if(pHead == NULL || pHead->next == NULL)
                return NULL;
            ListNode* fast = pHead;
            ListNode* slow = pHead;
            while(fast->next != NULL && fast->next->next != NULL)
            {
                fast = fast->next->next;
                slow = slow->next;
                if(fast == slow)
                {
                    slow = pHead;
                    while(fast != slow)
                    {
                        fast = fast->next;
                        slow = slow->next;
                    }
                    return fast;   
                }
            }
            return NULL;
        }
};

```

### 打印1到最大的N位数：

题目描述：

思路：使用字符串模拟大数，实现起来还挺麻烦的



### 写一个不能被继承的类：

思路：
1、把构造函数设为私有函数

在C++中子类的构造函数会自动调用父类的构造函数，子类的析构函数也会自动调用父类的构造函数，要想一个类不能被继承，只要把它的构造函数和析构函数都定义为私有函数。

当一个类试图从它那继承的时候，必然会由于调用构造函数、析构函数而导致编译错误。

但如果构造函数 、析构函数都为私有函数，怎么得到该类型的实例呢？可以通过定义公有的静态函数来创建和释放类的实例。

这样我们只能得到位于堆上的实例，而得不到栈上的实例。

代码：

```C++
class SealedClass{
public:
    static SealedClass* GetInstance(){
        return new SealedClass();
    }
 
    static void DeleteInstance(SealedClass* pInstance){
        delete pInstance;
    }
private:
    SealedClass(){};
    ~SealedClass(){};
};
2、虚拟继承

先看代码：


template<typename T>
class MakeSealed{
    friend T;
private:
    MakeSealed(){};
    ~MakeSealed(){};
};
 
class SealedClass:virtual public MakeSealed<SealedClass>{
public:
    SealedClass();
    ~SealedClass();
};
 
class Try:public SealedClass{
public:
    Try(){};
    ~Try(){};
};
```

类SealedClass是从类MakeSealed<SealedClass>继承过来的，在调用Try的构造函数的时候，会跳过SealedClass而直接调用MakeSealed<SealedClass>的构造函数，

但Try不是它的友元类型，因此不能调用它的私有构造函数，因此会编译出错。

缺点：移植性不好，SealedClass在VS能够编译，但GCC对friend的要求不同于VS，暂不支持模板参数类型作为友元类型。