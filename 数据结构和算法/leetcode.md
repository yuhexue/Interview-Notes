 

# 调整数组顺序使奇数位于偶数前面

 

创建[双向队列](http://cuijiahua.com/blog/tag/双向队列/)，遍历数组，奇数前插入，偶数后插入。最后使用assign方法实现不同容器但相容的类型赋值。

class Solution {

public:

​    void reOrderArray(vector<int> &array) {

​        deque<int> result;

​        int num = array.size();

​        for(int i = 0; i < num; i++){

​            if(array[num - i - 1] % 2 == 1){

​                result.push_front(array[num - i - 1]);

​            }

​            if(array[i] % 2 == 0){

​                result.push_back(array[i]);

​            }

​        }

​        array.assign(result.begin(),result.end());

​    }

};

 

 

 

输入一个正整数[数组](http://cuijiahua.com/blog/tag/数组/)，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。

遇到这个题，全排列当然可以做，但是时间复杂度为O(n!)。在这里我们自己定义一个规则，对拼接后的字符串进行比较。

排序规则如下：

§  若ab > ba 则 a 大于 b，

§  若ab < ba 则 a 小于 b，

§  若ab = ba 则 a 等于 b；

 

class Solution {

public:

​    string PrintMinNumber(vector<int> numbers) {

​        int length = numbers.size();

​        if(length == 0){

​            return "";

​        }

​        sort(numbers.begin(), numbers.end(), cmp);

​        string res;

​        for(int i = 0; i <　length; i++){

​            res += to_string(numbers[i]);

​        }

​        return res;

​    }

private:

​    // 升序排序

​    static bool cmp(int a, int b){

​        string A = to_string(a) + to_string(b);

​        string B = to_string(b) + to_string(a);

​        return A < B;

​    }

};

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组,求出这个数组中的逆序对的总数P。并将P对1000000007取模的结果输出。 即输出P%1000000007。

**输入描述：**

题目保证输入的数组中没有的相同的数字

如数组{7,5,6,4}，逆序对总共有5对，{7,5}，{7,6}，{7,4}，{5,4}，{6,4}；

思路1：暴力解法，顺序扫描整个数组，每扫描到一个数字的时候，逐个比较该数字和它后面的数字的大小。如果后面的数字比它小，则这两个数字就组成一个逆序对。假设数组中含有n个数字，由于每个数字都要和O(n)个数字作比较，因此这个算法的时间复杂度是O(n^2)。

思路2：分治思想，采用[归并排序](http://cuijiahua.com/blog/tag/归并排序/)的思路来处理，如下图，先分后治：

class Solution {

public:

​    int InversePairs(vector<int> data) {

​        if(data.size() == 0){

​            return 0;

​        }

​        // 排序的辅助数组

​        vector<int> copy;

​        for(int i = 0; i < data.size(); ++i){

​            copy.push_back(data[i]);

​        }

​        return InversePairsCore(data, copy, 0, data.size() - 1) % 1000000007;

​    }

​    long InversePairsCore(vector<int> &data, vector<int> &copy, int begin, int end){

​        // 如果指向相同位置，则没有逆序对。

​        if(begin == end){

​            copy[begin] = data[end];

​            return 0;

​        }

​        // 求中点

​        int mid = (end + begin) >> 1;

​        // 使data左半段有序，并返回左半段逆序对的数目

​        long leftCount = InversePairsCore(copy, data, begin, mid);

​        // 使data右半段有序，并返回右半段逆序对的数目

​        long rightCount = InversePairsCore(copy, data, mid + 1, end);

​        

​        int i = mid; // i初始化为前半段最后一个数字的下标

​        int j = end; // j初始化为后半段最后一个数字的下标

​        int indexcopy = end; // 辅助数组复制的数组的最后一个数字的下标

​        long count = 0; // 计数，逆序对的个数，注意类型

​        

​        while(i >= begin && j >= mid + 1){

​            if(data[i] > data[j]){

​                copy[indexcopy--] = data[i--];

​                count += j - mid;

​            }

​            else{

​                copy[indexcopy--] = data[j--];

​            }

​        }

​        for(;i >= begin; --i){

​            copy[indexcopy--] = data[i];

​        }

​        for(;j >= mid + 1; --j){

​            copy[indexcopy--] = data[j];

​        }

​        return leftCount + rightCount + count;

​    }

};

 

 

 

 

请实现一个函数，将一个[字符串](http://cuijiahua.com/blog/tag/字符串/)中的空格替换成“%20”。例如，当[字符串](http://cuijiahua.com/blog/tag/字符串/)为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

 

class Solution {

public:

void replaceSpace(char *str,int length) {

if(str == NULL && length <= 0){

​            return;

​        }

​        /*original_length为字符串str的实际长度*/

​        int original_length = 0; //原始长度

​        int number_blank = 0; //空格数

​        int i;

​        while(str[i++] != '\0'){ //遍历字符串

​            ++original_length; //长度+1

​            if(str[i] == ' '){

​                ++number_blank; //遇到空格+1

​            }

​        }

​        /*new_length为把空格替换成'%20'之后的长度*/

​        int new_length = original_length + 2 * number_blank;

​        

​        int index_original = original_length; //原始字符串末尾索引值

​        int index_new = new_length; //计算长度后的字符串末尾索引值

​        

​        /*index_original指针开始向前移动，如果遇到空格，替换成'%20'，否则进行复制操作*/

​        while(index_original >= 0 && index_new > index_original){

​            if(str[index_original] == ' '){

​                str[index_new--] = '0';

​                str[index_new--] = '2';

​                str[index_new--] = '%';

​            }

​            else{

​                str[index_new--] = str[index_original];

​            }

​            --index_original;

​        }

}

};

输入一个[字符串](http://cuijiahua.com/blog/tag/字符串/),按字典序打印出该字符串中字符的所有排列。例如输入字符串abc，则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。

**输入描述：**

输入一个字符串,长度不超过9(可能有字符重复)，字符只包括大小写字母。

 

class Solution {

public:

​    vector<string> Permutation(string str) {

​        //判断输入

​        if(str.length() == 0){

​            return result;

​        }

​        PermutationCore(str, 0);

​        //对结果进行排序

​        sort(result.begin(), result.end());

​        return result;

​    }

​    

private:

​    void PermutationCore(string str, int begin){

​        //递归结束的条件：第一位和最后一位交换完成

​        if(begin == str.length()){

​            result.push_back(str);

​            return;

​        }

​        for(int i = begin; i < str.length(); i++){

​            //如果字符串相同，则不交换

​            if(i != begin && str[i] == str[begin]){

​                continue;

​            }

​            //位置交换

​            swap(str[begin], str[i]);

​            //递归调用，前面begin+1的位置不变，后面的字符串全排列

​            PermutationCore(str, begin + 1);

​        }

​    }

​    vector<string> result;

};

 

## 二、题目

在一个字符串(1<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置。

### 1、思路

建立一个[哈希表](http://cuijiahua.com/blog/tag/哈希表/)，第一次扫描的时候，统计每个字符的出现次数。第二次扫描的时候，如果该字符出现的次数为1，则返回这个字符的位置。

class Solution {

public:

​    int FirstNotRepeatingChar(string str) {

​        int length = str.size();

​        if(length == 0){

​            return -1;

​        }

​        map<char, int> item;

​        for(int i = 0; i < length; i++){

​            item[str[i]]++;

​        }

​        for(int i = 0; i < length; i++){

​            if(item[str[i]] == 1){

​                return i;

​            }

​        }

​        return -1;

​    }

};

 

 

## 二、题目

汇编语言中有一种移位指令叫做循环左移（ROL），现在有个简单的任务，就是用[字符串](http://cuijiahua.com/blog/tag/字符串/)模拟这个指令的运算结果。对于一个给定的字符序列S，请你把其循环左移K位后的序列输出。例如，字符序列S=”abcXYZdef”,要求输出循环左移3位后的结果，即“XYZdefabc”。是不是很简单？OK，搞定它！

### 1、思路

例如：输入[字符串](http://cuijiahua.com/blog/tag/字符串/)"abcdefg"和数字2，该函数将返回左旋转2位得到的结果"cdefgab";

 

第一步：翻转字符串“ab”，得到"ba"；

第二步：翻转字符串"cdefg"，得到"gfedc"；

第三步：翻转字符串"bagfedc"，得到"cdefgab"；

或者：

第一步：翻转整个字符串"abcdefg",得到"gfedcba"

第二步：翻转字符串“gfedc”，得到"cdefg"

第三步：翻转字符串"ba",得到"ab"

### 2、代码

class Solution {

public:

​    string LeftRotateString(string str, int n) {

​        string result = str;

​        int length = result.size();

​        if(length < 0){

​            return NULL;

​        }

​        if(0 <= n <= length){

​            int pFirstBegin = 0, pFirstEnd = n - 1;

​            int pSecondBegin = n, pSecondEnd = length - 1;

​            ReverseString(result, pFirstBegin, pFirstEnd);

​            ReverseString(result, pSecondBegin, pSecondEnd);

​            ReverseString(result, pFirstBegin, pSecondEnd);

​        }

​        return result;

​    }

private:

​    void ReverseString(string &str, int begin, int end){

​        while(begin < end){

​            swap(str[begin++], str[end--]);

​        }

​    }

};

 

 

 

 

 

 

一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

f(n)=f(n-1)+f(n-2)。

一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

跳法f(n)=2^(n-1)。

 

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

将结果存入vector[数组](http://cuijiahua.com/blog/tag/数组/)，从左到右，再从上到下，再从右到左，最后从下到上遍历。

class Solution {

public:

​    vector<int> printMatrix(vector<vector<int> > matrix) {

​        int rows = matrix.size(); //行数

​        int cols = matrix[0].size(); //列数

​        vector<int> result;

​        

​        if(rows == 0 && cols == 0){

​            return result;

​        }

​        int left = 0, right = cols - 1, top = 0, bottom = rows - 1;

​        

​        while(left <= right && top <= bottom){

​            //从左到右

​            for(int i = left; i <= right; ++i){

​                result.push_back(matrix[top][i]);

​            }

​            //从上到下

​            for(int i = top + 1; i <= bottom; ++i){

​                result.push_back(matrix[i][right]);

​            }

​            //从右到左

​            if(top != bottom){

​                for(int i = right - 1; i >= left; --i){

​                    result.push_back(matrix[bottom][i]);

​                }

​            }

​            //从下到上

​            if(left != right){

​                for(int i = bottom - 1; i > top; --i){

​                    result.push_back(matrix[i][left]);

​                }

​            }

​            left++, top++, right--, bottom--;

​        }

​        return result;

​    }

};

 

 

输入一个整数n，求从1到n这n个整数的十进制表示中1出现的次数。例如输入12，从1到12这些整数中包含1的数字有1，10，11和12，1一共出现了5次。

class Solution {

public:

​    int NumberOf1Between1AndN_Solution(int n)

​    {

​        // 统计次数

​        int count = 0;

​        for(int i = 1; i <= n; i *= 10){

​            // 计算高位和低位

​            int a = n / i, b = n % i;

​            count += (a + 8) / 10 * i + (a % 10 == 1) * (b + 1);

​        }

​        return count;

​    }

};

输出所有和为S的连续正数序列。序列内按照从小至大的顺序，序列间按照开始数字从小到大的顺序。



 

由于我们要找的是和为S的连续正数序列，因此这个序列是个公差为1的等差数列，而这个序列的中间值代表了平均值的大小。假设序列长度为n，那么这个序列的中间值可以通过（S / n）得到，知道序列的中间值和长度，也就不难求出这段序列了。 

2）满足条件的n分两种情况： 

n为奇数时，序列中间的数正好是序列的平均值，所以条件为：(n & 1) == 1 && sum % n == 0；

n为偶数时，序列中间两个数的平均值是序列的平均值，而这个平均值的小数部分为0.5，所以条件为：(sum % n) * 2 == n.

3）由题可知n >= 2，那么n的最大值是多少呢？我们完全可以将n从2到S全部遍历一次，但是大部分遍历是不必要的。为了让n尽可能大，我们让序列从1开始， 

根据等差数列的求和公式：S = (1 + n) * n / 2，得到. 

 

最后举一个例子，假设输入sum = 100，我们只需遍历n = 13~2的情况（按题意应从大到小遍历），n = 8时，得到序列[9, 10, 11, 12, 13, 14, 15, 16]；n  = 5时，得到序列[18, 19, 20, 21, 22]。 

完整代码：时间复杂度为 

| 1   2   3   4   5   6   7   8   9   10   11   12   13   14   15   16 | `import` `java.util.ArrayList;`   `public` `class` `Solution {`   `    public` `ArrayList<ArrayList<Integer> >   FindContinuousSequence(int` `sum) {`   `          ArrayList<ArrayList<Integer>> ans = new` `ArrayList<>();`   `        for` `(int` `n = (int)   Math.sqrt(2` `* sum); n >= 2; n--) {`   `            if` `((n & 1) == 1` `&&   sum % n == 0` `|| (sum % n) * 2` `== n) {`   `                  ArrayList<Integer> list = new` `ArrayList<>();`   `                  for` `(int` `j = 0, k = (sum / n) - (n - 1) / 2; j < n; j++, k++)   {`   `                      list.add(k);`   `                  }`   `                  ans.add(list);`   `            }`   `        }`   `        return` `ans;`   `    }`   `}` |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
|                                                              |                                                              |

 

 

 

 

输入一个递增排序的[数组](http://cuijiahua.com/blog/tag/数组/)和一个数字S，在数组中查找两个数，是的他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。

对于一个数组，我们可以定义两个指针，一个从左往右遍历（pleft），另一个从右往左遍历（pright）。首先，我们比较第一个数字和最后一个数字的和curSum与给定数字sum，如果curSum < sum，那么我们就要加大输入值，所以，pleft向右移动一位，重复之前的计算；如果curSum > sum，那么我们就要减小输入值，所以，pright向左移动一位，重复之前的计算；如果相等，那么这两个数字就是我们要找的数字，直接输出即可。

这么做的好处是，也保证了乘积最小。

 

class Solution {

public:

​    vector<int> FindNumbersWithSum(vector<int> array,int sum) {

​        vector<int> result;

​        int length = array.size();

​        if(length < 1){

​            return result;

​        }

​        int pright = length - 1;

​        int pleft = 0;

​        

​        while(pright > pleft){

​            int curSum = array[pleft] + array[pright];

​            if(curSum == sum){

​                result.push_back(array[pleft]);

​                result.push_back(array[pright]);

​                break;

​            }

​            else if(curSum < sum){

​                pleft++;

​            }

​            else{

​                pright--;

​            }

​        }

​        return result;

​    }

};

 

 

请实现一个函数用来找出字符流中第一个只出现一次的字符。例如，当从字符流中只读出前两个字符"go"时，第一个只出现一次的字符是"g"。当从该字符流中读出前六个字符“google"时，第一个只出现一次的字符是"l"。如果当前字符流没有存在出现一次的字符，返回#字符。

 

class Solution

{

public:

  //Insert one char from stringstream

​    void Insert(char ch)

​    {

​        s += ch;

​        count[ch]++;

​    }

  //return the first appearence once char in current stringstream

​    char FirstAppearingOnce()

​    {

​        int length = s.size();

​        for(int i = 0; i < length; i++){

​            if(count[s[i]] == 1){

​                return s[i];

​            }

​        }

​        return '#';

​    }

private:

​    string s;

​    int count[256] = {0};

};

 

 

 原则：多写多练，笔记要精简为思想。

 

Find-kth问题：

 

class Solution 

{

​    public:     //find k-th问题

​        double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) 

​        {

​            int len = nums1.size() + nums2.size();

​            if(len == 0)

​                return 0;

​            if(len%2 == 1)

​                return find_kth(nums1,nums1.size(),0,nums2,nums2.size(),0,len/2+1);

​            else

​            {

​                double ret1 = find_kth(nums1,nums1.size(),0,nums2,nums2.size(),0,len/2);

​                double ret2 = find_kth(nums1,nums1.size(),0,nums2,nums2.size(),0,len/2+1);

​                return (ret1 + ret2)/2;

​            }               

​        }

​        double find_kth(vector<int>& nums1,int len1,int i1,vector<int>& nums2,int len2,int i2,int k)

​        {

​            if(len1 > len2) 

​                return find_kth(nums2,len2,i2,nums1,len1,i1,k);

​            if(len1 == 0) 

​                return nums2[i2+k-1];

​            if(k == 1)                    //这里1，2都要非空，故len1==0要在前面

​                return min(nums1[i1],nums2[i2]);

​            

​            **int pa = min(k/2,len1), pb = k-pa;**

​            **if(nums1[i1+pa-1] > nums2[i2+pb-1])**

​                **return find_kth(nums1,len1,i1,nums2,len2-pb,i2+pb,k-pb);**

​            **else if(nums1[i1+pa-1] < nums2[i2+pb-1])**

​                **return find_kth(nums1,len1-pa,i1+pa,nums2,len2,i2,k-pa);** 

​            **else**

​                **return nums1[i1+pa-1];**            

​        }

};

 

 

 

 

 

 

最长回文字串：

动态规划解法，复杂度n2，dp[i][j]表示区间[i,j]是否回文。

class Solution 

{

​    public:

​        string longestPalindrome(string s) 

​        {

​            int len = 0,left = 0,size = s.size();

​            string str;

​            if(!size)

​                return str;

​            int dp[size][size] = {0};

​            for(int i = 0;i < s.size();++i)

​            {

​                for(int j = i;j >= 0;--j)

​                {

​                    dp[j][i] = (s[i] == s[j] && (i - j < 2 || dp[j + 1][i - 1]));

​                    if(dp[j][i] && len < i-j+1)

​                    {

​                        len = i-j+1;

​                        left = j;

​                    }

​                }

​            }

​            return s.substr(left,len);

​        }

};

 

k数之和：哈希表法，先排序后夹逼法（节省一个时间复杂度）

 

 

 

树相关：

遍历的非递归写法：

 

//前序遍历，循环方法（用栈记录填入）

vector<int> PreorderTravelStack(TreeNode *root)

{

​       vector<int> ret;

​       if (!root)

​              return ret;    //返回一个空动态数组

​       vector<TreeNode*> tree;  //定义一个记录节点的动态数组

​       tree.push_back(root);

​       while (!tree.empty())

​       {

​              TreeNode* temp = tree.back();  //数据装入

​              ret.push_back(temp->val);

​              tree.pop_back();

 

​              if (!temp->right)

​                     tree.push_back(temp->right);

​              if (!temp->right)

​                     tree.push_back(temp->right);

​       }

​       return ret;

}

 

//中序遍历，注意p->left, p, p->right的使用顺序决定了遍历顺序

vector<TreeNode*> nodes;

TreeNode* p = root;

while(p || !nodes.empty()) 

{

while(p)

 {

nodes.push_back(p);

p = p->left;

}

if(!nodes.empty())

 {

p = nodes.back();

vals.push_back(p->val);

nodes.pop_back();

p = p->right;

}

}

树的非递归遍历，前序，中序，后序难度依次增大。

 

后序遍历算法原理：

当遍历到一个节点的时候，首先我们将右子树压栈，然后将左子树压栈。这里需要注意一下出栈的规则，对于叶子节点来说，直接可以出栈，但是对于根节点来说，我们需要一个变量记录上一次出栈的节点，如果上一次出栈的节点是该根节点的左子树或者右子树，那么该根节点可以出栈，否则这个根节点是新访问的节点，将右和左子树分别压栈。

while(!nodes.empty())

 {

TreeNode* p = nodes.back();

if((p->left == NULL && p->right == NULL) ||

(pre != NULL && (pre == p->left || pre == p->right)))    //要么出栈

{

vals.push_back(p->val);

nodes.pop_back();

pre = p;                                                  //用完p后，更新给pre

} 

else                                                                           //要么压栈

{

if(p->right != NULL) 

nodes.push_back(p->right);

if(p->left != NULL) {

nodes.push_back(p->left);

}

}

 

 

 

 

 

 

 

 

判断平衡二叉树，DFS遍历，从下往上检测 , 递归

int getHeight(TreeNode* root)                                                          

{

​       if (root == NULL)

​              return 0;

​       int leftHeight = getHeight(root->left);       //递归写在基本情况的前面

​       if (leftHeight == -1)

​              return -1;

​       int rightHeight = getHeight(root->right);

​       if (rightHeight == -1)

​              return -1;

​       int diffHeight = abs(leftHeight - rightHeight);

​       if (diffHeight > 1)

​              return -1;

​       else

​              return diffHeight = max(leftHeight , rightHeight) + 1;  

}

 

 

Dp问题：

 

两次收益问题：学习这种O(n)时间复杂度的解法

for(int i = 1; i < len; i++)

 {

minP = min(minP, prices[i - 1]);

profits[i] = max(sum, prices[i] - minP);

sum = profits[i];

}

int maxP = prices[len - 1];

int sum2 = numeric_limits<int>::min();

//逆向遍历

for(int i = len - 2; i >= 0; i--)

 {

maxP = max(maxP, prices[i + 1]);

sum2 = max(sum2, maxP - prices[i]);

if(sum2 > 0) 

{

profits[i] = profits[i] + sum2;

sum = max(sum, profits[i]);

}

}

 

三角形动态规划：

求出一个三角形中从顶到底最小路径和，并且要求只能使用O(n)的空间。有两种解法，自顶向下以及自底向上。

一定要先明确DP方程，以dp[m][n]表示第m行第n个节点的最小路径和，

dp[m + 1][n] = min(dp[m][n], dp[m][n - 1]) + triangle[m + 1][n]      （if n > 0）

dp[m + 1][0] = dp[m][0] + triangle[m + 1][0]

 

程序源码：时间n2，空间n

int Triangle(vector<vector<int>>& num)

{

​       int res = INT_MAX;

​       int row = num.size();

​       vector<int> dp(row,INT_MAX);

​       dp[0] = num[0][0];

​       for (int i = 1; i < row; ++i)

​       {

​              for (int j = i; j >= 0; --j)        //注意1

​              {

​                     if (j == 0)

​                            dp[j] = dp[j] + num[i][j];

​                     else

​                            dp[j] = min(dp[j],dp[j-1]) + num[i][j];    //注意2处

​              }

​       }

​       for (int i = 0; i < row; ++i)

​       {

​              res = min(res,dp[i]);

​       }

​       return res;

}

由于空间复杂度要求n，故用到滚动方法，内涵是;

在外部循环体的角度里，是将上一次外部循环的dp[j]和dp[j-1]更新给下一层的dp[j].

在内部循环体的角度里，由于dp方程要用到dp[j-1],故每层的更新是从尾到头。

总之，每一次外部循环会把上一层的dp传递给下一层(隐性表示i和i+1)，每层的更新要按照从尾到头（用到dp[j-1]，尾更新前头不能变）

 

同理：从下往上的方法自行练习.

int Triangle(vector<vector<int>>& num)

{

​       int res = INT_MAX;

​       int row = num.size();

​       vector<int> dp(row);

​       for (int i = 0; i < row; ++i)

​       {

​              dp[i] = num[row - 1][i];

​       }

​       for (int i = row-1; i >= 0; --i)

​       {

​              for (int j = 0; j <= i; ++j)

​              {

​                     dp[j] = min(dp[j],dp[j+1]) + num[i][j];

​              }

​       }

​       return dp[0];

}

要初始化,且只有dp[0会更新彻底,也就是最后的结果,省去了找min的过程.

 

 

 

1到n的数值组成BST树,有多少种组成方式:

DP方程:  dp[n ] = sum {dp[k]*dp[n-k-1]}

 

vector<int> dp(n + 1, 0);

dp[0] = 1;

dp[1] = 1;

for(int i = 2; i <= n; i++) 

{

for(int j = 0; j < i; j++) 

{

//如果左子树的个数为j，那么右子树为i - j - 1

dp[i] += dp[j] * dp[i - j - 1];

}

}

return dp[n];

首先左右子树之间肯定是乘法关系

***核心:  注意仔细深刻分析这个DP方程的动态迭代过程,在每一个内部循环中,会将j个各种情况产生的数量进行加和,并最终给到dp[i].

在外部循环中,实际上产生了新的i(从前往后),新的dp[i]初始值也是0,重复遍历dp[j]的过程.实际上,只是遍历j会累积(加和),新i和旧i并没有累积关系.

 

一个目标数,使用最少量的数,让这些数的平方和等于该数.

int numSquares(int n)

 {

vector<int> dp(n + 1, INT_MAX);

dp[0] = 0;

for (int i = 0; i <= n; i++) 

{

for (int j = 1; i + j * j <= n; j++) 

{

dp[i + j * j] = min(dp[i + j * j], dp[i] + 1);

}

}

return dp[n];

}

 

 

 

Sort Linked List:

 

ListNode *sortList(ListNode *head) 

{

if(head == NULL || head->next == NULL)

 { return head; }

ListNode* fast = head;

ListNode* slow = head;

//快慢指针得到中间点

while(fast->next && fast->next->next) 

{

fast = fast->next->next;

slow = slow->next;

}

//将链表拆成两半

fast = slow->next;

slow->next = NULL;

//左右两半分别排序

ListNode* p1 = sortList(head);

ListNode* p2 = sortList(fast);

//合并

return merge(p1, p2);

}

 

ListNode *merge(ListNode* l1, ListNode* l2) 

{

if(!l1) 

{ return l2; } 

else if (!l2) 

{ return l1; } 

else if (!l1 && !l2) 

{ return NULL; }

ListNode dummy(0);

ListNode* p = &dummy;

while(l1 && l2)

 {

if(l1->val < l2->val) 

{

p->next = l1;

l1 = l1->next;

} 

else

 {

p->next = l2;

l2 = l2->next;

}

p = p->next;

}

if(l1) 

{ p->next = l1; } 

else if(l2)

{ p->next = l2; }

return dummy.next;

}

注意前一个函数不断递归(不断二分),每层递归完后进行归并(类似于数组归并排序).

 

 

 

 

 

第k大数：

bool cmp(T ,x, T y)

{

if(x>y)return 1;

else 

return 0;

}

如果0, 那么函数就会将他们互换位置, 1就会保持原来位置不变。

简洁写法：默认升序，不用改写这里只写降序

bool cmp(T ,x, T y)

{

return x>y;

}

这里介绍algorithm里面的标准函数：

nth_element(v.begin(), v.begin() + k, v.end(),cmp);

平均复杂度保证On，迭代器表列abc里保证（ab）小于（bc），但是不保证ab和bc内部有序。

以升序为例，索引值为b的元素实际上就是第b+1小的数。

 

原理：

 

在STL库中，nth_element的实现是基于快速排序的partition的过程。

 

0）分两种，第一，如果处理的元素的个数小于某一个阈值（此处为3）。那么使用插入排序，否则，转1）

1）选出pivot，使用的是first，last，以及中间元素的中间值，（其实，在快速排序中，这样选择pivot可能导致最坏时间复杂度的概率已经是可以忽略不计了），根据该pivot的值将数组进行划分，得到A[ first ,.... ,cut] , 与 A[cut  ... last]

2） 如果小于pivot的元素个数已经是K，直接返回。

3） 如果小于pivot的元素小于K，那么在 大于pivot的部分寻找第K - ( cut - first +1 ) 大的元素。转入1），递归即可。

4） 如果小于pivot的元素大于K，那么在 小于pivot的部分继续寻找第 K大 的 元素。转入1），转入1），递归即可。

 

为什么pivot使用的是first，last，以及中间元素的中间值，因为在已经排好序，逆序，所有元素都相等这些最差的情况里，pivot如果是某一个元素，如last,递归要进行n次。

 

 

 

 

 

TopK问题：分块，堆排On*k

排序部分：

最基础：快排On*logn(全部排序)

部分排序且内部有序：选择或者冒泡Ok*n(常规堆排预建堆要On*logn)

部分排序且内部无序：堆排On*lgk

快排的partition平均复杂度On

如果是大数据背景，要对数据进行拆分，因为单机可能无法处理或处理速度慢，可以做分布式处理。之后进行排序：先找出前k（内部无序），之后统一排序

如果要求输出内部无序的前k个数，复杂性与查找第k小的数复杂性相同，为O(n)

如果是有序的前k个数，复杂性在O(n)的基础上，还要加上对前k个数排序的复杂性O(klogk)，复杂性为O(n+klogk)

 

 

Merge k Sorted Lists

思路一：

参考归并排序的思路，对k个链表进行分组，最后两两归并，最终合成一个链表，具体代码实现如下所示

class Solution {

public:

​    ListNode* mergeKLists(vector<ListNode*>& lists) {

​        if(lists.size() == 0) return NULL;

​        return helper(lists, 0, lists.size()-1);

​    }

​    ListNode* helper(vector<ListNode*>& lists, int left, int right) {

​        if(left < right) {

​            int mid = (left + right)/2;

​            return merge(helper(lists, left, mid), helper(lists, mid+1, right));

​        }

​        return lists[left];

​    }

​    ListNode* merge(ListNode * l1, ListNode * l2) {

​        ListNode * dummy = new ListNode(0);

​        ListNode * cur = dummy;

​        while(l1 != NULL && l2 != NULL) {

​            if(l1->val < l2->val) {

​                cur->next = l1;

​                l1 = l1->next;

​                cur = cur->next;

​            }

​            else {

​                cur->next = l2;

​                l2 = l2->next;

​                cur = cur->next;

​            }

​        }

​        if(l1 != NULL)

​            cur->next = l1;

​        else

​            cur->next = l2;

​        cur = dummy->next;

​        delete dummy;

​        return cur;

​    } 

};

时间复杂度为O(nklognk)，因为这里以nk为基础，空间复杂度为O(logk)

 

 

求纵数问题：哈希表法On和On，更好的方法：摩尔投票法，On和O1

class Solution 

{

​    public:

​        int majorityElement(vector<int>& nums) 

​        {

​            int count = 1, temp = nums[0];

​            for(int i = 1;i < nums.size();++i)

​            {

​                if(nums[i] == temp)

​                    count++;

​                else

​                {

​                    count--;

​                    if(count == 0)

​                    {

​                        count = 1;

​                        temp = nums[i];

​                    }

​                }      

​            }

​            return temp;   

​        }

};

 

 

鸡蛋掉落问题：

 

 

 

 

hash_map在C++11被废弃。map的内部结构是红黑树来实现的，有序排列，所以保证了一个稳定的动态操作时间，查询、插入、删除都是O（logn），最坏和平均都是。而unordered_map如前所述，是哈希表。哈希表的查询时间虽然是O（1），但是并不是unordered_map查询时间一定比map短，因为实际情况中还要考虑到数据量，而且unordered_map的hash函数的构造速度也没那么快，所以不能[一概而论](https://www.baidu.com/s?wd=一概而论&tn=24004469_oem_dg&rsv_dl=gh_pl_sl_csd)，应该具体情况具体分析。

**三、unordered_map与unordered_set**

后者就是在哈希表插入value，而这个value就是它自己的key，而不是像之前的unordered_map那样有键-值对，这里单纯就是为了方便查询这些值。我在Longest Consecutive Sequence这一题，我的方法一就是用了unordered_set，同样是一个将空间弥补时间的方法。

 

总之，set强调单值，map强调键值对，都是红黑树实现。unordered强调无序，用哈希表实现，增快查找效率。Multi强调可重复。

 

1.vector 底层数据结构为数组 ，支持快速随机访问

2.list 底层数据结构为双向链表，支持快速增删，stl的list是双向链表。

 

 

 

 

| http  | 超文本传输协议     | 以   http:// 开头的普通网页。不加密。 |
| ----- | ------------------ | ------------------------------------- |
| https | 安全超文本传输协议 | 安全网页，加密所有信息交换。          |
| ftp   | 文件传输协议       | 用于将文件下载或上传至网站。          |

Url绝对路径和相对路径，网址和文件路径。

 

 

三次握手的攻击：

如果第三次握手失败，服务器会给客户端发送消息，然后关闭等待。三次握手可能受到的攻击比如Ddos攻击，伪造不存在的源地址，使服务器处于半连接的状态，防范方法是缩短等待时长（即第三次等待的时间）。

 

 

2MSL：光1msl保证发送的报文要消失还不够，还要保证应答的也要消失，服务端在timeout(超时)后重发，所以要保证2msl。

 

2、初始化类的成员有两种方式，一是使用初始化列表，二是在构造函数体内进行赋值操作。因此，由于常量只能初始化不能赋值，所以常量成员必须使用初始化列表；[1]

 

(当然你可以在类定义的时候，就对const成员变量进行赋值: class A{const int a = 1;}；但是这样操作的话，这个变量就失去了意义，即基于这个类生成的所有对象a的值都为1！

 

再更进一步的讲，其实非const变量也可以在类定义的时候就进行赋值的操作，但是static变量不可以！）

 

 

1.**ps****命令**——查看静态的进程统计信息（Processes Statistic）

常见的选项：

a：显示当前终端下的所有进程信息，包括其他用户的进程。

u：使用以用户为主的格式输出进程信息。

x：显示当前用户在所有终端下的进程。

-e：显示系统内的所有进程信息。

ps aux 将以简单列表的形式显示出进程信息

 

结合管道操作和grep命令进行过滤，用于查询某一个进程的信息。

top命令提供了运行中系统的动态实时视图。在命令提示行中输入top：

 

\3. kill 命令用于终止进程
 例如： kill -9 [PID]
 -9 表示强迫进程立即停止
 通常用 ps 查看进程 PID ，用 kill 命令终止进程

 

查看进程端口号：

### 1、先查看进程pid

ps -ef | grep 进程名

### 2、通过pid查看占用端口

netstat -nap | grep 进程pid

通过端口查看进程：

netstat -nap | grep 端口

 

 

### 1、特点

\1.       

共享内存是最快的一种 IPC，因为进程是直接对内存进行存取。

\2.       

\3.       

因为多个进程可以同时操作，所以需要进行同步。

\4.       

\5.       

信号量+共享内存通常结合在一起使用，信号量用来同步对共享内存的访问。

\6.       

 

主键不能重复，不能为空，唯一键不能重复，可以为空。

追问

```
这样的目的是做什么的啊,为什么一个空一个可以不为空?
```

追答

```
主键是用来标识数据库中一条记录的，这条记录是唯一的，所以主键可以是一个字段，或者多个字段做联合主键。

唯一键只作用在一个字段上，使该字段中的值不重复。
 
新增元素：Vector通过一个连续的数组存放元素，如果集合已满，在新增数据的时候，就要分配一块更大的内存，将原来的数据复制过来，释放之前的内存，在插入新增的元素；
对vector的任何操作，一旦引起空间重新配置，指向原vector的所有迭代器就都失效了 ；
初始时刻vector的capacity为0，塞入第一个元素后capacity增加为1；
不同的编译器实现的扩容方式不一样，VS2015中以1.5倍扩容，GCC以2倍扩容。
```

 

 

 

 

·         

 

```
Next节点：仅适用完全二叉树的边界条件
class Solution {
public:
    void connect(TreeLinkNode *root) {
        if (root == NULL || root->left == NULL)
            return;
        root->left->next = root->right;
        if (root->next)  //if(root->right && root->next)
            root->right->next = root->next->left;
        connect(root->left);
        connect(root->right);
    }
};
```

 

 

普通二叉树：   //切记，以右为尊，确定了右才能确定左边

class Solution {

public:

​    Node* connect(Node* root) {

​        if (!root) return NULL;

​        Node *next = root->next;

​        while (next) {

​            if (next->left) {

​                next = next->left;

​                break;

​            } else if (next->right) {

​                next = next->right;

​                break;

​            } else next = next->next;

​        }

​        if (root->left && root->right) {

​            root->left->next = root->right;

​            root->right->next = next;

​        } else if (!root->left && root->right) root->right->next = next;

​        else if (root->left && !root->right) root->left->next = next;

​        connect(root->right);

​        connect(root->left);

​        return root;

​    }

};

 

 

 

 

 

//73题，要求原地

空间复杂度 O(2) ，用两个布尔变量就可以解决。方法就是利用数组的首行和首列来记录 0 值。从数组下标的 A[1][1] 开始遍历，两个布尔值记录首行首列是否需要置0

 

 

 

哈希表经常用来统计个数，统计元素：

​        vector<vector<string>> groupAnagrams(vector<string>& strs) 

​        {

​            vector<vector<string>> ret;

​            if(strs.empty())

​                return ret;

​            unordered_map<string,vector<string>> map;

​            for(int i = 0;i<strs.size();++i)

​            {

​                string temp = strs[i];

​                sort(temp.begin(),temp.end());

​                map[temp].push_back(strs[i]);    

​            }

​            for(auto m:map)

​                ret.push_back(m.second);  

​            return ret;

​        }

 //这里有个去重的bug，对每组，可能有多个重复元素

 

 

 

 

子集II去重的思考：

class Solution {

public:

vector<vector<int> > subsetsWithDup(vector<int> &S) {

sort(S.begin(), S.end()); // 必须排序

vector<vector<int> > result;

vector<int> path;

dfs(S, S.begin(), path, result);

return result;

}

private:

static void dfs(const vector<int> &S, vector<int>::iterator start,

vector<int> &path, vector<vector<int> > &result) {

result.push_back(path);

for (auto i = start; i < S.end(); i++) {              //start代表遍历的起点，left

if (i != start && *i == *(i-1)) continue;            //算法核心

path.push_back(*i);

dfs(S, i + 1, path, result);

path.pop_back();

}

}

};

什么时候出现重复结果（在非起点位置添加重复元素），在添加新元素时，出现重复元素。

 

 

 

法二：位向量，记录各个元素出现的次数，根据各元素次数构成子集

 

 

 

```
Lambda表达式的使用
sort(intervals.begin(), intervals.end(), [](const Interval& a, const Interval& b){
            return a.start < b.start;});
```

 

 

搜索二维矩阵II：

可以发现左下角和右上角的数很有特点。左下角的18，往上所有的数变小，往右所有数增加，那么我们就可以和目标数相比较，如果目标数大，就往右搜，如果目标数小，就往上搜。。当然也可以把起始数放在右上角，往左和下搜。

 

 

 

```
/**
 * 根据题意，从最后一天推到第一天，这样会简单很多。因为最后一天显然不会再有升高的可能，结果直接为0。
 * 再看倒数第二天的温度，如果比倒数第一天低，那么答案显然为1，如果比倒数第一天高，又因为倒数第一天
 * 对应的结果为0，即表示之后不会再升高，所以倒数第二天的结果也应该为0。
 * 自此我们容易观察出规律，要求出第i天对应的结果，只需要知道第i+1天对应的结果就可以：
 * - 若T[i] < T[i+1]，那么res[i]=1；
 * - 若T[i] > T[i+1]
 *   - res[i+1]=0，那么res[i]=0;
 *   - res[i+1]!=0，那就比较T[i]和T[i+1+res[i+1]]（即将第i天的温度与比第i+1天大的那天的温度进行比较）
 */public int[] dailyTemperatures(int[] T) {
    int[] res = new int[T.length];
    res[T.length - 1] = 0;
    for (int i = T.length - 2; i >= 0; i--) {
        for (int j = i + 1; j < T.length; j += res[j]) {
            if (T[i] < T[j]) {
                res[i] = j - i;
                break;
            } else if (res[j] == 0) {
                res[i] = 0;
                break;
            }
        }
    }
    return res;
}
```

 

 

  

判断平衡二叉树，DFS遍历，从下往上检测

int getHeight(TreeNode* root)                                                          

{

​       if (root == NULL)

​              return 0;

​       int leftHeight = getHeight(root->left);       //递归写在基本情况的前面

​       if (leftHeight == -1)

​              return -1;

​       int rightHeight = getHeight(root->right);

​       if (rightHeight == -1)

​              return -1;

​       int diffHeight = rightHeight > leftHeight ? rightHeight - leftHeight : leftHeight - rightHeight;

​       if (diffHeight > 1)

​              return -1;

​       else

​              return diffHeight = (rightHeight>leftHeight ? rightHeight : leftHeight) + 1;

}

画图理解树的DFS遍历过程。

 

//前序遍历，递归方法

void PreorderTravel(TreeNode *root,vector<int>& array)

{

​       if (!root)

​              return;

​       else

​              array.push_back(root->val);

​       PreorderTravel(root->left,array);

​       PreorderTravel(root->right, array);

}

 

//前序遍历，循环方法（用栈记录填入）

vector<int> PreorderTravelStack(TreeNode *root)

{

​       vector<int> ret;

​       if (!root)

​              return ret;    //返回一个空动态数组

​       vector<TreeNode*> tree;  //定义一个记录节点的动态数组

​       tree.push_back(root);

​       while (!tree.empty())

​       {

​              TreeNode* temp = tree.back();  //数据装入

​              ret.push_back(temp->val);

​              tree.pop_back();

 

​              if (!temp->right)

​                     tree.push_back(temp->right);

​              if (!temp->right)

​                     tree.push_back(temp->right);

​       }

​       return ret;

}

 

//中序遍历，注意p->left, p, p->right的使用顺序决定了遍历顺序

vector<TreeNode*> nodes;

TreeNode* p = root;

while(p || !nodes.empty()) 

{

while(p)

 {

nodes.push_back(p);

p = p->left;

}

if(!nodes.empty())

 {

p = nodes.back();

vals.push_back(p->val);

nodes.pop_back();

p = p->right;

}

}

树的非递归遍历，前序，中序，后序难度依次增大。

 

后序遍历算法原理：

当遍历到一个节点的时候，首先我们将右子树压栈，然后将左子树压栈。这里需要注意一下出栈的规则，对于叶子节点来说，直接可以出栈，但是对于根节点来说，我们需要一个变量记录上一次出栈的节点，如果上一次出栈的节点是该根节点的左子树或者右子树，那么该根节点可以出栈，否则这个根节点是新访问的节点，将右和左子树分别压栈。

while(!nodes.empty())

 {

TreeNode* p = nodes.back();

if((p->left == NULL && p->right == NULL) ||

(pre != NULL && (pre == p->left || pre == p->right)))    //要么出栈

{

vals.push_back(p->val);

nodes.pop_back();

pre = p;                                                  //用完p后，更新给pre

} 

else                                                                           //要么压栈

{

if(p->right != NULL) 

nodes.push_back(p->right);

if(p->left != NULL) {

nodes.push_back(p->left);

}

}

 

 

 

 

 

 

 

 

 

有序链表到二叉搜索树（BST）：

TreeNode* BalanceTree(ListNode* head)

{

​       return travel(head,NULL);   //NULL填充

}

 

TreeNode* travel(ListNode* start, ListNode* end)

{

​       if (start == end)

​              return NULL;

​       ListNode* fast = start;

​       ListNode* slow = start;

​       while (fast != end && fast->next != end)        //fast->next

​       {

​              fast = fast->next->next;

​              slow = slow->next;

​       }

​       TreeNode* tree = new TreeNode(slow->val);

​       tree->left = travel(start,slow);

​       tree->right = travel(slow->next,end);

}

注意这里NULL填充和fast->next的作用：处理链表的单双数目。

 

两次收益问题：学习这种O(n)时间复杂度的解法

for(int i = 1; i < len; i++)

 {

minP = min(minP, prices[i - 1]);

profits[i] = max(sum, prices[i] - minP);

sum = profits[i];

}

int maxP = prices[len - 1];

int sum2 = numeric_limits<int>::min();

//逆向遍历

for(int i = len - 2; i >= 0; i--)

 {

maxP = max(maxP, prices[i + 1]);

sum2 = max(sum2, maxP - prices[i]);

if(sum2 > 0) 

{

profits[i] = profits[i] + sum2;

sum = max(sum, profits[i]);

}

}

 

三角形动态规划：

求出一个三角形中从顶到底最小路径和，并且要求只能使用O(n)的空间。有两种解法，自顶向下以及自底向上。

一定要先明确DP方程，以dp[m][n]表示第m行第n个节点的最小路径和，

dp[m + 1][n] = min(dp[m][n], dp[m][n - 1]) + triangle[m + 1][n]      （if n > 0）

dp[m + 1][0] = dp[m][0] + triangle[m + 1][0]

 

程序源码：时间n2，空间n

int Triangle(vector<vector<int>>& num)

{

​       int res = INT_MAX;

​       int row = num.size();

​       vector<int> dp(row,INT_MAX);

​       dp[0] = num[0][0];

​       for (int i = 1; i < row; ++i)

​       {

​              for (int j = i; j >= 0; --j)        //注意1

​              {

​                     if (j == 0)

​                            dp[j] = dp[j] + num[i][j];

​                     else

​                            dp[j] = min(dp[j],dp[j-1]) + num[i][j];    //注意2处

​              }

​       }

​       for (int i = 0; i < row; ++i)

​       {

​              res = min(res,dp[i]);

​       }

​       return res;

}

由于空间复杂度要求n，故用到滚动方法，内涵是;

在外部循环体的角度里，是将上一次外部循环的dp[j]和dp[j-1]更新给下一层的dp[j].

在内部循环体的角度里，由于dp方程要用到dp[j-1],故每层的更新是从尾到头。

总之，每一次外部循环会把上一层的dp传递给下一层(隐性表示i和i+1)，每层的更新要按照从尾到头（用到dp[j-1]，尾更新前头不能变）

 

同理：从下往上的方法自行练习.

int Triangle(vector<vector<int>>& num)

{

​       int res = INT_MAX;

​       int row = num.size();

​       vector<int> dp(row);

​       for (int i = 0; i < row; ++i)

​       {

​              dp[i] = num[row - 1][i];

​       }

​       for (int i = row-1; i >= 0; --i)

​       {

​              for (int j = 0; j <= i; ++j)

​              {

​                     dp[j] = min(dp[j],dp[j+1]) + num[i][j];

​              }

​       }

​       return dp[0];

}

要初始化,且只有dp[0会更新彻底,也就是最后的结果,省去了找min的过程.

 

 

 

 

 

1到n的数值组成BST树,有多少种组成方式:

DP方程:  dp[n ] = sum {dp[k]*dp[n-k-1]}

 

vector<int> dp(n + 1, 0);

dp[0] = 1;

dp[1] = 1;

for(int i = 2; i <= n; i++) 

{

for(int j = 0; j < i; j++) 

{

//如果左子树的个数为j，那么右子树为i - j - 1

dp[i] += dp[j] * dp[i - j - 1];

}

}

return dp[n];

首先左右子树之间肯定是乘法关系

***核心:  注意仔细深刻分析这个DP方程的动态迭代过程,在每一个内部循环中,会将j个各种情况产生的数量进行加和,并最终给到dp[i].

在外部循环中,实际上产生了新的i(从前往后),新的dp[i]初始值也是0,重复遍历dp[j]的过程.实际上,只是遍历j会累积(加和),新i和旧i并没有累积关系.

 

一个目标数,使用最少量的数,让这些数的平方和等于该数.

int numSquares(int n)

 {

vector<int> dp(n + 1, INT_MAX);

dp[0] = 0;

for (int i = 0; i <= n; i++) 

{

for (int j = 1; i + j * j <= n; j++) 

{

dp[i + j * j] = min(dp[i + j * j], dp[i] + 1);

}

}

return dp[n];

}

 

链表: 用快慢指针判断链表是否有环以及环的起点在哪.

以一个类似_O形状的链表举例, _的长度设为a,O的长度设为b,慢指针的步数设为x,则快指针的步数为2x.

首先,假使有环,快慢指针必定会相遇(一步两步,接近必定相遇,不存在跨过的情况).

其次,相遇的时候,慢指针还没有遍历完O部分一次(请用x和2x的数量关系去推证).

数学演算: 假使相遇的时候, x = a + c  (c< b),则a+c = b,也即b-c = a.一个指针从相遇的地方出发,另一个指针从头出发,那么它们便能在环的起点相遇.

 

链表的反转:

ListNode *reverseBetween(ListNode *head, int m, int n)

 {

if(!head) 

{ return head; }

ListNode dummy(0);

dummy.next = head;

ListNode* p = &dummy;

for(int i = 1; i < m; i++) 

{ p = p->next; }     //定位到m

//p此时就是pm的前驱节点

ListNode* pm = p->next;

for(int i = m; i < n; i++)           //核心部分

 {

ListNode* n = pm->next;

pm->next = n->next;

n->next = p->next;

p->next = n;

}

return dummy.next;

}

注意核心迭代程序,语句自行用链表画箭头图来理解,实际上每次迭代后,pm都会向后后走动.举例说明:迭代过程是,12345,21345,32145,43215,54321.(pm一直是1,但是它们的箭头指向变化了)

 

 

Sort Linked List:

 

ListNode *sortList(ListNode *head) 

{

if(head == NULL || head->next == NULL)

 { return head; }

ListNode* fast = head;

ListNode* slow = head;

//快慢指针得到中间点

while(fast->next && fast->next->next) 

{

fast = fast->next->next;

slow = slow->next;

}

//将链表拆成两半

fast = slow->next;

slow->next = NULL;

//左右两半分别排序

ListNode* p1 = sortList(head);

ListNode* p2 = sortList(fast);

//合并

return merge(p1, p2);

}

 

ListNode *merge(ListNode* l1, ListNode* l2) 

{

if(!l1) 

{ return l2; } 

else if (!l2) 

{ return l1; } 

else if (!l1 && !l2) 

{ return NULL; }

ListNode dummy(0);

ListNode* p = &dummy;

while(l1 && l2)

 {

if(l1->val < l2->val) 

{

p->next = l1;

l1 = l1->next;

} 

else

 {

p->next = l2;

l2 = l2->next;

}

p = p->next;

}

if(l1) 

{ p->next = l1; } 

else if(l2)

{ p->next = l2; }

return dummy.next;

}

注意前一个函数不断递归(不断二分),每层递归完后进行归并(类似于数组归并排序).

 

 

 背包问题：  代价和价值  求取最大利润

0-1背包问题

有N件物品和一个容量为V的背包。第i件物品的重量是w[i]，价值是v[i]。求解将哪些物品装入背包可使这些物品的重量总和不超过背包容量，且价值总和最大。

用子问题定义状态：即f[i][v]表示前i件物品恰放入一个容量为v的背包可以获得的最大价值。则其[状态转移方程](https://baike.baidu.com/item/状态转移方程)便是：

f[i][v]=max{ f[i-1][v], f[i-1][v-w[i]]+v[i] }。

这里就需要得到f[i-1][v]和 f[i-1][v-w[i]]问题的解，追溯下去，就是一个dp过程。

可以压缩空间，f[v]=max{f[v],f[v-w[i]]+v[i]}，因为i是一个从前往后的过程，不需要记录f的i。

 

完全背包问题：无穷

有N**种**物品和一个容量为V的背包，**每种无物品无限件**。第i件物品的重量是w[i]，价值是v[i]。求解将哪些物品装入背包可使这些物品的重量总和不超过背包容量，且价值总和最大。

[状态转移方程](https://baike.baidu.com/item/状态转移方程)：f[i][v]=max{ f[i-1][v-k*w[i]] + k*v[i] }。  0 <= k*v[i] < v

同理，也可以压缩空间。

此外，由于这里每种物品无限件，实际上也就可以筛选种类了，又重 && 又贵，直接pass。

 

多重背包问题：有限

有N**种**物品和一个容量为V的背包，**每种物品n[i]件**。第i件物品的重量是w[i]，价值是v[i]。求解将哪些物品装入背包可使这些物品的重量总和不超过背包容量，且价值总和最大。

[状态转移方程](https://baike.baidu.com/item/状态转移方程)：f[i][v]=max{ f[i-1][v-k*w[i]] + k*v[i] }。  k的两个限制条件略

 

实际上归结为k为0，1，i的情况，过程还是要和不要。

 

 

 

 

 