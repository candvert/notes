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
- [常考算法题](#常考算法题)
	- [最长回文子串](#最长回文子串)
	- [最长公共子串（KMP算法）](#最长公共子串（KMP算法）)
	- [最大连续bit数](#最大连续bit数)

## 命名空间
```
三种方法使用c++标准库中的标识符
1. std::cout

2. 使用using declaration：using std::cout;

3. 使用using directive：using namespace std;
```
# 常用函数
```
vector数组
	push_back()，pop_back()
	size()，empty()，clear()
	begin()，end()，rbegin()，rend()
string字符串
	getline(cin, s)
	substr(start, length)，find(str)，replace(start, count, str)
	replace(const_iterator first, const_iterator last, const basic_string& str)
	stoi(str)，stol(str)，to_string(value)
	push_back()，pop_back()
	size()，empty()，clear()
	begin()，end()，rbegin()，rend()
stack
	push()，pop()，top()，empty()，size()
queue
	push()，pop()，front()，back()，empty()，size()
priority_queue优先队列（堆）
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
## 最长公共子串（KMP算法）
```cpp

```
## 最大连续bit数
```cpp
// nowcoder HJ86
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