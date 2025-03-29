- [常用函数](#常用函数)
- [排序算法](#排序算法)
	- [用来测试的cpp文件](#用来测试的cpp文件)
	- [冒泡排序](#冒泡排序)
	- [选择排序](#选择排序)
	- [插入排序](#插入排序)
	- [二分插入排序](#二分插入排序)
	- [希尔排序](#希尔排序)
	- [归并排序（递归版）](#归并排序（递归版）)
	- [归并排序（迭代版）](#归并排序（迭代版）)
	- [快速排序（递归版）](#快速排序（递归版）)
	- [快速排序（迭代版）](#快速排序（迭代版）)
	- [堆排序](#堆排序)
	- [基数排序](#基数排序)
	- [桶排序](#桶排序)
	- [计数排序](#计数排序)
- [二分查找](#二分查找)
- [处理边界条件](#处理边界条件)
	- [螺旋矩阵II](#螺旋矩阵II)
	- [设计链表](#设计链表)
	- [反转字符串中的单词](#反转字符串中的单词)
- [双指针](#双指针)
- [字符串](#字符串)
	- [重复的子字符串](#重复的子字符串)
- [滑动窗口](#滑动窗口)
	- [长度最小的子数组](#长度最小的子数组)
	- [滑动窗口最大值](#滑动窗口最大值)
- [哈希表](#哈希表)
	- [三数之和](#三数之和)
	- [四数之和](#四数之和)
	- [四数相加II](#四数相加II)
- [常考算法题](#常考算法题)
	- [最长回文子串](#最长回文子串)
	- [最长公共子串](#最长公共子串)
	- [找出字符串中第一个匹配项的下标（KMP算法）](#找出字符串中第一个匹配项的下标（KMP算法）)
	- [环形链表II](#环形链表II)
	- [最大连续bit数](#最大连续bit数)

# 常用函数
```
vector数组
	push_back()，pop_back()
	size()，empty()，clear()
	begin()，end()，rbegin()，rend()
	front()，back()
	insert()，emplace()，erase()
string字符串
	getline(cin, s)
	substr(start, length)，find(str)，replace(start, count, str)
	replace(const_iterator first, const_iterator last, const basic_string& str)
	stoi(str)，stol(str)，to_string(value)
	push_back()，pop_back()，append()，copy()
	size()，empty()，clear()
	begin()，end()，rbegin()，rend()
	find()，compare()，contains()
stack
	push()，pop()，top()，empty()，size()
queue
	push()，pop()，front()，back()，empty()，size()
priority_queue优先队列（堆）
	top()，empty()，size()，push()，pop()
	默认大根堆，小根堆：priority_queue<int, vector<int>, greater<>>
deque双端队列
	push_front()，push_back()，pop_front()，pop_back()
	front()，back()
	size()，empty()，clear()
	begin()，end()，rbegin()，rend()
set/map（有序）
	insert()，erase()，clear()，find(key)，count(key)，lower_bound()，upper_bound()
	empty()，size()
underered_set/unordered_map（哈希表）
	insert()，erase()，clear()，find(key)，count(key)
	empty()，size()
pair/tuple
	pair<int, int> pa;
	pair.first，pair.second



#include <algorithm>
reverse(v.begin(), v.end())
排序和查找
	sort(v.begin(), v.end());
	sort(v.begin(), v.end(), greater<>());
	auto it = lower_bound(v.begin(), v.end(), x);
	auto it = upper_bound(v.begin(), v.end(), x);
	binary_search(v.begin(), v.end(), x);
最值和交换
	max(a, b); min(a, b); swap(a, b); minmax(a, b);
	auto max_val = *max_element(v.begin(), v.end());
数组/容器操作
	reverse(v.begin(), v.end()); // 反转
	auto last = unique(v.begin(), v.end()); // 去重相邻重复
	fill(v.begin(), v.end(), 0); // 填充值
	rotate(v.begin(), v.begin()+k, v.end()); // 旋转
排列组合
	next_permutation(v.begin(), v.end()); // 下一个排列
	prev_permutation(v.begin(), v.end());



# include <numeric>
数值运算
	int sum = accumulate(v.begin(), v.end(), 0);
	reduce(v.begin(), v.end());（c++17）
	midpoint(a, b);


# include <ctype.h>
isdigit(c)，isalpha(c)，islower(c)，isupper(c)



# include <utility>
std::move，std::forward，std::swap，std::pair，std::make_pair



# include <functional>
std::bind
std::function



# include <tuple>
std::tuple，std::make_tuple



# include <memory>
std::shared_ptr，std::unique_ptr，std::weak_ptr
std::make_shared，std::make_unique



# include <any>（c++17）
std::any
```
# 排序算法
## 用来测试的cpp文件
```cpp
#include <iostream>
#include <cstdlib>
#include <cstdio>
#include "bucket_sort.h"

int main() {
    for (int i = 0; i != 10; ++i) {
        int arr[20];
        for (int j = 0; j != 20; ++j) {
            arr[j] = rand() % 80 + 10;
        }
        bucket_sort(arr, 0, 19);
        for (int j = 0; j != 20; ++j) {
//             cout << arr[j] << " ";
            printf("%3d", arr[j]);
        }
        printf("\n");
    }
}
```
## 冒泡排序
```cpp
#ifndef _BUBBLE_SORT_H
#define _BUBBLE_SORT_H

#include <algorithm>

void bubble_sort(int arr[], int low, int high) {
    for (int i = 0; i != high - low; ++i) {
        for (int j = low; j < high; ++j) {
            if (arr[j] > arr[j + 1]) {
                std::swap(arr[j], arr[j + 1]);
            }
        }
    }
}

#endif
```
## 选择排序
```cpp
#ifndef _SELECT_SORT_H
#define _SELECT_SORT_H

#include <climits>
#include <algorithm>

void select_sort(int arr[], int low, int high) {
    int min_num = INT_MAX, min_pos = low;
    for (int i = low; i < high; ++i) {
        for (int j = i; j <= high; ++j) {
            if (arr[j] < min_num) {
                min_num = arr[j];
                min_pos = j;
            }
        }
        std::swap(arr[i], arr[min_pos]);
        min_num = INT_MAX;
    }
}

#endif
```
## 插入排序
```cpp
#ifndef _INSERT_SORT_H
#define _INSERT_SORT_H

int nums;

void insert_sort(int arr[], int low, int high) {
    for (int i = low + 1; i <= high; ++i) {
        int j = i - 1;
        while (j >= low) {
            if (arr[i] >= arr[j]) break;
            --j;
        }
        int temp = arr[i];
        for (int m = i; m > j + 1; --m) {
            arr[m] = arr[m - 1];
        }
        arr[j + 1] = temp;
    }
}

#endif
```
## 二分插入排序
```cpp
#ifndef _BINARY_INSERT_SORT_H
#define _BINARY_INSERT_SORT_H

void binary_insert_sort(int arr[], int low, int high) {
    for (int i = low + 1; i <= high; ++i) {
        int left = low, right = i - 1, temp = arr[i], pos;
        while (left <= right) {
            if (left == right) {
                if (temp >= arr[left]) {
                    pos = left + 1;
                    break;
                } else {
                    pos = left;
                    break;
                }
            }
            if (left - right == 1) {
                pos = right;
                break;
            }

            int mid = (left + right) / 2;
            if (arr[mid] <= temp) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        
        for (int j = i; j > pos; --j) {
            arr[j] = arr[j - 1];
        }
        arr[pos] = temp;
    }
}

#endif
```
## 希尔排序
```cpp
#ifndef _SHELL_SORT_H
#define _SHELL_SORT_H

#include <algorithm>

int nums = 0;

void shell_sort(int arr[], int low, int high) {
    for (int step_len = (high-low+1)/2; step_len > 0; step_len >>= 1) {
        for (int i = 0; i < step_len; ++i) {
            for (int j = low + i; j <= high; j += step_len) {
                int temp = j;
                for (int m = j - step_len; m >= low + i; m -= step_len) {
                    if (arr[j] < arr[m]) {
                        std::swap(arr[j], arr[m]);
                        j = m;
                    } else break;
                }
                j = temp;
            }
        }
    }
}

#endif
```
## 归并排序（递归版）
```cpp
#ifndef _MERGE_SORT_H
#define _MERGE_SORT_H

void merge_sort(int arr[], int low, int high) {
    if (low >= high) {
        return;
    }
    int mid = (low + high) / 2;
    merge_sort(arr, low, mid);
    merge_sort(arr, mid + 1, high);

    int brr[high - low + 1];
    int i = low, j = mid + 1, brr_ptr = 0;
    while (i <= mid && j <= high) {
        if (arr[i] < arr[j]) {brr[brr_ptr++] = arr[i++];}
        else {brr[brr_ptr++] = arr[j++];}
    }

    while (i <= mid) {
        brr[brr_ptr++] = arr[i++];
    }
    while (j <= high) {
        brr[brr_ptr++] = arr[j++];
    }
    for (i = low, brr_ptr = 0; i <= high;) {
        arr[i++] = brr[brr_ptr++];
    }
    return;
}

#endif
```
## 归并排序（迭代版）
```cpp
#ifndef _MERGE_SORT_ITERATOR_H
#define _MEGER_SORT_ITERATOR_H

#include <algorithm>

void merge(int arr[], int low, int mid, int high) {
    int brr[high - low + 1];
    int i = low, j = mid + 1, brr_ptr = 0;
    while (i <= mid && j <= high) {
        if (arr[i] < arr[j]) {brr[brr_ptr++] = arr[i++];}
        else {brr[brr_ptr++] = arr[j++];}
    }

    while (i <= mid) {
        brr[brr_ptr++] = arr[i++];
    }
    while (j <= high) {
        brr[brr_ptr++] = arr[j++];
    }
    for (i = low, brr_ptr = 0; i <= high;) {
        arr[i++] = brr[brr_ptr++];
    }
}

void merge_sort_iterator(int arr[], int low, int high) {
    int len = high - low + 1;
    // 第一种写法
    // for (int times = 1; times < len; times *= 2) {
    //     int ptr = low;
    //     while (ptr + times*2 - 1 <= len) {
    //         merge(arr, ptr, ptr + times - 1, ptr + times*2 - 1);
    //         ptr += times*2;
    //     }
    //     if (ptr + times <= len) {
    //         merge(arr, ptr, ptr + times - 1, high);
    //     }
    // }

    // 第二种写法
    for (int step = 1; step < len; step <<= 1) {
        for (int index = low; index < len; index += step*2) {
            merge(arr, index, std::min(index + step - 1, len), std::min(index + step*2 -  1, len));
        }
    }
}

#endif
```
## 快速排序（递归版）
```cpp
\\ 第一种写法
#ifndef _QUICK_SORT_H
#define _QUICK_SORT_H

void swap(int &a, int &b) {
    int temp = a;
    a = b;
    b = temp;
}

void quick_sort(int *arr, int low, int high) {
    if (low >= high || arr == nullptr) {
        return;
    }
    int left = low - 1;
    for (int move = low; move != high; ++move) {
        if (arr[move] < arr[high]) {
            ++left;
            if (left != move) {
                swap(arr[move], arr[left]);
            }
        }
    }
    ++left;
    swap(arr[high], arr[left]);

    quick_sort(arr, low, left - 1);
    quick_sort(arr, left + 1, high);
    
}

#endif






\\ 第二种写法
#ifndef _QUICK_SORT2_H
#define _QUICK_SORT2_H

int partition(int[], int, int);

void quick_sort2(int arr[], int low, int high) {
    if (low >= high || arr == nullptr) {
        return;
    }

    int pivotpos = partition(arr, low, high);
    quick_sort2(arr, low, pivotpos - 1);
    quick_sort2(arr, pivotpos + 1, high);
}

int partition(int arr[], int low, int high) {
    int pivot = arr[low];
    while (low < high) {
        while (arr[high] >= pivot && low < high) --high;
        arr[low] = arr[high];
        while (arr[low] < pivot && low < high) ++low;
        arr[high] = arr[low];
    }
    arr[low] = pivot;
    return low;
}

#endif
```
## 快速排序（迭代版）
```cpp
#ifndef _QUICK_SORT_ITERATOR_H
#define _QUICK_SORT_ITERATOR_H

#include <stack>
using namespace std;

void quick_sort_iterator(int arr[], int low, int high) {
    if (low >= high || arr == nullptr) {
        return;
    }
    stack<int> stk;
    stk.push(low);
    stk.push(high);
    while (!stk.empty()) {
        int high = stk.top();
        stk.pop();
        int low = stk.top();
        stk.pop();

        int left = low, right = high;
        int pivot = arr[left];
        while (left < right) {
            while (arr[right] >= pivot && left < right) --right;
            arr[left] = arr[right];
            while (arr[left] < pivot && left < right) ++left;
            arr[right] = arr[left];
        }
        arr[left] = pivot;
        if (low < left - 1) {
            stk.push(low);
            stk.push(left - 1);
        }
        if (left + 1 < high) {
            stk.push(left + 1);
            stk.push(high);
        }
    }
    
}

#endif
```
## 堆排序
```cpp
#ifndef _HEAP_SORT_H
#define _HEAP_SORT_H

#include <algorithm>

void heap_adjust(int arr[], int pos, int high) {
    int temp = arr[pos];
    while (pos < high) {
        int index = pos*2 + 1;
        if (index < high && arr[index] < arr[index + 1]) {
            ++index;
        }
        if (index <= high && arr[index] > temp) {
            std::swap(arr[index], arr[pos]);
        }
        pos = index;
    }
}

void heap_sort(int arr[], int low, int high) {
    for (int i = (high-1)/2; i >= 0; --i) {
        heap_adjust(arr, i, high);
    }
    for (int i = high; i > 0; --i) {
        std::swap(arr[0], arr[i]);
        heap_adjust(arr, 0, i - 1);
    }
}

#endif
```
## 基数排序
```cpp
#ifndef _RADIX_SORT_H
#define _RADIX_SORT_H

#include <math.h>
#include <list>
using std::list;

void radix_sort(int arr[], int low, int high) {
    list<int> arr2[10];
    int div = 10;
    for (int i = 1; i <= 3; ++i) {
        for (int i = low; i <= high; ++i) {
            arr2[arr[i] % div / (div / 10)].push_back(arr[i]);
        }

        int index = 0;
        for (int i = 0; i != 10; ++i) {
            while (!arr2[i].empty()) {
                arr[index++] = arr2[i].front();
                arr2[i].pop_front();
            }
        }
        div *= 10;
    }
}

#endif
```
## 桶排序
```cpp
#ifndef _BUCKET_SORT_H
#define _BUCKET_SORT_H

#include <list>
#include <algorithm>
#include <limits.h>
using std::list;

void bucket_sort(int arr[], int low, int high) {
    // assume that the range of number is between 0 and 20
    int min_val = INT_MAX; int max_val = INT_MIN;
    for (int i = low; i <= high; ++i) {
        min_val = std::min(min_val, arr[i]);
        max_val = std::max(max_val, arr[i]);
    }
    int size = (max_val-min_val) / (high-low) + 1;
    int bucket_num = (high-low) + 1;
    list<int> buckets[bucket_num];
    for (int i = low; i <= high; ++i) {
        buckets[(arr[i]-min_val) / size].push_back(arr[i]);
    }

    int index = low;
    for (int i = 0; i != bucket_num; ++i) {
        buckets[i].sort();
        while (!buckets[i].empty()) {
            arr[index++] = buckets[i].front();
            buckets[i].pop_front();
        }
    }
}

#endif
```
## 计数排序
```cpp
#ifndef _COUNT_SORT_H
#define _COUNT_SORT_H

void count_sort(int arr[], int low, int high) {
    // assume that the value of k is between 0 and 9;
    int arr_k[10];
    for (int i = 0; i != 10; ++i) {
        arr_k[i] = 0;
    }

    for (int i = 0; i <= 19; ++i) {
        ++arr_k[arr[i]];
    }
    for (int i = 1; i != 10; ++i) {
        arr_k[i] += arr_k[i - 1];
    }

    int arr2[high - low + 1];
    for (int i = 0; i <= 19; ++i) {
        arr2[arr_k[arr[i]] - 1] = arr[i];
        --arr_k[arr[i]];
    }
    for (int i = 0; i <= 19; ++i) {
        arr[i] = arr2[i];
    }
}
#endif
```
## 二分查找
```cpp
// leetcode 704
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n = nums.size();
        int left = 0, right = n - 1;
        
        while (left <= right) {
            int mid = (left + right) / 2;
            if (target > nums[mid]) left = mid + 1;
            else if (target < nums[mid]) right = mid - 1;
            else return mid;
        }
        
        return -1;
    }
};
```
# 处理边界条件
## 螺旋矩阵II
```cpp
// leetcode 59
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> vec(n, vector<int>(n));
        int top = 0, left = 0, bottom = n - 1, right = n - 1;
        int current = 1;

        while (current <= n * n) {
            for (int l = left; l <= right; ++l) vec[top][l] = current++;
            ++top;
            for (int t = top; t <= bottom; ++t) vec[t][right] = current++;
            --right;
            for (int r = right; r >= left; --r) vec[bottom][r] = current++;
            --bottom;
            for (int b = bottom; b >= top; --b) vec[b][left] = current++;
            ++left;
        }

        return vec;
    }
};
```
## 设计链表
```cpp
// leetcode 707
class MyLinkedList {
    struct ListNode {
        int val;
        ListNode* next;
        ListNode(int val) : val(val), next(nullptr) { }
        ListNode(int val, ListNode* next) : val(val), next(next) { }
    };

public:
    MyLinkedList() : dummyhead(new ListNode(0)), size(0) { }
    int get(int index) {
        if (index >= size) return -1;
        ListNode* cur = dummyhead->next;
        while (index--) cur = cur->next;
        return cur->val;
    }

    void addAtHead(int val) {
        ListNode* head = new ListNode(val, dummyhead->next);
        dummyhead->next = head;
        ++size;
    }

    void addAtTail(int val) {
        ListNode *cur = dummyhead;
        while (cur->next) cur = cur->next;
        ListNode* tmp = new ListNode(val);
        cur->next = tmp;
        ++size;
    }

    void addAtIndex(int index, int val) {
        if (index > size) return;    // 特别注意，index可以等于size，此时相当于加在末尾
        ListNode *cur = dummyhead;
        while(index--) cur = cur->next;
        ListNode* tmp = new ListNode(val, cur->next);
        cur->next = tmp;
        ++size;
    }

    void deleteAtIndex(int index) {
        if (index >= size) return;
        ListNode *cur = dummyhead;
        while (index--) cur = cur->next;
        ListNode* toDelete = cur->next;
        cur->next = cur->next->next;
        delete toDelete;
        --size;
    }

private:
    ListNode* dummyhead;
    int size;
};
```
## 反转字符串中的单词
```cpp
// leetcode 151
// 标签：双指针
class Solution {
public:
    string reverseWords(string s) {
        int fast = 0, slow = 0;
        while (fast < s.size()) {
            while (fast < s.size() && s[fast] == ' ') ++fast;
            if (fast == s.size()) break;
            while (fast < s.size() && s[fast] != ' ') s[slow++] = s[fast++];
            s[slow++] = ' ';
        }
        s.resize(slow - 1);
        
        fast = 0;
        while (fast <= s.size()) {
            slow = fast;
            while (fast <= s.size() && s[fast++] != ' ');
            reverse(s.begin() + slow, s.begin() + fast - 1);
        }
        reverse(s.begin(), s.end());
        return s;
    }
};
```
# 双指针
# 字符串
## 重复的子字符串
```cpp
// leetcode 459
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        return (s + s).find(s, 1) != s.size();
    }
};
```
# 滑动窗口
## 长度最小的子数组
```cpp
// leetcode 209
// 思路：
// 如果窗口内所有数的和小于target，那么就移动窗口右边到和大于target。然后比较上次和此次的窗口大小，取小者。接着移动窗口左边到和小于target。循环直到right == nums.size()。
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int left = 0, right = 0;
        int sum = 0;
        int result = INT_MAX;
  
        while (right < nums.size()) {
            while (sum < target && right < nums.size()) sum += nums[right++];
            while (sum >= target) {
                result = min(result, right - left);
                sum -= nums[left++];
            }
        }
        
        return result == INT_MAX ? 0 : result;
    }
};
```
## 滑动窗口最大值
```cpp
// leetcode 239
class Solution {
private:
    class MyQueue { //单调队列（从大到小）
    public:
        deque<int> que; // 使用deque来实现单调队列
        // 每次弹出的时候，比较当前要弹出的数值是否等于队列出口元素的数值，如果相等则弹出。
        // 同时pop之前判断队列当前是否为空。
        void pop(int value) {
            if (!que.empty() && value == que.front()) {
                que.pop_front();
            }
        }
        // 如果push的数值大于入口元素的数值，那么就将队列后端的数值弹出，直到push的数值小于等于队列入口元素的数值为止。
        // 这样就保持了队列里的数值是单调从大到小的了。
        void push(int value) {
            while (!que.empty() && value > que.back()) {
                que.pop_back();
            }
            que.push_back(value);
        }
        // 查询当前队列里的最大值 直接返回队列前端也就是front就可以了。
        int front() {
            return que.front();
        }
    };
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        MyQueue que;
        vector<int> result;
        for (int i = 0; i < k; i++) { // 先将前k的元素放进队列
            que.push(nums[i]);
        }
        result.push_back(que.front()); // result 记录前k的元素的最大值
        for (int i = k; i < nums.size(); i++) {
            que.pop(nums[i - k]); // 滑动窗口移除最前面元素
            que.push(nums[i]); // 滑动窗口前加入最后面的元素
            result.push_back(que.front()); // 记录对应的最大值
        }
        return result;
    }
};
```
# 哈希表
```
遇到需要判断一个元素是否出现过的场景应该第一时间想到哈希法
```
## 三数之和
```cpp
// leetcode 15
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] > 0) {
                return res;
            }
  
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            int left = i + 1;
            int right = nums.size() - 1;
            while (right > left) {
                if (nums[i] + nums[left] + nums[right] > 0) right--;
                else if (nums[i] + nums[left] + nums[right] < 0) left++;
                else {
                    res.push_back(vector<int>{nums[i], nums[left], nums[right]});
                    while (right > left && nums[right] == nums[right - 1]) right--;
                    while (right > left && nums[left] == nums[left + 1]) left++;
                    right--;
                    left++;
                }
            }
        }
        return res;
    }
};
```
## 四数之和
```cpp
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        for (int k = 0; k < nums.size(); k++) {
            if (nums[k] > target && nums[k] >= 0) {
                break;
            }
            if (k > 0 && nums[k] == nums[k - 1]) {
                continue;
            }
            for (int i = k + 1; i < nums.size(); i++) {
                if (nums[k] + nums[i] > target && nums[k] + nums[i] >= 0) {
                    break;
                }

                if (i > k + 1 && nums[i] == nums[i - 1]) {
                    continue;
                }
                int left = i + 1;
                int right = nums.size() - 1;
                while (right > left) {
                    if ((long) nums[k] + nums[i] + nums[left] + nums[right] > target) {
                        right--;
                    } else if ((long) nums[k] + nums[i] + nums[left] + nums[right]  < target) {
                        left++;
                    } else {
                        result.push_back(vector<int>{nums[k], nums[i], nums[left], nums[right]});
                        while (right > left && nums[right] == nums[right - 1]) right--;
                        while (right > left && nums[left] == nums[left + 1]) left++;
                        right--;
                        left++;
                    }
                }
            }
        }
        return result;
    }
};
```
## 四数相加II
```cpp
// leetcode 454
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        unordered_map<int, int> m;
        int result = 0;
        for (int a : nums1) {
            for (int b : nums2) {
                ++m[a + b];
            }
        }
        
        for (int c : nums3) {
            for (int d : nums4) {
                if (m.find(0 - (c + d)) != m.end()) {
                    result += m[0 - (c + d)];
                }
            }
        }
        return result;
    }
};
```
# 常考算法题
## 最长回文子串
```cpp
// leetcode 5
// 标签：动态规划
// 思路：
// 定义一个二维数组，初始化为false。设i<=j，如果s[i]==s[j]并且dp[i+1][j-1]为true，那么dp[i][j]就为true。i等于j即一个字母必为回文串。j-i<=1是为了防止出现遇到dp[i+1][j-1]时i+1=s.size()或j-1=-1的越界行为。

class Solution {
public:
	string longestPalindrome(string s) {
		vector<vector<bool>> dp(s.size(), vector<bool>(s.size(), false));
		int result = 1;
		int start = 0;
		for (int j = 0; j < s.size(); ++j) {
			for (int i = j; i >= 0; --i) {
				if (s[i] == s[j] && (j - i <= 1 || dp[i+1][j-1])) {
					dp[i][j] = true;
					if(j - i + 1 > result) {
						result = j - i + 1;
						start = i;
					}
				}
			}
		}
		return s.substr(start, result);
	}
};
```
## 最长公共子串
```cpp
// nowcoder HJ75
// 标签：动态规划
// 思路：
// dp[i][j]表示以str1中第i个字符，str2中第j个字符作为公共子串结尾字符时的公共子串的长度。maxlen表示最长公共子串的长度。有以下两种情况：
// 1. 如果str1中第i和str2中第j个字符相同，则在以i-1和j-1结尾的子串后面加上i、j一位仍然是子串，因此dp[i][j]=dp[i−1][j−1]+1。
// 2. 如果str1中第i和str2中第j个字符不相同，则以他们结尾的子串不可能相同，dp[i][j]=0。
// 如果更新后的dp[i][j]比maxlen大，则更新maxlen。
#include <iostream>
#include <vector>
using namespace std;

int main() {
    string str1, str2;
    while(cin >> str1 >> str2) {
        int maxLen = 0;
        vector<vector<int>> dp(str1.size() + 1, vector<int>(str2.size() + 1, 0));
        
        for (int i = 1; i <= str1.size(); ++i) {
            for (int j = 1; j <= str2.size(); ++j) {
                if (str1[i - 1] == str2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = 0;
                }
                if (maxLen < dp[i][j]) {
                    maxLen = dp[i][j];
                }
            }
        }
        
        cout << maxLen << endl;
    }
    return 0;
}
```
## 找出字符串中第一个匹配项的下标（KMP算法）
```cpp
// leetcode 28
class Solution {
public:
    int strStr(string haystack, string needle) {
        int n = haystack.size(), m = needle.size();
        if(m == 0) return 0;
        //设置哨兵
        haystack.insert(haystack.begin(),' ');
        needle.insert(needle.begin(),' ');
        vector<int> next(m + 1);
        //预处理next数组
        for(int i = 2, j = 0; i <= m; i++){
            while(j and needle[i] != needle[j + 1]) j = next[j];
            if(needle[i] == needle[j + 1]) j++;
            next[i] = j;
        }
        //匹配过程
        for(int i = 1, j = 0; i <= n; i++){
            while(j and haystack[i] != needle[j + 1]) j = next[j];
            if(haystack[i] == needle[j + 1]) j++;
            if(j == m) return i - m;
        }
        return -1;
    }
};
```
## 环形链表II
```cpp
// leetcode 142
// 标签：数学
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast = head;
        ListNode* slow = head;
        
        while(fast != nullptr && fast->next != nullptr) {
            slow = slow->next;
            fast = fast->next->next;
            if (slow == fast) {
                ListNode* index1 = fast;
                ListNode* index2 = head;
                while (index1 != index2) {
                    index1 = index1->next;
                    index2 = index2->next;
                }
                return index2;
            }
        }
        
        return nullptr;
    }
};
```
## 最大连续bit数
```cpp
// nowcoder HJ86
// 标签：数学
// 思路：
// 如果有连续的1那么每次左移并和原来的数相交，就会消去一个1。如果不存在连续的1，那么则为0。
// 原来的：10111001
// 左移后：01110010
// 相交后：00110000
// 可以看到两个及以上连续的1相交后减少一个1，单独的不连续的1就消去了
#include <iostream>
using namespace std;

int main() {
    int n;
    while(cin >> n) {
        int count = 0;
        for(; n != 0; ++count) n &= n << 1;
        cout << count << endl;
    }
    return 0;
}
```