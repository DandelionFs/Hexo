---
title: '[C++] IMU'
date: 2020-04-14 03:00:00
toc: true
---

<center><font size =2 color=grey>CD:2020-01-12 01:29:05</font></center>
</br>

### 0x00 Proface

实现

### 0x01 Data Transform

Q：

```c++
int main() {
	int a;
	cin >> a;
	cout << (5 / 9.0) * (a - 32) << endl; //还是和数字运算的时候不同类型（int double）的 转化相关
	cout << (5 / 9) * (a - 32) << endl;
	cout << 5 / 9 * (a - 32) << endl;
	cout << 5 * (a - 32) / 9 << endl;
}
```

> 编程的过程中需要将值从一种数据类型转换为另一种数据类型 ，C++ 提供了这样做的方法：
>
> 当运算符的操作数具有不同的数据类型时，C++ 会自动将它们转换为相同的数据类型。当它这样做时，遵循一组规则。理解这些规则将有助于程序员防止一些细微的错误蔓延到自己的程序中。就像军队的军官有军阶一样，数据类型也可以按等级排名。如果一个数字数据类型可以容纳的数字大于另一个数据类型，那么它的排名就高于后者。例如，float类型就超越了 int 类型，而 double 类型又超越了 float 类型。

 数字优先级（逐级递减)  ：` long double`、`float``unsigned long long int`    、`long long int`、`unsigned long int`、`long int`、`unsigned int`、`int`。



**注意**：**上溢**（`OVERFLOW`），他的`int` 为3。

```cpp
int main(){
	int  sum = 1,i;
	for (i = 1; i <= 32; i++) sum *= 2;
	cout << sum;
}
```

运行这段代码你会发现结果是`0`,但其实结果是`4,294,967,296`,已经远超int的范围了，所以应该换成`long long`或者`double`才可以。



### 0x02  Auto

> auto不能作为函数的参数
>
> auto不能直接用来声明数组
>
> 为了避免与C++98中的auto发生混淆，C++11只保留了auto作为类型指示符的用法 ，只是一个占位符
>
> auto不能定义类的非静态成员变量
>
> 实例化模板时不能使用auto作为模板参数

```cpp
int main() {
	int a;
	auto m = a;//C++11后auto 声明的变量必须由编译器在编译时期推导而得
	auto n;//用auto声明引用类型时则必须加&
	auto q = 3.14;//用auto声明指针类型时，用auto和auto*没有任何区别
	auto w = 4;
	auto r = " ";
	auto v = 3, d = 3.0;//求同一类型
}
```

针对于`auto`的使用，在处理复杂类型，如标准模块库（STL）中的类型时，才能显现出来
例如，对于下述C++98代码：——《C++ Primer Plus》

```cpp
std::vector<double> scores；
std::vectorsdouble>::iterator pv = scores.begin();
//C++11允许您将其重写为下面这样:
std::vector<double> scores;
auto pv = scores.begin();
```

对于使用`auto`形成的新的循环:

```cpp
#include<iostream>
using namespace std;
int main() {
	int array[] = { 1,2,3,4,5 };
	for (auto& e : array)e *= 2;
	for (auto& e : array)cout << e << " ";
	cout << endl;
	return 0;
}
```

还是很神奇的，`auto`应该是C++11里面的新特性。

</br>

</br>



### 0x03 Prime

这道题的历史比较悠久了，我还是挖出来写了一写，发现和第一次写差不太多诶!!!(bug满天飞，想哭woc)······
在不考虑时间复杂度的时候（测试数据不大）可以直接从 2 暴力循环到 该数-1，好处是不用动脑子，简单暴力；以下提供优化计算速度和时间复杂度的方法：

<font color ="red">**源于证明（所有 ≤√m 的数都不能整除 m，则＞√m的数也一定不能整除m。O(n*log(n))**</font>

```cpp
int prime(int a){
if(a>1){
    for(int i=2;i<=sqrt(a);i++)
        if(!(a%i)) return 0;
    return 1;
    }
else return 0;
}
//for(i=2;i<=sqrt(a)+1;i++)||for(i=2;i < sqrt(a)+1;i++)这两种写法1会是个问题？
//看上去不错，但其实很不经济。我们知道，我们的算法如果写成线性算法，也就是O(n)，已经算是不错了，但是最好的是O(Log(n))的算法，这是一个对数级的算法，著名的二分取中（Binary Search）正是O(Log(n))的算法。通常来说，O(Log(n))的算法都是以排除法做为手段的。所以，找质数的算法完全可以采用排除法的方式。
```

* <font color ="red">**筛取质数的方法——埃拉托斯特尼筛法(埃氏筛 O(n(log(logn)))**</font>

  > 埃拉托斯特尼筛法，简称埃氏筛或爱氏筛，是一种由希腊数学家埃拉托斯特尼所提 出的一种简单检定素数的算法。
  > 给出要筛选的范围N，找出∨N以内的素数 p1,p2,p3...pk，不断用素数去筛。如果一次筛完这个序列中最大数小于等于最后一个标出质数的平方，那么剩下来的数都是质数，否则返回重新筛选。

   简单模拟这个过程如下 :

  <img src="http://img.dandelionflowers.xyz/Sieve%20of%20Eratosthenes%20animation.gif" alt="img" style="zoom: 67%;" />

伪代码：

```cpp
  Input: an integer n > 1
  Let A be an array of Boolean values, indexed by integers 2 to n,
  initially all set to true.
   for i = 2, 3, 4, ..., not exceeding  ∨n:
    if A[i] is true:
      for j = i^2, i^2+i, i^2+2i, i^2+3i, ..., not exceeding n :
        A[j] := false
  Output: all i such that A[i] is true.
```

代码示例（Wikipedia）：

```cpp
  #include <vector>
  auto eratosthenes(int upperbound){
      std::vector<bool> flag(upperbound+1, true);
  	flag[0]=flag[1]=false; //exclude 0 and 1
  	
  	for(int i=2; i<=upperbound; ++i)
  	    if(flag[i]) // if i is prime number
  		    for(int j=i*i; j<=upperbound; j+=i)
  			    flag[j] = false;
  	return flag
  }
```

最终实现：

```cpp
  const int MAXN = 100000001;
  int p[MAXN];//这里避免使用了memset，合数是1，素数是0
  void Prime(int end) {
  	p[0] = p[1] = 1;
  	for (int i = 2; i < sqrt(end); i++) {
  		if (p[i])continue;
  		for (int j = i * 2; j < end; j += i)p[j] = 1;
  	}
  }
```

  

* **实际应用的算法**：实际应用中，我们通常不会使用上述的两种算法，因为那是理论学院派的算法。实际中的算法是，我**把质数事先就计算好，放在一个文件中，然后在程序启动时（注意是在启动时读这个文件，而不是运行时每调用一次就读一次文件），读取这个文件，然后打印出来就可以了**。如果需要查找的话，二分查找或是hash表查找将会获得巨大的性能提升。当然，这样的方法对于空间来说比前面两个都要消耗得大，但是你可以有O(log(n))或是O(1)的时间复杂度。对应现实中很多事情不是计算出来的，而是事先写在文件中的。

* <font color ="red">**线性筛法  O(n)**</font>
  原理：对于任意合数，必定可以有最小质因子乘以最大因子的分解方式。因此，对于每个合数，只要用最大因子筛一遍，枚举时只要枚举最小质因子即可

  （运用到欧拉函数的知识，这里还没有学习，看了好半天也没有看懂，先在这里立下一个小小的Flog）[·](https://www.wikii.cc/zh-cn/%E6%AC%A7%E6%8B%89%E5%87%BD%E6%95%B0)   [·](https://coolshell.cn/articles/3738.html)   [·](https://www.cnblogs.com/grubbyskyer/p/3852421.html) 

  ​	

</br>

### 0x04 Sort 

| 排序法     | 最差时间分析 | 平均时间复杂度 | 稳定度 | 空间复杂度    |
| ---------- | ------------ | -------------- | ------ | ------------- |
| 冒泡排序   | O(n2)        | O(n2)          | 稳定   | O(1)          |
| 插入排序   | O(n2)        | O(n2)          | 稳定   | O(1)          |
| 选择排序   | O(n2)        | O(n2)          | 稳定   | O(1)          |
| 二叉树排序 | O(n2)        | O(n*log2n)     | 不一顶 | O(n)          |
| 快速排序   | O(n2)        | O(n*log2n)     | 不稳定 | O(log2n)~O(n) |
| 堆排序     | O(n*log2n)   | O(n*log2n)     | 不稳定 | O(1)          |
| 希尔排序   | O            | O              | 不稳定 | O(1)          |
| 归并排序   | ...          | O（n*log2n）   | ... | ...          |


</br>

#### 4.1  冒泡排序

类似冒泡泡一般的排序算法，不过多解释

```c++
void bubbles(int p[], int m) {
	int i, j;
	for (i = 0; i < m; i++) {
		for (j = i + 1; j < m - 1; j++) {
			if (p[i] < p[j])swap(p[i], p[j]);
		}
	}
}
```

<br/>

#### 4.2  选择排序

```c++
void selections(int p[], int m) {
	int k, i, j;
	for (i = 0; i < m; i++) {
		k = i;
		for (j = i + 1; j < m - 1; j++) {
			if (p[k] > p[j]) k = j;//是p[k]而不是p[q]的原因是因为一找到比他大的就赋值，会导致最后交换的不是最大的数的后果。这样才会记录最大值。最后用于交换！！
		}
		if (k != i)swap(p[i], p[k]);
	}
}
```

还可以计算一下交换的次数，应该注意的一点是：



<br/>

#### 4.3 插入排序

主要思想：从第二个元素开始，依次比较大小，小的去前面，大的去后面，采用的方式是最低效的平移数组，针对于基本有序的数列很高效。

```c++
void InsertSort(int p[], int start, int end) {
	for (int i = start + 1; i < end; i++) {
		int temp = p[i];//相当于一个容器，将无序数列的第一个元素取出
		int j = i - 1;//作为有序数列的最后一个元素
		while (j > 0 && temp < p[j]) {
			p[j + 1] = p[j];
			j--;
		}
		p[j + 1] = temp;
	}
}
```

<br/>

#### 4.4 希尔排序（缩小增量排序）

作为插入排序的改进版,，先分组进行排序使得数列基本有序，再用插入排序进行排序。

```c++
void Shellsort(int p[], int len) {
	for (int i = len / 2; i < len; i /= 2) {
		for (int j = i; j < len; j++) {
			int temp = i;
			while (temp - j >= 0 && p[temp] < p[temp - j]) {
				swap(p[temp], p[temp - j]);
				temp -= i;
			}
		}
	}
}
```

<br/>

#### 4.5 桶排序





#### 4.6  Set

**明明的随机数**

有两种方法 ：其一，是用笨办法遍历一遍找出哪个是最多的，再遍历一遍得出相应的位置。是上学期的期末考试试题。

```c++
int main(){
	int N, a, j, h = 0, m, n, sum = 0;
	cin >> N;
	int* p = new int[N];
	for (int i = 0; i < N; i++)cin >> p[i];
	for (m = 0; m < N; m++)
		for (n = 1; n < N; n++)
			if (p[m + n] == p[m]) p[m] = 0;
	sort(p, p + N);
	for (j = 0; j < N; j++) if (p[j] != 0) sum += 1;
	cout << sum << endl;
	for (j = 0; j < N; j++) if (p[j] != 0) cout << p[j] << " ";
}
```

其二，就是使用桶排序

```c++
int main() {
	int n, x;
	cin >> n;
	int sum(0), bus[1002] = { 0 };
	for (int i = 1; i <= n; i++) {
		cin >> x;//if (bus[x]) continue;
		bus[x]++;
		sum++;
	}
	cout << sum << endl;//根据需求遍历一遍找最大值
}
```

其三，用STL库中的Set（集合做）[·](https://www.cnblogs.com/yaoyueduzhen/p/4536929.html)

> begin() 　　 返回set容器的第一个元素的 地址
> end() 　　 返回set容器的最后一个元素 地址
> clear() 　 　 删除set容器中的所有的元素
> empty() 　　　 判断set容器是否为空
> max_size() 　 返回set容器可能包含的元素最大个数
> size() 　　　　 返回当前set容器中的元素个数
> erase(it) 删除迭代器指针it处元素
> insert(a) 插入某个元素

```c++
#include<set>
int main() {
	int a;
	set<int>number;
	while (cin >> a) {
		if (a != -1)number.insert(a);
		else break;
	}
	cout << *(number.begin()) << endl;
}
```

<br/>

#### 4.7  **快排**

```c++
int* p;
void qsort(int l, int r) {
	int mid = p[(l + r) / 2];
	int i = l, j = r;
	do {
		while (p[i] < mid) i++;
		while (p[j] > mid) j--;
		if (i <= j) {
			swap(p[i], p[j]);
			i++;
			j--;
		}
	} while (i <= j);//注意等号
	if (l < j) qsort(l, j);//递归搜索左半部分
	if (i < r) qsort(i, r);//递归搜索右半部分
}
```

**拓展：**二分查找

```c++
int main() {
	int p[100], i = 0, step = 1, n;
	while (cin >> p[i]) {
		if (p[i] == 0) {
			p[i] = '\0';
			break;
		}
		i++;
	}
	int len = i, start = 0;
	sort(p, p + len);
	cin >> n;
	for (int j = 0; j < i; j++)cout << p[j] << " ";
	cout << endl;
	while (start <= len) {
		if (n == p[(start + len) / 2]) break;
		else if (n > p[(start + len) / 2]) start = (start + len) / 2 + 1;
		else if (n < p[(start + len) / 2]) len = (start + len) / 2 - 1;
		step++;
	}
	cout << (start + len) / 2 << endl << step << endl;
	return 0;
}
```

</br>

<br/>

#### 4.8. sort

```c++
#include<algorithm>
#define sorts(p,m) sort(p,p+m);
sorts(p,m);//此处只做参考，具体使用的时候再做打算
//sort ( start , end , cmp(compare) )
//start表示要排序数组的起始地址；
//end表示数组结束地址的下一位；
//cmp用于规定排序的方法，可不填，默认升序。
```

<br/>

#### 4.9 优先队列——堆排序

```c++
#include<iostream>
#include<queue>
using namespace std;
int main() {
	priority_queue<int>q;//优先队列会从大到小输出
	for (inti = 1; i <= 5; i++)q.push(i);
	for (inti = 0; i < 5; i++) {
		cout << q.top() << endl;
		q.pop();
	}
	return 0;
}
```

</br>

#### 4.10 归并排序——分治

运用**递归化简到两两排序**和二分归并

```cpp
int a[10] = { 13 , 27 , 19 , 2 , 8 , 12 , 2 , 8 , 30 , 89 };
int b[10];
void Merge(int a[],int s,int m,int e,int tmp[]) {
	int pb = 0;
	int p1 = s, p2 = m + 1;
	while (p1 <= m && p2 <= e) {
		if (a[p1] < a[p2]) tmp[pb++] = a[p1++];
		else tmp[pb++] = a[p2++];
	}
	while (p1 <= m) tmp[pb++] = a[p1++];
	while (p2 <= e) tmp[pb++] = a[p2++];
	for (int i = 0; i < e - s + 1; i++) a[s+i] = tmp[i];
}
void MergeSort(int a[], int s, int e, int tmp[]) {
	if (s < e) {
		int m = (s + e) / 2;//向下取整得到m=s或写作 m=s + (e - s) / 2;
		MergeSort(a, s, m, tmp);
		MergeSort(a, m + 1, e, tmp);
		Merge(a, s, m, e, tmp);
	}
}
int main() {
	int size = sizeof(a) / sizeof(int);
	MergeSort(a, 0, size - 1, b);
	for (int i = 0; i < size; ++i)cout << a[i] << " ";
	return 0;
}
```







#### 4.11 Struct&sort

[1936] 学生的年龄排序、[1888]总分排序、[1225]日期排序

```c++
bool cmp(S a, S b) {
	return a.age < b.age;
}
sort(stu, stu + n, cmp);//可以用上面提到的各种排序算法来实现你的代码，这里避免赘余不说了
```

注意：

1. 偷懒的时候注意各结构体变量的加权（OVERFLOW）

</br>

</br>

### 0x05 Big Date

#### 5.1 +

</br>

斐波那契数列

```c++
int main() {
	int N, j;
	cin >> N;
	if (N < 3) {
		if (N < 0) return 0;
		if (N == 0)cout << "0";
		if (N == 1)cout << "1";
		if (N == 2)cout << "1";
	}
	else {
		N -= 2;
		int a[5000] = { 0 }, b[5000] = { 0 }, c[5000] = { 0 };
		a[0] = 1;
		b[0] = 1;
		for (int i = 0; i < N; i++) {
			for (j = 0; j < N; j++) {
				c[j] += a[j] + b[j];
				if (c[j] > 9) {
					c[j] -= 10;
					c[j + 1] = 1;
				}
			}
			swap(a, b);
			swap(b, c);
			for (int i = 0; i < 5000; i++) c[i] = 0;
		}
		for (j; b[j] == 0; j--);
		for (j; j >= 0; j--)cout << b[j];
	}
}

```

</br>

#### 5.2 -

</br>

#### 5.3 *

</br>

#### 5.4 /

</br>

 被17整除

```c++
int main()
{
	char p[1000];
	while (cin.getline(p, 1000)) {
		if (p[0] != '0') {
			int sum = 0;
			for (int i = 0; p[i] != '\0'; i++) {
				sum *= 10;
				sum += p[i] - '0';
				if (sum > 17)sum %= 17;
			}
			if (sum == 0)cout << "1\n";
			else cout << "0\n";
		}
	}
}
```

</br>



### 0x06 GCD&LCM [·](https://blog.csdn.net/qq_42504734/article/details/88369780)

#### 6.1 辗转相除法（欧几里德算法）（Euclidean algorithm）

Greatest common divisor&least common multiple10.29[·](https://blog.csdn.net/qq_42504734/article/details/88369780)

基于一个定理：两个正整数a和b（a>b），它们的最大公约数等于a除以b的余数c和b之间的最大公约数。但是当数字比较大的时候a%b的性能会变差也是理所当然。

```c++
int cdivisor(int m, int n) {
	int a, b, r;
	a = (m > n) ? m : n;
	b = (m > n) ? n : m;
	r = b;
	while (r != 0) {
		r = a % b;
		a = b;
		b = r;
	}
	return a;
}
```

</br>

#### 6.2 更相减损术

出自《九章算术》，也是一种求最大公约数的算法。两个正整数a和b（a>b），它们的最大公约数等于a-b的差值c和较小数b的最大公约数。（可以用递归实现）

```c++
int cdivisor(int a, int b) {
	while (a != b) {
		if (a > b)a -= b;
		else b -= a;
	}
	return a;
}
```

</br>

### 0x07 Circle

菱形

```c++
int main() {
	cout << "请输入半菱形高度n\n";
	int n = 0;
	cin >> n;
	for (int i = 0; i < 2 * n + 1; i++) {
		for (int j = 0; j < 2 * n + 1; j++) {
			if (abs(i - n) + abs(j - n) == n - 1) cout << "*";
			else cout << " ";
		}
		cout << endl;
	}
}
```

</br>

### 0x08 Number System

#### 8.1 十进制->N进制

**[除N取余法]**：如十进制整数10转化为2进制的过程为：

```cpp
10/2 = 5余0
5/2 = 2余1
2/2 = 1余0
1/2 = 0余1 
```

所以二进制形式为`1010`.

**[降幂法]：**将十进制整数不断减去与该整数最接近的N进制整数的位权，如果够减m次则对应N进制位上的数字为m(m<N)，否则为0。得到的差值作为新的被减数进行下一次计算，直到被减数为0.

```cpp
30-82=66
66-82=2
2-8（不够减）
2-1=1
1-1=1
```

所以对应的8进制整数为`202`.

#### 8.2  十进制->小数转换

**[乘N取整法]**：十进制的0.54转换为16进制的过程为：

```cpp
0.55*16=8.8 --8
0.8*16=12.8 --12(C)
0.8*16=12.8 --12(C)
0.8*16=12.8 --12(C)
...
```

由于不能被精确的转换，我们可以只取前4位，为`0.8CCC`.

<font color="red">一般的十进制数转换为N进制数，分别转换整数和小数部分。</font>

**[注]：** **倒序处理！**

这是十进制转化二进制!%N处理没有进位的数字，然后相当于转化后的数字移位，转化的数字再从个位开始的操作。但是从十进制到其他进制的时候就画风不一样了。<font color="red">%出来的是个位开始，也就是说必须从后面开始输出。</font>

简单来说，不倒序处理无法处理进位。

```cpp
#include<iostream>
using namespace std;
int main(){
  int m,i;
  char p[1000];
  cin>>m;
  for(i=0;m!=0;i++){
      p[i]=(m%2)+'0';
      m=m/2;
  }
    p[i]='\0';
    for(int j=i-1;j>=0;j--)cout<<p[j];	
}//这里采取倒序输出是因为开始处理的位数是个位，所以最后输出的位数也因该是倒序输出，关于这类问题，首先着手的是自己处理的位数是哪位！是关于移位处理的进制转换问题，
```

这是二进制转化十进制的操作！

```cpp
#include<iostream>
#include<cstring>
using namespace std;
int main(){
  int sum=0;
  char p[1000];
  cin.getline(p,1000);
  int len=strlen(p);
  for(int i=0;i<len;i++){
      sum*=2;
      sum+=p[i]-'0'; 
  }
    cout<<sum<<endl;
}//getline读取后的数组从最大位置开始，然后迭代算到个位结束。
```

</br>

std::bitset（转2进制），std::oct（转8进制），std::dec （转10进制），std::hex（转16进制）。另外发现的一个有趣的点是VS内置iostream是支持strlen的操作的，但是不不通用真是鸡肋的说·····

```c++
#include<cstring>
void conversionnum(int m, int j, char p[]) {//m是原来的进制，j是转换进制，p是原来的数组
	int i, sum = 0;
	char z[1000];
	for (int i = 0; i < strlen(p); i++) {
		sum *= m;
		if (p[i] >= 'A' && p[i] <= 'Z')sum += p[i] - 'A' + 10;
		else if (p[i] >= '0' && p[i] <= '9')sum += p[i] - '0';
	}//将读回来的字符数组转化成为对应十进制的数字（可不可以一步从m到j转化）
	i = 0;
	if (sum > j) {
		for (i = 0; sum != 0; i++) {
			if ((sum % j) <= 9)z[i] = (sum % j) + '0';
			else z[i] = (sum % j) + 'A' - 10;
			sum = sum / j;
		}
	}//对应的进制转换（貌似不用提前讨论转换后是不是比十进制要大···以前写复杂了）
	else z[i++] = sum + '0';
	z[i] = '\0';
	for (int j = i - 1; j >= 0; j--) cout << z[j];
	cout << endl;
}
```

</br>

#### 8.4 堆栈

</br>

### 0x09 Newton's method

牛顿迭代，其又称为**牛顿-拉夫逊方法（Newton-Raphson method）**是牛顿在17世纪提出的一种在实数域和复数域上近似求解方程的方法。 多数方程不存在求根公式，因此求精确根非常困难，甚至不可能，从而寻找方程的近似根就显得特别重要。方法使用函数f(x)的泰勒级数的前面几项来寻找方程f(x) = 0的根。牛顿迭代法是求方程根的重要方法之一，其最大优点是在方程f(x) = 0的单根附近具有平方收敛，而且该法还可以用来求方程的重根、复根。

解非线性方程f(x)=0的牛顿法是把非线性方程线性化的一种近似方法。把f(x)在x0点附近展开成泰勒级数 f(x) = f(x0)+(x－x0)f'(x0)+(x－x0)^2*f''(x0)/2! +… 取其线性部分，作为非线性方程f(x) = 0的近似方程，即泰勒展开的前两项，则有f(x0)+f'(x0)(x－x0)=f(x)=0 设f'(x0)≠0则其解为x1=x0－f(x0)/f'(x0) 这样，得到牛顿法的一个迭代序列：x(n+1)=x(n)－f(x(n))/f'(x(n))。

迭代公式具体为Xn+1=(Xn+a/xn)/2 (n=0,1,2,3….;X0=a/2)，

C++实现：
```c++
#include<iostream>
#include<cmath>
using namespace std;
int main() {
	double a, x0, x;
	cout << "please input a:";
	cin >> a;
	x0 = a / 2;
	x = (x0 + a / x0) / 2;
	while (fabs(x - x0) > 0) {
		x0 = x;
		x = (x0 + a / x0) / 2;
		cout << x << endl;
		cout << x0 << endl;
	}
	cout << x << endl;
}
```

</br>

### 0x0a 二分查找

二分查找思想以及查找次数——它的主要思想就是在一个有序数列中取中间的数作为基点，比大小，看是从左找还是右面找

```c++
void binarysh(int p[], int len, int goal) {
	int start = 0, step = 1, mid;
	while (start <= len) {
		mid = (start + len) / 2;//取整操作后的整数是抛掉所有的小数部分（!=四舍五入）
		//如果是偶数（数组），以上操作取整后不会造成任何影响
		//如果是奇数（数组），那么中间数会前移一位，造成前小后大的操作
		//这样的话如果在中间数的后一位，关于这里取整后查找的次数就存在商榷（从你的全世界路过）
		//基于1 2 3 4 5 6 7 8 查找4得出
		if (goal == p[mid])break;
		else if (goal < p[mid])len = mid - 1;
		else if (goal > p[mid])start = mid + 1;//这里头加一和尾减一才可以交错开
		step++;
	}
	cout << mid << " " << step << endl;
}
```

</br>

### 0x0b 读取一行类函数

#### 5.1   `get()`成员类函数

该函数有几种变体，其中一种`cin.get()`可用来接收一行字符串，可接收空格,自动接收一个 ‘\0’

 ```cpp
char p[1000];
cin.get(p, 100);    
cout << p ;
 ```

 cin.get(无参数) :用于舍弃输入流中不需要的字符，或者舍弃回车，弥补cin.get(字符数组名，接收字符数)的不足

```cpp
char a, c;
cin >> a;
cin.get(c);
while (getchar() != '\n')
cout << a;
cout << c;
```

对于没有窗口停留的编译器最好加上以下两行代码：

```c++
    cin.get();//add this statement 
    cin.get();//and maybe this, too 
    return 0;
```

cin.get（）语句读取下一次键击，因此上述语句让程序等待，直到按下了Enter键（在按下Enter键之前，键击将不被发送给程序，因此按其他键都不管用）。如果程序在其常规输入后留下一个没有被处理的键击，则第二条语句是必需的。例如，如果要输入一个数字，则需要输入该数字，然后按Enter键。程序将读取该数字，但Enter键不被处理，这样它将被第一个cin.get（）读取。

get()家庭还有这个？具体是什么暂时还不太清楚？

```cpp
#include<conio.h>
#include<iostream>
using namespace std;
int main() {
	char p;
	p = getche();
	cout << endl << p;
}
```

fgets函数——get_s（）得到一个包含空格的字符串？？？？





<br/>

#### 5.2 `getline()`成员函数

:接收一个字符串，可以接收空格并输出,自动接收一个 ‘\0’ 

**cin.getline(接收字符串,接收个数，(结束字符))**

```cpp
char p[100];
cin.getline(p, 100);
cout << p;
```

**注意：**getline :接收一个字符串，可以接收空格并输出。用`getline`处理`string`而不是字符数组的时候，需包含头文件 #include <string>，因为`<iostream>`头文件中包含操作字符数组的预处理信息（ cin.getline() 属于 iostream 流，而 getline() 属于 string 流，是不一样的两个函数）。

---

`get()（fgets、cin.get()）`和`getline()`的区别：

> `istream`中的类（如`cin`）提供了一些面向行的类成员函数：`getline()`和`get()`，这两个函数都读取一行输入，直到到达换行符。然而，随后`getline()`将丢弃换行符，而`get()`将换行符保留在输入序列中。——《C++ Primer Plus》

 特别注意：这里我再补充一点`cin.getline`是读取换行符的，只是保存的时候将换行符丢弃了，保存的数组中换行符`\n`变成了 `\0.`但是`cin.get()`则不同，它不再读取并丢弃换行符，而是将其留在输入队列中。测试代码如下：

```cpp
#include <iostream>
using namespace std;
int main() {
	char p[5];
	cin.get(p,5);//cin.getline(p,5)
	char m = cin.get();
	cout << p << endl;
	cout << m;
}
```

由于第一次调用后，换行符将留在输入队列中，因此第二次调用时看到的第一个字符便是换行符。因此`get()`认为已到达行尾，而没有发现任何可读取的内容。如果不借助于帮助，`get()`将不能跨过该换行符。

幸运的是，`get()`有另一种变体——`cin.get（）`调用可读取下一个字符（即使是换行符），因此可以用它来处理换行符，为读取下一行输入做好准备。也就是说，平时敲代码的时候常常出现的下面的调用序列：

 ```cpp
cin.get(p,50);
cin.get();
cin.get(q,50);
//还有一种方式把1和2连接前来
cin.get(p,50).get();
cin.getline(p1,50).getline(p2,50);
 ```

之所以可以这样做，是由于`cin.get（p,50）`返回一个cin对象，该对象随后将被用来调用`get()`函数。同样下面的语句将把输入中连续的两行分别读入到数组p1和p2中，其效果与两次调用`cin.getline()`相同。有趣的是还有一种读取类似于`cin.get()`，那就是`getchar()`。类似的行为还存在于数字和字符串混合输入的时候，需要在数字后面先把换行符吃掉，才可以正常的读取一行的操作。

 ```cpp
char ch = cin.get();
cout << ch;
char zh =getchar();
cout<<zh;
（cin>>year）.get();
cin.getline(p,50);
 ```

但是有一点要声明的是：`ctrl+z`读取回来的值是`-1`，空格读取回来是ASKII的值`32`,h下面是我的测试代码： `cout << getchar();` 。为什么要使用get()，而不是getline()呢？

1. 老式实现没有`getline()`。

2. get()使输入更仔细。
   例如，假设用get()将一行读入数组中。如何知道停止读取的原因是由于已经读取了整行，而不是由于数组已填满呢？查看下一个输入字符，如果是换行符，说明已读取了整行；否则，说明该行中还有其他输入。

总之，getline()使用起来简单一些，但get()使得检查错误更简单些。可以用其中的任何一个来读取一行输入；只是应该知道，它们的行为稍有不同。

疑点：空行和其他问题：

> 当getline()或get()读取空行时，将发生什么情况？最初的做法是，下一条输入语句将在前一条getline()或get()结束读取的位置开始读取；但当前的做法是，当get()（不是getine()读取空行后将设置失效位(failbit)，这意味着接下来的输入将被阻断，但可以用下面的命令来恢复输入：cin.clear()；另一个潜在的问题是，输入字符串可能比分配的空间长。如果输入行包含的字符数比指定的多，则getline()和get()将把余下的字符留在输入队列中，而getline()还会设置失效位，并关闭后面的输入。
>
> 还是不太懂失效位(failbit)？？？？是做什么的？？？？

#### 5.3 Puts的操作

这里还是不太清楚，先上代码：

```cpp
#include <stdio.h>
using namespace std;
int main(){
    char s[81];
    int i;
    gets(s);
    for (i = 0; s[i] != '\0'; i++){
        if (s[i] >= 'a' && s[i] <= 'z'){
            if (s[i] == 'z') s[i] = 'a';
            else s[i] += 1;
        }
    }
    puts(s);
    return 0;
}
```



<br/>

### 0x0c `String`类库 

有趣的一点是，string和字符数组有时是可以相互转化的

#### 6.1 输出操作（类切片）

```c++
char p[100];
cin>>p;
cout<<string(p,5);
cout<<(p,5);
```

然后就真的可以输出到那个位置就结束了（一共5个字符）

#### 6.2 对象连接

体现string优越的可能只有这个了

```c++
int main(){
	string a,b;
	cin>>a>>b;
	string c=a+b;
	cout<<c<<endl;
}
```

```c++
int main(){
	char a[]="dsafjuhjs";
	char b[]="sdjfhjshf";
	char *p=(char*)malloc(strlen(a)+strlen(b)+1);
	strcpy(p,a);
	strcat(p,b);
	cout<<p<<endl;
}
```

string的写法优化

```cpp
#include<iostream>
using namespace std;
int main() {
	char p[100];
	cin >> p;
	cout << string(p, 5);
}//类似于Python的切片操作，原理是字符数组和字符串是可以转化的
```

string和字符数组可以相互转化

```cpp
#include<iostream>
using namespace std;
int main() {
	string a;
	cin >> a;
	cout << a.c_str() << endl;
	//这样就可以从标准输入里输入任意长的字符串，并按const*char来使用。
	//如果要把一个char转换成string,可以使用strings(char*);
	cout << a;
}
```

`string`的大小是提前设定好的：

```cpp
#include <iostream>
using namespace std;
int main() {
	while (1) {
		string a;
		cin >> a;
		cout << sizeof(a);
	}
}
```

在我的VS里面`sizeof`大小永远是28.另外，找了一下`string`的函数，列在下面：

|                函数                |                         作用                          |
| :--------------------------------: | :---------------------------------------------------: |
|      `    string s(cstr)    `      |                 将C字符串作为s的初值                  |
| `  string s(chars,chars_len)     ` |     将C字符串前chars_len个字符作为字符串s的初值。     |
|     `  string s(num,c)      `      |            生成一个字符串，包含num个c字符             |
|     `    string s(beg,end)  `      |   以区间beg;end(不包含end)内的字符作为字符串s的初值   |
|        `    s.~string()   `        |                销毁所有字符，释放内存                 |
|       `    =,assign()     `        |                       赋以新值                        |
|          `  swap()      `          |                 交换两个字符串的内容                  |
|  `    +=,append(),push_back()   `  |                    在尾部添加字符                     |
|         `  insert()     `          |                       插入字符                        |
|          `   erase()   `           |                       删除字符                        |
|         `    clear()    `          |                     删除全部字符                      |
|        `    replace()    `         |                       替换字符                        |
|             `  +     `             |                      串联字符串                       |
|  `   ==,!=,>= ,=,compare()     `   |                      比较字符串                       |
|     `   size(),length()     `      |                     返回字符数量                      |
|         `  max_size()    `         |                返回字符的可能最大个数                 |
|         `  empty()      `          | 判断字符串是否为空，是空时返回ture，不是空时返回false |
|        ` capacity()      `         |              返回重新分配之前的字符容量               |
|        `    reserve()    `         |          保留一定量内存以容纳一定数量的字符           |
|         `   [ ], at()    `         |                     存取单一字符                      |
|       `    >>,getline()   `        |                   从stream读取某值                    |
|             `  <<    `             |                   将谋值写入stream                    |
|           `   copy()   `           |               将某值赋值为一个C_string                |
|          `   c_str()    `          |                 将内容以C_string返回                  |
|          `    data()   `           |               将内容以字符数组形式返回                |
|         `    substr()   `          |                   返回某个子字符串                    |
|       `  begin() end()    `        |                提供类似STL的迭代器支持                |
|      ` rbegin() rend()     `       |                      逆向迭代器                       |
|    `  y) get_allocator()     `     |                      返回配置器                       |

相对的，字符数组库：

|      函数       |           作用            |
| :-------------: | :-----------------------: |
| `   strlen   `  |       求字符串长度        |
| `  strcmp    `  |   比较2个字符串是否一样   |
|  `   strcat  `  |      字符串连接操作       |
| `   strcpy   `  |      字符串拷贝操作       |
| ` strncat     ` | 字符串连接操作(前n个字符) |
| `   strncpy   ` | 字符串拷贝操作(前n个字符) |
| `    strchr   ` |         查询字串          |
| `  strstr    `  |         查询子串          |
| `  strrev    `  |        反转字符串         |

<br/>

### 0x0d 动态二维数组的申请

#### 7.1 申请

好处不由分说，传递指针比传递整个对象更方便高效；随时确定存储大小，避免内存浪费。C++建立动态二维数组主要有两种方法：

1. 多维动态数组的申请是由高级指针+for循环实现，先依次声明维度的大小，然后从最低维度开始申请内存，其内存释放则是由内到外不断释放的。使用数组指针，分配一个指针数组，将其首地址保存在b中，然后再为指针数组的每个元素分配一个数组。

   ```cpp
   int **b=new int*[row];//分配一个指针数组，将其首地址保存在b中
   for(i=0;i<row;i++) b[i]=new int[col];//为指针数组的每个元素分配一个数组。
   ```

2. 利用`vector`.

   ```cpp
   vector<vector<int> > a(row,vector<int>(column));
   ```

   注意：在当函数的数组定义功能被不断的函数调用而不进行内存的释放的时候，就会导致软件奔溃。

<br/>

#### 7.2 释放

释放动态数组时，使用`delete[]Dynamic_Arr4;`即在数组名前加上一个中括弧。这个时候的中括弧，或者说是指向数组的指针时，空括号是必须的。它告诉编译器，指针指向一个数组的第一个元素。需先释放指针数组的每个元素指向的数组，然后再释放该指针数组：

```cpp
for(i=0;i<row;i++){
   delete []b[i];
   b[i]=NULL;
}
delete []b;
b=NULL;
```

delete释放数组是逆序进行的，最后一个元素被最先释放，第一个元素最后一个被释放。

```cpp
 for (int i = 0; i < MAX_NUM; i++){
     for (int j = 0; j < ROW_NUM; j++){
         delete[] Arr3D[i][j];
     }
     delete[] Arr3D[i];
 }
delete[] Arr3D;
```

<br/>

#### 7.3 内存使用的问题

```cpp
A a;//直接将a放入栈区(局部变量，大小受限，自动释放)； 
A * a = new A();//在堆区(动态内存，大小任意，手动释放)分配一块内存，然后用指针a去指向
```

<br/>

#### 7.4 动态申请的显示初始化

```cpp
string* Dynamic_Arr4 = new string[size]{"aa", "bb","cc", "dd", string(2, 'e') };      
```

**显式初始化可能会因为编译器版本原因报错，笔者在VS2013与VS2017下进行测试，VS2013下在释放内存时出现内存错误。**

测试代码：

```cpp
void not_delete_fun(){
	int* arr_test = new int[10];
	cout << arr_test << endl;
	//delete[] arr_test;
}
void delete_fun(){
	int* arr_test = new int[10];
	cout << arr_test << endl;
	delete[] arr_test;
}
int main(){
	int* Dynamic_Arr1 = NULL;
	Dynamic_Arr1 = new int[10];
	int size = 10;
	int* Dynamic_Arr2 = new int[size];       //未初始化;
	int* Dynamic_Arr3 = new int[size] {10, 9, 2, 8, 3, 6, 4};     //默认的初始化；
	string a = "ee";
	string* Dynamic_Arr4 = new string[size]{ "aa", "bb", "cc", "dd", string(1, 'e'), "aa", "bb", "cc", "dd", string(2, 'e') };      //显式的初始化
	//string *Dynamic_Arr4 = new string[size];
	//cout << Dynamic_Arr4[4] << endl;
	cout << "未释放内存的数组位置" << endl;
	for (int i = 0; i < 3; i++){
		not_delete_fun();
	}
	cout << "释放过内存的数组位置" << endl;
	for (int i = 0; i < 3; i++){
		delete_fun();
	}
}
```

多维数组的存储和释放:

```cpp
int main() {
	int MAX_NUM = 10;
	int COL_NUM = 5, ROW_NUM = 3;
	double*** Arr3D = new double** [MAX_NUM];
	for (int i = 0; i < MAX_NUM; i++){
		Arr3D[i] = new double* [ROW_NUM];
		for (int j = 0; j < ROW_NUM; j++){
			Arr3D[i][j] = new double[COL_NUM];
		}
	}
	//cout << Arr3D[9][4][2] << endl;
	for (int i = 0; i < MAX_NUM; i++){
		for (int j = 0; j < ROW_NUM; j++){
			delete[] Arr3D[i][j];
		}
		delete[] Arr3D[i];
	}
	delete[] Arr3D;
	delete[] Dynamic_Arr1;
	delete[] Dynamic_Arr2;
	delete[] Dynamic_Arr3;
	delete[] Dynamic_Arr4;
	system("pause");
	return 0;
}
```



### 0x0e 统计字符
```c++
int main() {
	int sz, zm, kg, qt;
	sz = zm = kg = qt = 0;
	char p[1000];
	cin.getline(p, 1000);
	for (int i = 0; p[i] != '\0'; i++) {
		if (p[i] >= 'A' && p[i] <= 'Z' || p[i] >= 'a' && p[i] <= 'z') zm++;
		else if (p[i] >= '0' && p[i] <= '9') sz++;
		else if (p[i] == ' ') kg++;
		else qt++;
	}
	cout << zm << " " << sz << " " << kg << " " << qt;
}
```

升级版
#### 统计一个段落单词数量

```c++
#include <iostream>
#include <string>
#include <algorithm>
using namespace std;
int main() {
	string a, b, temp;
	cin >> a;
	cin.get();
	getline(cin, b);
	a.insert(a.begin(), ' ');
	b.insert(0, " ");
	transform(a.begin(), a.end(), a.begin(), ::toupper);
	transform(b.begin(), b.end(), b.begin(), ::toupper);
	int step = 0, m = 0, n = 0;
	m = b.find(a);
	int k = m;
	if (m == -1) {
		cout << "-1";
		return 0;
	}
	else {
		while (b.find(a, n) != -1) {
			step++;
			n = b.find(a, n) + 1;
		}
	}
	cout << step << " " << k;
}
```

###  0x0f 全排列 

```c++
int main(){
	int a[4];
	for (int i = 0; i < 4; i++) cin >> a[i];
	for (int i = 3; i >= 0; i--)
		for (int j = 0; j <= 3; j++)
			if (i != j)
				for (int k = 0; k <= 3; k++)
					if (k != i && k != j)
						for (int t = 0; t <= 3; t++)
							if (t != k && t != i && t != j)
								cout << a[j] << " " << a[k] << " " << a[t] << endl;
}
```

截取字符串

```c++
int main() {
	int N;
	cin >> N;
	cin.get();
	for (int i = 0; i < N; i++) {
		int step = 0;
		char p[1000];
		cin.getline(p, 1000);
		bool m = false;
		for (int i = 0; p[i] != '\0'; i++) {
			if (p[i] <= 'z' && p[i] >= 'a') m = true;
			else break;
			if (m) step++;
		}
		cout << step << endl;
	}
}
```

</br>

</br>



### 0x10 回文数 [·](https://baike.baidu.com/item/%E5%9B%9E%E6%96%87%E6%95%B0/1830170?fr=aladdin#5_3)

回文数的处理有两种：一种是全都转化为字符进行处理；一种是全都转化为数字处理。虽然说两者是等效的，但是方法不同，代码如下

其一：

```c++
int palindromeint(int a) {
	int k = a, sum = 0;
	while (a) {
		sum *= 10;
		sum += a % 10;
		a /= 10;
	}
	if (k == sum)return 1;
	else return 0;
}
```

其二：主函数用getline将字符数组取回来

```c++
#include<cstring>
int palindromechar(char p[]) {
	int i, len = strlen(p);
	for (i = 0; i < len / 2 + 1; i++)if (p[i] != p[len - i - 1]) return 0;
	return 1;
}
```

但是字符串就又是不一样的画风了，第一种方法（倒序）的话的方法就显得效率很低了···这里不再赘写。

</br>


### 0x11 递归

</br>

#### 4.3.1 阶乘

```c++
int lfh(int a) {
	if (a == 1)return 2;
	if (a > 1)return lfh(a - 1) * 2;
}
```

</br>

#### 4.3.2 多次输入输出

```c++
void lfh(int a) {
	cout << "ABC\n";
	if (a > 1) return lfh(a - 1);
}
```

</br>

#### 4.3.3 从1打印

```c++
int a;
int lfh(int m) {
	if (m <= a) {
		cout << m << " ";
		return lfh(m + 1);
	}
}
int main() {
	int m = 1;
	cin >> a;
	lfh(m);
}
```

</br>


### 0x12 栈

当我学习了栈之后写逆序的题我不太想用循环写了,手动滑稽

```c++
char p[1000];
cin.getline(p, 1000);
stack<char>m;
for (int i = 0; p[i] != '\0'; i++) m.push(p[i]);
while (!m.empty()) cout << m.top(), m.pop();
```
（回去重做字符替换1637，1685）（字符逆序用栈可以过去 为什么数组不可以？？？）

</br>


### 0x13 蛇矩

下面是从算法书里面找到的方法：利用每行的横纵坐标相加的和是定值来确定书写的格式

```c++
int p[50][50];
int main() {
	int N, i, j, y, k, step1 = 0, step2 = 0;
	cin >> N;
	k = N;
	for (i = 0; i < k; i++) {
		for (j = 0; j < k; j++)
			for (y = 0; y < k; y++) {
				if (i % 2) {
					if (y + j == i) p[j][y] = N--;
					if (N == 0) goto L;
				}
				else {
					if (y + j == i) p[y][j] = N--;
					if (N == 0) goto L;
				}
			}
	}
L: for (j = 0; p[0][j] != 0; j++)step1++;
	for (j = 0; p[j][0] != 0; j++)step2++;
	for (i = 0; ; i++) {
		for (j = 0; j < step2; j++) {
			for (y = 0; y < step1 - i; y++)
				if (p[j][y] != 0) cout << p[j][y] << " ";
			cout << endl;
		}
		return 0;
	}
}
```

发现自己现在还是爱用goto，期末考完试再优化一下自己写的代码吧······（用外层的break可以出去）

还有另外一种方法不用开数组直接暴力用`for`模拟，[参考题目](https://www.luogu.com.cn/problem/P1014)：

```cpp
#define _for(i,a,b) for(int i=(a);i<(b);++i)
int main() {
	int N, ii, i_x, i_y;
	cin >> N;
	for (ii = 1; (ii * (ii + 1)) / 2 < N; ii++);//ii行
	N -= ((ii - 1) * ii) / 2;
	if (ii % 2) {
		_for(i, 0, ii) {
			if ((N--) == 0)break;
			i_y = i;
			i_x = ii - 1 - i_y;
		}
	}
	else {
		_for(i, 0, ii) {
			if ((N--) == 0)break;			
			i_x = i;
			i_y = ii - 1 - i_x;
		}
	}
	cout << i_x + 1 << "/" << i_y + 1;
}
```



</br>

### 0x14 最长上升自序列(LIS算法)

```c++
char str[10001];
int main() {
	int T, i, j, len, ans;
	cin >> T;
	cin.get();
	while (T--) {
		cin.getline(str, 10001);
		len = strlen(str);
		ans = 0;
		for (i = 0; i < len; i++) {
			for (j = 0; j < ans; j++) {
				if (str[j] >= str[i]) {
					str[j] = str[i];
					break;
				}
			}
			if (j == ans) {
				str[j] = str[i];
				ans++;
			}
		}
		cout << ans << endl;
	}
}
/*
abklmnnopqrstcdefg 2
abklmnopqrstcdefg
abcdefgpqrstcdefg
abcdefgpqrsttcdefg
*/
```

补充：字符串的修改（

### 0x15 动态规划

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
int p[20][20];
int main() {
	char p1[20];
	char p2[20];
	cin.getline(p1, 20);
	cin.getline(p2, 20);
	int len1 = strlen(p1);
	int len2 = strlen(p2);
	for (int i = 0; i < len1; i++)p[0][i] = i;
	for (int i = 0; i < len2; i++)p[i][0] = i;
	for (int j = 0; j < len2; j++) {
		for (int i = 0; i < len1; i++) {
			if (j != 0 && i != 0) p[j][i] = min(min(p[j - 1][i] + 1, p[j][i - 1] + 1), p[j - 1][i - 1] + ((p1[i - 1] == p2[j - 1]) ? 0 : 1));
		}//这里最好不要写成if(i&&j)要不然会有的地方赋不了值。（？？？？？？）
	}
	for (int j = 0; j < len2; j++) {
		for (int i = 0; i < len1; i++) cout << p[j][i] << " ";//注意行列应用在字符串先后中的应用
		cout << "\n";
	}
	cout << p[len2 - 1][len1 - 1];//注意不可以解决首尾字母相同的情况
}
```

</br>

### Aferword

转自系其他学生的曲子：😄（09/22 22:40）

```cpp
#include<iostream>
#include<windows.h>
using namespace std;
enum fy {
	d1 = 262, d1_ = 277, d2 = 294, d2_ = 311, d3 = 330, d4 = 349, d5 = 392, d5_ = 415,
	d6 = 440, d6_ = 466, d7 = 494, z1 = 523, z1_ = 554, z2 = 578, z2_ = 622, z3 = 659,
	z4 = 698, z4_ = 740, z5 = 784, z5_ = 831, z6 = 880, z6_ = 932, z7 = 988, g1 = 1046,
	g1_ = 1109, g2 = 1175, g2_ = 1245, g3 = 1318, g4 = 1397, g4_ = 1480,
	g5 = 1568, g5_ = 1661, g6 = 1760, g6_ = 1865, g7 = 1976, yaya = 0
};
struct yf {
	enum fy s;
	int t;
};
int main() {
	struct yf a[1000] = {
	{z6,50},{z7,50},{g1,150},{z7,50},{g1,100}, //5
	{g3,100},{z7,300},{z3,100},{z6,150},{z5,50}, //10
	{z6,100},{g1,100},{z5,300},{z2,50}, //14
	{z3,50},{z4,150},{z3,50},{z4,50},{g1,150}, //19
	{z3,150},{z2,50},{z3,50},{g1,150},{z7,150}, //24
	{z4_,50},{z4_,100},{z7,100},{z7,200},{z6,50}, //29
	{z7,50},{g1,150},{z7,50},{g1,100},{g3,100}, //34
	{z7,200},{z3,100},{z6,150}, //37
	{z5,50},{z6,100},{g1,100},{z5,300},{z3,100},{z4,100},{g1,50}, //44
	{z7,150},{g1,100},{g2,100}, //47
	{g3,50},{g1,150},{g1,50},{z7,50},{z6,100},{z7,100},{z5_,100}, //54
	{z6,300},{g1,50},{g2,50}, //57
	{g3,150},{g2,50},{g3,100},{g5,100},{g2,300},{z5,100}, //63
	{g1,150},{z7,50},{g1,100},{g3,100},{g3,300},{z6,50},{z7,50},{g1,150}, //71
	{z7,50},{g1,100},{g2,100},{g1,150},{z5,50}, //76
	{z5,200},{g4,100},{g3,100},{g2,100},{g1,100}, //81
	{g3,400},{yaya,50},{g3,50},{g6,200},{g5,100},{g5,100},{g3,50}, //88
	{g2,50},{g1,100},{yaya,50},{g1,50},{g2,100},{g1,50},{g2,100},{g5,100}, //96
	{g3,200},{yaya,50},{g3,50},{g6,200},{g5,200},{g3,50},{g2,50}, //103
	{g1,200},{yaya,50},{g1,50},{g2,100},{g1,50},{g2,100},{z7,100}, //110
	{z6,200},{yaya,100},{z6,50},{z7,50},{z6,500}
	};
	struct yf* atop;
	atop = a;
	int n = 194;
	while (n--) {
		Beep(atop->s, atop->t * 5);
		atop++;
	}
	return 0;
}
```

