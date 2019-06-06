### C++源码：

 ```C++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

void BubbleSort(vector<int>& nums)     //最小的冒到最左
{
	for (int i = 0; i<nums.size(); i++)
	{
		bool flag = false;
		for (int j = nums.size() - 1; j>i; j--)
		{
			if (nums[j] < nums[j - 1])
			{
				int temp = nums[j];
				nums[j] = nums[j - 1];
				nums[j - 1] = temp;
				flag = true;
			}
		}
		if (!flag) break;     // 某次循环没有交换冒泡，则已有序，退出  
	}
}

void InsertSort(vector<int>& nums)   //比较站在他前面的
{
	int i, j, temp;
	for (i = 1; i < nums.size(); i++)
	{
		temp = nums[i];
		for(j = i; j>0 && temp<nums[j - 1]; j--)
			nums[j] = nums[j - 1];
		nums[j] = temp;
	}
}

void SelectSort(vector<int>& nums)      //选择最小的
{
	for (int i = 0; i<nums.size() - 1; i++)
	{
		int index = i;
		for (int j = i + 1; j<nums.size(); ++j)
		{
			if (nums[j]<nums[index])
				index = j;
		}
		if (index != i)
		{
			int temp = nums[i];
			nums[i] = nums[index];
			nums[index] = temp;
		}
	}
}

void QuickSort(vector<int>& nums, int left, int right)
{
	if (left < right)
	{
		int mid = nums[(left + right) / 2];
		int i = left - 1, j = right + 1;
		while (1)
		{
			while (nums[++i]<mid);
			while (nums[--j]>mid);
			if (i >= j)break;
			int temp = nums[i];
			nums[i] = nums[j];
			nums[j] = temp;
		}
		QuickSort(nums, left, i - 1);
		QuickSort(nums, j + 1, right);
	}
}
void ShellSort(vector<int>& nums)
{
	for (int gap = nums.size() / 2; gap > 0; gap /= 2)
	{
		for (int i = gap; i<nums.size(); i++)
		{
			int temp = nums[i];
			int j;
			for (j = i; j >= gap && temp <nums[j - gap]; j -= gap)
				nums[j] = nums[j - gap];
			nums[j] = temp;
		}
	}
}
6 堆排 (大顶堆，升序)     
这里的堆是一种数据结构，满足大顶或小顶的完全二叉树.小顶堆：nums[i] <= nums[2i+1] && nums[i] <= nums[2i+2]。注意i父，2i+1左子，2i+2右子。 
堆排序的基本思想是：先将待排序序列构造成一个小顶堆，此时能确定整个序列的最小值就是堆顶的根节点。将其与末尾元素进行交换，此时末尾就为最小值。然后将剩余n-1个元素重新构造成一个堆，这样会得到n个元素的次小值。如此反复执行。
void Adjust(vector<int>& nums, int size, int index)   //logn，递归流程是自上而下
{
	int left = 2 * index + 1;
	int right = 2 * index + 2;
	int max = index;
	if (left<size && nums[left] > nums[max])   //在父和左右找目标
		max = left;
	if (right<size && nums[right] > nums[max])
		max = right;
	if (max != index)
	{
		swap(nums[max], nums[index]);
		Adjust(nums, size, max);
	}
}

void HeapSort(vector<int>& nums, int size)
{
	for (int i = size / 2 - 1; i >= 0; i--)    //初步预建一个大顶堆，预构很重要
	{							
		Adjust(nums, size, i);           //注意预购的循环顺序
	}
	for (int i = size - 1; i >= 1; i--)     //On
	{
		swap(nums[0], nums[i]);   //将大顶堆顶点和数组记录的末尾交换
		Adjust(nums, i, 0);       //重构大顶堆
	}
}


//拆，排，并
void Merge(vector<int>& nums, int left, int mid, int right, vector<int>& temp)
{
	int i = left;
	int j = mid + 1;
	int t = 0;   //临时数组指针
	while (i <= mid && j <= right)
	{
		if (nums[i] <= nums[j])
			temp[t++] = nums[i++];
		else
			temp[t++] = nums[j++];
	}
	while (i <= mid)
		temp[t++] = nums[i++];
	while (j <= right)
		temp[t++] = nums[j++];
	t = 0;   //将temp中的元素全部拷贝到原数组中
	while (left <= right)
		nums[left++] = temp[t++];
}

void MergeSort(vector<int>& nums, int left, int right, vector<int>& temp)
{
	if (left < right)
	{
		int mid = (left + right) / 2;
		MergeSort(nums, left, mid, temp); 	//左
		MergeSort(nums, mid + 1, right, temp);	//右
		Merge(nums,left,mid,right,temp);
	}
}
mergeSort里面先划分左右子区域（递归是下一层），merge对已经有序的左右子区域进行整合排序，并将临时空间复制到原数组相应范围里。注意，最里层只有left(mid=left)，right(right = left+1)两个元素，即对这两个元素整合排序并复制，在外层对一个个有序的左右子区域进行整合排序。Temp是一个同大小的临时容器。

int BinarySearch(vector<int>& nums, int target)
{
	int left= 0;
	int right= nums.size();
	while(left <= right)
	{
		int mid = (left + right) / 2;  //可能溢出                   
		if(nums[mid] > target)
			right = mid - 1;
		else if(nums[mid] < target)
			left = mid + 1;
		else
			return mid;
	}
	return 0;
}
int main()
{
	vector<int> nums = {9,8,7,6,5,4,3,2,1};
	BubbleSort(nums);
	for (int i = 0; i < nums.size(); ++i)
	{
		cout << nums[i] << endl;
	}
	system("pause");
	return 0;
}        //计数，基数，桶排，略。。。
 ```

### 特性对比：

| 排序方法 | 时间复杂度 | 空间复杂度 | 稳定性     |               |        |
| -------- | ---------- | ---------- | ---------- | ------------- | ------ |
| 最好     | 平均       | 最坏       | 辅助存储   |               |        |
| 直接插入 | O(N)       | O(N2)      | O(N2)      | O(1)          | 稳定   |
| 希尔排序 | O(N)       | O(N1.3)    | O(N2)      | O(1)          | 不稳定 |
| 直接选择 | O(N)       | O(N2)      | O(N2)      | O(1)          | 不稳定 |
| 堆排序   | O(N*log2N) | O(N*log2N) | O(N*log2N) | O(1)          | 不稳定 |
| 冒泡排序 | O(N)       | O(N2)      | O(N2)      | O(1)          | 稳定   |
| 快速排序 | O(N*log2N) | O(N*log2N) | O(N2)      | O(log2n)~O(n) | 不稳定 |
| 归并排序 | O(N*log2N) | O(N*log2N) | O(N*log2N) | O(n)          | 稳定   |

 

