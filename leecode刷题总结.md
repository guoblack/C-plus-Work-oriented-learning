# 分类大纲

1. 算法的复杂度分析。
2. 排序算法，以及他们的区别和优化。
3. 数组中的双指针、滑动窗口思想。
4. 利用 Map 和 Set 处理查找表问题。
5. 链表的各种问题。
6. 利用递归和迭代法解决二叉树问题。
7. 栈、队列、DFS、BFS。
8. 回溯法、贪心算法、动态规划。

## 😮查找算法

### 二分查找+插值查找

> 二分查找计算公式：
> $$
> mid = \frac{1}{2}(high+low)=low+\frac{1}{2}(high-low)
> $$
> 插值查找计算公式：
> $$
> mid = low+\frac{key-a[low]}{a[high]-a[low]}(high-low)
> $$

```c++
int Binary_Search(vector<int>&num,int n,int key)
{
    int low(0),high(n),mid(0);
    while(low<=high)
    {
        mid = (high+low)/2;	//二分查找
       // mid = low+((key-a[low])/(a[high]-a[low]))*(high-low);	//插值查找
        if(key<a[mid])
        {
            high = mid - 1;
        }
        else if(key>a[mid])
        {
            low = mid + 1;
        }
      	else
            return mid;
    }
    return 0;
}
```

### 斐波那契查找





---

## 🙄排序算法

> 必做：[215]([215. 数组中的第K个最大元素 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/))、

选择排序

```c++
void SelectSort(vector<int>&arr)
{
    for(int i=0;i<arr.size()-1;i++)
    {
        int i = minpos;
        for(int j=i+1;j<arr.size()-1;j++)
        {
            if(arr[j]<arr[minpos])
            {                                                                                                                           
              minpos = j;
            }
        }
        swap(arr,minpos,j);
    }
}
void swap(vector<int>&arr,int i,int j)
{
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] =temp;
}
```

## 😪一切皆可搜索

> **最常见图的遍历方式：**
>
> DFS-深度优先搜索：（结果不唯一）
>
> ![image-20210507105936785](https://typora-1300187609.cos.ap-chongqing.myqcloud.com/img/image-20210507105936785.png)
>
> 
>
> BFS-广度优先搜索：（结果不唯一）
>
> ![image-20210428205755750](https://typora-1300187609.cos.ap-chongqing.myqcloud.com/img/image-20210428205755750.png)
>
> 特点：每当访问一个结点，会把与这个结点相连的所有结点访问一遍

### 695.[岛屿的面积](https://leetcode-cn.com/problems/max-area-of-island/)

> 给定一个二维的 0-1 矩阵，其中 0 表示海洋， 1 表示陆地。单独的或相邻的陆地可以形成岛
> 屿，每个格子只与其上下左右四个格子相邻。求最大的岛屿面积  

- 找出所有岛屿面积，求出最大值
- **找出某一岛屿面积思路：**（图的遍历过程）
  - 从某位置出发，向四个方向探寻相连的土地	
  - 每探寻到一块土地，计数加一
  - 探寻过的土地置0，防止重复

![image-20210411144150840](https://typora-1300187609.cos.ap-chongqing.myqcloud.com/img/image-20210411144150840.png)

```c++
class Solution {
public:
    int IslandDFS(vector<vector<int>>&gird,int i,int j)
    {
        //base case
        if(i<gird.size()&&(i>=0)&&(j<gird[0].size())&&j>=0)	
        {
            if(gird[i][j]==0)		//如果遍历到的是0，返回
            {
                return 0;
            }
            else				  //如果遍历到的是1
                gird[i][j]=0;	   //将1置0，防止重复遍历   是否可以修改原始数组？？？
            return 1+IslandDFS(gird,i,j-1)+IslandDFS(gird,i-1,j)+IslandDFS(gird,i,j+1)+IslandDFS(gird,i+1,j); //返回该块陆地遍历一圈后的值
        }
        else					//越界
            return 0;
    }
    int maxAreaOfIsland(vector<vector<int>>&gird)
    {
        int ans(0);
        for(int i=0;i<gird.size();i++)
        {
            for(int j=0;j<gird[0].size();j++)
            {
                ans = max(ans,IslandDFS(gird,i,j));
            }
        }
        return ans;
    }
};
//空间复杂度：在递归时，最大情况下递归深度为整个网格大小(全1),故空间复杂度为O(R*C),其中R与C分别为输入的行数、列数。
//时间复杂度：在DFS过程中，网格每个节点至多访问一次，时间复杂度为O(R*C).R、C含义同上
```

### 463.[岛屿的周长](https://leetcode-cn.com/problems/island-perimeter/)

![image-20210428144520625](https://typora-1300187609.cos.ap-chongqing.myqcloud.com/img/image-20210428144520625.png)

```c++
//思路：找到陆地及公共边的个数，周长 = 4*陆地个数 - 2*公共边个数
class Solution
{
    int islandPerimeter(vector<vector<int>>&gird)
    {
        if(grid.empty()||grid[0].empty())	return 0;
        int land_size(0),same_side(0);
        for(int i=0;i<grid.size();i++)
        {
            for(int j=0;j<grid[0].size();j++)
            {
                if(grid[i][j]==0)	continue;
                land_size++;
                if(gird[i-1][i+1]==1)	same_size++;
                if(gird[i][j-1]==1)		same_size++;
            }
        }
        return 4*land_size - 2*same_size;
    }

};
```

### 200.[岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

> 给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。此外，你可以假设该网格的四条边均被水包围。
>
> 示例 1：
>
> 输入：grid = [
> ["1","1","1","1","0"],
> ["1","1","0","1","0"],
> ["1","1","0","0","0"],
> ["0","0","0","0","0"]
> ]
> 输出：1

```c++
//深度优先搜索，将访问过的岛屿置 ‘0’
class Solution
{
    public:
    int numIslands(vector<vector<char>>& grid) 
    {
        int land_size(0);
        int m=grid.size(),n=grid[0].size();
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(grid[i][j]=='0')    continue;
                ++land_size;
                IslandDFS(grid,i,j);
            }
        }
        return land_size;
    }
    void IslandDFS(vector<vector<char>>& grid,int i,int j)
    {
        if(i<0||i>=grid.size()||j<0||j>=grid[0].size()||grid[i][j]=='0')    return ;//越界
        grid[i][j] = '0';
        IslandDFS(grid,i-1,j);
        IslandDFS(grid,i+1,j);
        IslandDFS(grid,i,j-1);
        IslandDFS(grid,i,j+1);
    }
};
//深度优先搜索，用一个数组来存放遍历的结果
class Solution
{
    public:
    void IslandDFS(vector<vector<char>>& grid,vector<vector<bool>> &vis,int i,int j)
    {
        if(i<0||i>=grib.size()||j<0||j>=grib[0].size()||grib[i][j]=='0'||vis[i][j]=true)	return ;
        vis[i][j] = true;
        IslandDFS(grid,vis,i-1,j);
        IslandDFS(grid,vis,i+1,j);
        IslandDFS(grid,vis,i,j-1);
        IslandDFS(grid,vis,i,j+1);
    }
    int numIslands(vector<vector<char>>& grid) 
    {
        int m = grid.size(), n = grid[0].size();
        vector<vector<bool>> vis = (m,vector<bool>(n));
        int land_size(0);
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(grid[i][j]=='0'||vis[i][j])	continue;	
                land_size++；
                IslandDFS(grid,vis,i,j);
            }
        }
        return land_size;
    }
};
```

### 236. [二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

> 定义：设节点root为节点p，q的某公共祖先，且root的左右子节点root.left、root.right均不是p,q的公共节点，则root是p,q的最近公共节点。

若root是p，q的最近公共节点，有且仅有下列情况：

- p，q分列root左右子树中
- p=root，且q位于root的左右子树中
- q=root，且p位于root的左右子树中

[题解](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/solution/236-er-cha-shu-de-zui-jin-gong-gong-zu-xian-hou-xu/)

> 递归解法：
>
> **1.终止条件**
>
> - 当越过叶节点，则直接返回 null ；
> - 当 root 等于 p,*q* ，则直接返回 root；
>
> **2.递归工作**
>
>  - 开启递归左子节点，返回值记为**left**；
>  - 开启递归右子节点，返回值记为**right**；
>
> **3.返回值**
>
> - 当**left** 和 **right**同时为空：说明root的左/右子树中都不包含p,q,返回null;               if(!left&&!right)  return null;
> - 当**left **和 **right** 同时不为空：说明p,q分列在root的异侧(分别在左/右子树)，因此root为最近公共祖先，返回root;      if(left&&right) return root;
> - 当**left** 为空，**right **不为空：p,q都不在root的左子树中，直接返回 right。具体可分为两种情
>   况
>   - p,q其中一个在root的右子树中，此时 **right** 指向p(假设为p)
>   - p,q两节点都在root的右子树中，此时的 **right** 指向最近公共祖先节点
> - 当**left**不为空， **right**为空：与情況3.同理



![image-20210429100806148](https://typora-1300187609.cos.ap-chongqing.myqcloud.com/img/image-20210429100806148.png)

 

![image-20210429101944687](https://typora-1300187609.cos.ap-chongqing.myqcloud.com/img/image-20210429101944687.png)

 ![image-20210429102443899](https://typora-1300187609.cos.ap-chongqing.myqcloud.com/img/image-20210429102443899.png)

![image-20210429102533138](https://typora-1300187609.cos.ap-chongqing.myqcloud.com/img/image-20210429102533138.png)

![image-20210429102550813](https://typora-1300187609.cos.ap-chongqing.myqcloud.com/img/image-20210429102550813.png)

## 😫数据结构系列

### 146.LRU[实现机制](https://leetcode-cn.com/problems/lru-cache/solution/lruhuan-cun-ji-zhi-by-leetcode-solution/)📺

<video src="https://typora-1300187609.cos.ap-chongqing.myqcloud.com/video/146LRU%E5%AE%9E%E7%8E%B0%E6%9C%BA%E5%88%B6.mp4"></video>

> 哈希表 + 双向链表
>
> LRU 缓存机制可以通过**哈希表**辅以双向链表实现，我们用一个哈希表和一个双向链表维护所有在缓存中的**键值对**。
>
> 双向链表按照被使用的顺序存储了这些键值对，靠近头部的键值对是最近使用的，而靠近尾部的键值对是最久未使用的。
>
> 哈希表即为普通的哈希映射（HashMap），通过缓存数据的键映射到其在双向链表中的位置。
>
> 这样以来，我们首先使用哈希表进行定位，找出缓存项在双向链表中的位置，随后将其移动到双向链表的头部，即可在 O(1)O(1) 的时间内完成 get 或者 put 操作。具体的方法如下：
>
> - **对于 get 操作，首先判断 key 是否存在：**
>  - **如果 key 不存在，则返回 −1；**
>
>  - **如果 key 存在，则 key 对应的节点是最近被使用的节点。通过哈希表定位到该节点在双向链表中的位置，并将其移动到双向链表的头部，最后返回该节点的值。**
>
> - **对于 put 操作，首先判断 key 是否存在：**
>
>  - **如果 key 不存在，使用 key 和 value 创建一个新的节点，在双向链表的头部添加该节点，并将 key 和该节点添加进哈希表中。然后判断双向链表的节点数是否超出容量，如果超出容量，则删除双向链表的尾部节点，并删除哈希表中对应的项；**
>
>  - **如果 key 存在，则与 get 操作类似，先通过哈希表定位，再将对应的节点的值更新为 value，并将该节点移到双向链表的头部。**
>
> 上述各项操作中，访问哈希表的时间复杂度为 O(1)O(1)，在双向链表的头部添加节点、在双向链表的尾部删除节点的复杂度也为 O(1)O(1)。而将一个节点移到双向链表的头部，可以分成「删除该节点」和「在双向链表的头部添加节点」两步操作，都可以在 O(1)O(1) 时间内完成。
>
> 小贴士
>
> 在双向链表的实现中，使用一个**伪头部（dummy head）**和**伪尾部（dummy tail）**	标记界限，这样在添加节点和删除节点的时候**就不需要检查相邻的节点是否存在**。
>
> ![image-20210429215331345](https://typora-1300187609.cos.ap-chongqing.myqcloud.com/img/image-20210429215331345.png)

```c++
//双向链表结构体
struct DLinkedNode {
    int key, value;
    DLinkedNode* prev;
    DLinkedNode* next;
    DLinkedNode(): key(0), value(0), prev(nullptr), next(nullptr) {}
    DLinkedNode(int _key, int _value): key(_key), value(_value), prev(nullptr), next(nullptr) {}
};

class LRUCache {
private:
    unordered_map<int, DLinkedNode*> cache;
    DLinkedNode* head;
    DLinkedNode* tail;
    int size;
    int capacity;

public:
    LRUCache(int _capacity): capacity(_capacity), size(0) {
        // 使用伪头部和伪尾部节点
        head = new DLinkedNode();
        tail = new DLinkedNode();
        head->next = tail;
        tail->prev = head;
    }
    //读
    int get(int key) {
        if (!cache.count(key)) {
            return -1;
        }
        // 如果 key 存在，先通过哈希表定位，再移到头部
        DLinkedNode* node = cache[key];			//找到node的位置
        moveToHead(node);
        return node->value;					   //返回val值
    }
   //存 
    void put(int key, int value) {
        if (!cache.count(key)) {
            // 如果 key 不存在，创建一个新的节点
            DLinkedNode* node = new DLinkedNode(key, value);
            // 添加进哈希表
            cache[key] = node;
            // 添加至双向链表的头部
            addToHead(node);
            ++size;
            if (size > capacity) {
                // 如果超出容量，删除双向链表的尾部节点
                DLinkedNode* removed = removeTail();
                // 删除哈希表中对应的项
                cache.erase(removed->key);
                // 防止内存泄漏
                delete removed;
                --size;
            }
        }
        else {
            // 如果 key 存在，先通过哈希表定位，再修改 value，并移到头部
            DLinkedNode* node = cache[key];
            node->value = value;
            moveToHead(node);
        }
    }

    void addToHead(DLinkedNode* node) {
        node->prev = head;
        node->next = head->next;
        head->next->prev = node;
        head->next = node;
    }
    
    void removeNode(DLinkedNode* node) {
        node->prev->next = node->next;
        node->next->prev = node->prev;
    }

    void moveToHead(DLinkedNode* node) {
        removeNode(node);
        addToHead(node);
    }

    DLinkedNode* removeTail() {
        DLinkedNode* node = tail->prev;
        removeNode(node);
        return node;
    }
};

```

> void addToHead(DLinkedNode* node)![image-20210429215014493](https://typora-1300187609.cos.ap-chongqing.myqcloud.com/img/image-20210429215014493.png)

## 😨双指针

### 26.[删除有序数组中重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)📺

> 给你一个有序数组 nums ，请你 **原地** 删除重复出现的元素，使每个元素 只出现一次 ，返回删除后数组的新长度。
>
> 不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

<video src="https://typora-1300187609.cos.ap-chongqing.myqcloud.com/video/26%E5%88%A0%E9%99%A4%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E4%B8%AD%E7%9A%84%E9%87%8D%E5%A4%8D%E9%A1%B9.mp4"></video>

```C++
//法一：双指针	时间复杂度 O（n）
class Solution {
public:
    int removeDuplicates(vector<int>& nums) 
    {
        //空数组
        if(nums.empty()) return 0;
        int slow(1),fast(1);
        for(;fast<nums.size();fast++)
        {
            if(nums[fast-1]!=nums[fast])
            {
                nums[slow] = nums[fast];
                slow++;
            }
        }
        return slow;
    }        
};
//法二：STL	
class Solution {
public:
    int removeDuplicates(vector<int>& nums) 
    {
        return distance(nums.begin(),unique(nums.begin,num.end()); //返回从begin到end范围内除去重复数字的距离
    }        
};
```

### 80.[删除有序数组中的重复项Ⅱ](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/submissions/)

> 给你一个有序数组 nums ，请你 **原地** 删除重复出现的元素，使每个元素 **最多出现两次** ，返回删除后数组的新长度。
>
> 不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。
>
> ```c++
> 输入：nums = [1,1,1,2,2,3]
> 输出：5, nums = [1,1,2,2,3]
> ```

```c++
//定义两个指针slow，fast。slow表示处理后的数组长度，fast表示已经检查过的数组长度。nums[slow-1]为上一个应该被保留的元素class Solution {public:    int removeDuplicates(vector<int>& nums)     {       	int slow(2),fast(2);        if(nums.size()<=2)	return nums.size();	//如果数组长度小于等于2，直接返回        for(;fast<nums.size();fast++)        {            if(nums[slow-2]!=nums[fast])		//如果最大重复次数为n，则 nums[slow-n]!=nums[fast] ;对应的int slow(n),fast(n);            {                nums[slow] = nums[fast];                slow++;                //等价于 nums[slow++] = nums[fast];            }        }        return slow;    }};
```



### 27.[移除元素](https://leetcode-cn.com/problems/remove-element/)📺

> 给你一个数组 nums 和一个值 val，你需要 **原地** 移除所有数值等于 val 的元素，并返回移除后数组的新长度。
>
> 不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 **原地** 修改输入数组。
>
> 元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。
>
> ```c++
> //示例：输入：nums = [3,2,2,3], val = 3输出：2, nums = [2,2]
> ```

![27.移除元素-暴力解法](https://typora-1300187609.cos.ap-chongqing.myqcloud.com/img/68747470733a2f2f747661312e73696e61696d672e636e2f6c617267652f30303865476d5a456c7931676e747263377839746a673330647530396d316b792e676966)

​																													<!--暴力解法-->

![27.移除元素-双指针法](https://camo.githubusercontent.com/a701d49e9f64bd71f08a19276679f49d971d765e07402b3982f9b0ab63969f45/68747470733a2f2f747661312e73696e61696d672e636e2f6c617267652f30303865476d5a456c7931676e7472647336723539673330647530396d6e70642e676966)

​																											<!--双指针法-->

```C++
//法一：暴力解法		时间复杂度O（n²）class Solution {public:    int removeElement(vector<int>& nums, int val)     {        int size = nums.size();        if(nums.empty())    return 0;        for(int i=0;i<size;i++)        {            if(nums[i]==val)            {                for(int j=i+1;j<size;j++)                {                    nums[j-1]=nums[j];                }                i--;//因为下标i以后的数值都向前移动了一位，所以i也向前移动一位                size--;            }        }        return size;    }};//法二：双指针		时间复杂度O（n）class Solution {public:    int removeElement(vector<int>& nums, int val)     {        if(nums.empty())    return 0;        int slow(0),fast(0);        //双指针：一个for循环实现两个for循环的功能        for(;fast<nums.size();fast++)         {            if(nums[fast]!=val)            {                nums[slow++] = nums[fast];            }        }        return slow;    }};
```

### 142.[环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

> 给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。
>
> 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意，pos 仅仅是用于标识环的情况，并不会作为参数传递到函数中。
>
> 说明：不允许修改给定的链表。
>
> **进阶：**
>
> - 你是否可以使用 `O(1)` 空间解决此题？

```c++
//快指针每次走2，慢指针每次走1，快指针走的距离是慢指针的两倍。而快指针又比慢指针多走了一圈。所以 head 到环的起点+环的起点到他们相遇的点的距离 与 环一圈的距离相等。现在重新开始，head 运行到环起点 和 相遇点到环起点 的距离也是相等的，相当于他们同时减掉了 环的起点到他们相遇的点的距离class Solution {public:    ListNode *detectCycle(ListNode *head)    {    	ListNode *slow = head,*fast = head;        if(!fast||!fast->next)	return null;        do        {            fast = fast->next->next;            slow = slow->next;        }while(fast!=slow);        fast = head;        while(fast!=slow)        {            fast = fast->next;            slow = slow->next;        }        return fast;    }};
```

### [240. 搜索二维矩阵 II](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/)📺

> 编写一个高效的算法来搜索 m x n 矩阵 matrix 中的一个目标值 target 。该矩阵具有以下特性：
>
> 每行的元素从左到右升序排列。
> 每列的元素从上到下升序排列。
>
> ![image-20210506203415135](https://typora-1300187609.cos.ap-chongqing.myqcloud.com/img/image-20210506203415135.png)
>
> ```c++
> 输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5输出：true
> ```

> 如果 **左下角元素** 大于了目标值，则目标值一定在该行的上方， **左下角元素** 所在行可以消去。
>
> 如果 **左下角元素** 小于了目标值，则目标值一定在该列的右方， **左下角元素** 所在列可以消去。
>
> 具体操作为从矩阵左下角元素开始遍历，并与目标值对比：
>
> - 当 `matrix[i][j] > target` 时： 行索引向上移动一格（即 `i--`），即消去矩阵第 `i` 行元素；
> - 当 `matrix[i][j] < target` 时： 列索引向右移动一格（即 `j++`），即消去矩阵第 `j` 列元素；
> - 当 `matrix[i][j] == target` 时： 返回 true。
> - 如果越界，则返回 false。

<video src="https://typora-1300187609.cos.ap-chongqing.myqcloud.com/video/240%E6%90%9C%E7%B4%A2%E4%BA%8C%E7%BB%B4%E7%9F%A9%E9%98%B5.mp4"></video>

```C++
class Solution {public:    bool searchMatrix(vector<vector<int>>& matrix, int target)     {        int  i = matrix.size()-1,j=0;   //matrix[i][j]对应最左下角元素        if(matrix.empty()||matrix[0].empty())   return false;        while(i>=0&&j<matrix[0].size())        {            if(matrix[i][j]>target)                i--;//行索引向上移动一格（即 i-- ），即消去矩阵第 i 行元素            else if(matrix[i][j]<target)                j++;//列索引向右移动一格（即 j++ ），即消去矩阵第 j 列元素            else if(matrix[i][j]==target)//找到目标值，直接返回 ture                return true;        }        return false;    }};
```

