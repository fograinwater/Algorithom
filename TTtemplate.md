# **头**

```c++
#include<bits/stdc++.h>
using namespace std;

typedef long long ll;
#define int long long
#define lowbit(x) ((x)&(-x))
#define IOS std::ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);

```



## typedef

**一.使用格式**

```c++
typedef x y;//将x取别名为y
```

**二.举例**

1.基本数据类型

```c++
typedef long long ll;
```

2.结构体

```c++
typedef struct edge
{
    int u,v,w;
}edge;

//等价于：先定义结构体，再使用typedef
struct edge{
    int u,v,w;
};
typedef struct edge edge;
```

**！注意：**
当结构体中有该结构体类型的指针时，该指针前面的类型声明只能为完整的结构体类型名；

或者先typedef还未定义的结构体，再进行结构体的定义（但不推荐），如下：

1）错误写法,在检测结构体内部的变量时，typedef的过程还没有执行

```c++
typedef struct node{
    int x;
    node* p;
}node;
```

2）推荐写法，标准写法（c++，在定义的时候就自动typedef成struct 后面的名字了)

```c++
struct student{
    int age;
    struct student* p;
};
student x;

//也可直接使用
struct student{
    int age;
    struct student* p;
}x;

//也可也再简化一点
typedef student{
    int age;
    struct student* p;
}stu;
stu x;
```



## **输入输出**

### **1. 快读**

**关于数据的快速读入和输出**

**cin ,cout**读取速度比较慢，可以通过下面的语句对其解绑，解绑后的速度与scanf、printf相当，解绑前耗时为scanf的3.5~4倍

```c
std::ios::sync_with_stdio(false); cin.tie(0);cout.tie(0); 
```

**注：**解绑之后，cin，cout和scanf，printf、getchar(快读函数）混用，否则有一定几率会发生输入输出和预想不一样的情况。因为cin，cout之所以效率低，是因为先把要输出的东西存入缓冲区，再输出，导致效率降低，而解绑语句可以来打消iostream的输入输出缓存，可以节省许多时间

read() 函数速度为cin 的5倍左右，当测试次数在10^6^级别，每次读入10^9^级别的数据时，使用自定义快读函数read()，同时头文件不能包含<iostream>，否则可能会读入数据超时，出现不能输入程序直接结束运行的情况。

```c++
int read()//可以尝试加入inline关键字防止重复调用消耗过多内存
{
    int x = 0, f = 1;
    char c = getchar();
    while (c < '0' || c > '9')//判断首位是否为负号
    {
        if (c == '-')
            f = -1;
        c = getchar();
    }
    while (c >= '0' && c <= '9') {
        x = (x << 1) + (x << 3) + (c ^ 48); //采用位运算具有更高的效率,但是一定要加括号
        c = getchar(); //可以读取多位数
    }
    return x = x * f;
}
//'0'~'9'ASCII码：48~57对应二进制数110000~111001
//因此c^48=c^110000 ===> 0^a=a,所得结果即为数字字符转换得到的对应的数字
//解答一直以来的疑问：在执行line4的"char c=getchar()"时，一下子可以输入多个字符，因此我们想要将字符转换为数字，在这个语句执行时就已经输入完毕了我们想要得到的哪个数字，后面的getchar()只是不断的在读取这个数的每一位，而不是输入一位按个回车再输入一位按个回车直到输入完毕，而且对于getchar()函数，在读取一个字符后，后面的回车符会放入缓冲区当中，接着等待读取
```



### **2. 遇到EOF时输入停止**

```c++
//1.cin	（遇到EOF时自动停止）
int n;
while(cin >> n){
	
}	
//当使用cin输入时，可以直接while(cin >> n),因为没有读取结束标志，（cin>>a>>b>>c）就会返回true;
//当读取结束标志则返回false；另外cin不会读取空格和回车

//2.scanf （加判断条件）
char a,b,c;
while(scanf("%c%c%c",&a,&b,&c) != EOF){//这里数值之间不可以输入空格，因为空格也会被读取
　　　　　　　　　　　　　　　　　 			//如果需要中间输入空格的话，可以scanf("%c %c %c",&a,&b,&c);
    
}

//在windows上输入EOF的方法为Ctrl+Z,其它平台上输入EOF的方法为Ctrl+D
```



## **数据类型 数据范围**

2^7 = 128

2^15 = 32768 

2^31 = 2,147,483,648 = 2 e 9

2^63 = 9,223,372,036,854,775,808 = 9 e 18



## 常见返回值错误

3221225620 (0xC0000094): 除0错误，一般发生在整型数据除了0的时候

3221225477 (0xC0000005): 访问越界，一般是读或写了野指针指向的内存

3221225725 (0xC00000FD): 堆栈溢出，一般是无穷递归造成的；数组定义的容量太大 - 五十万起步的样子
而且每次循环都会再定义一次，导致缓存区溢出---->办法：数组定义在主函数外面 作为全局变量



## **ASCII码**

大写字母：65-90

小写字母：97-122

数字字符：48-57

记忆化搜索（递归）：使用哈希表/字典，来保存计算的中间结果，以空间换时间；

```c
//判断a是否为奇数
1.if(a % 2)
2.if(a & 1) a与1进行与运算，因为1的二进制位前面都是0，所以a与1进行与运算相当于是把a的二进制最低位与1进行与运算，如果a为奇数，则二进制最低位为1，结果也为1，反之则为0
```



## **sizeof()与lenth()    ,c++  size()的区别**

sizeof()是计算所占内存字节数的运算符，而不是函数，只能计算静态分配的内存空间的大小！因为sizeof在编译的时候就已经完成了计算，所以不能够计算动态分配的内存的大小

lenth() ，size()是函数，计算的是字符串的长度，可以计算string,vector类型，strlen()可以用于字符指针

```c++
sizeof(char) = 1;
sizeof("a") = 2;
sizeof('a') = 4;//!!! 	在c语言中，字符常量是int型，字符变量为char型
strlen("a") = 1;
```



## **取模运算**

**用乘除法和减法实现整数取模运算：**

a % b，整数除法下等价于a − a / b × b

```c++
加：(a + b) % mod = ((a % mod) + (b % mod)) % mod
减：(a - b) % mod = ((a % mod) - (b % mod) + mod) % mod
乘：(a * b) % mod = ((a % mod) * (b % mod)) % mod
除：(a / b) % mod = ((a % mod) * (inv(b) % mod)) % mod
```



# 二分法

**二分查找：**lowerbound(), upperbound() 返回的是地址，一般要减去首地址

将区间[l,r]划分为[l,mid]和[mid+1,r]时，其更新操作是r=mid或者l=mid+1;计算mid时不用加一。

```c++
int l=-1,r=n;
while(l+1!=r)
{
	mid = (l+r)>>1;
	if(n[mid] < q[i])//if(isblue(m))
		l = mid;
	else
		r = mid;
	return l;//or r;
}
```



# **离散化**

在不改变数据相对大小的前提下，对数据进行缩小

```c++
ll a[N];//原数组
ll aa[N];//离散化后的数组
//法一：
for(int i=1;i<=n;i++)
{
    cin >> a[i];
    aa[i]=a[i];
}
sort(aa+1,aa+n+1);
int cnt=unique(aa+1,aa+n+1)-1-aa;//去重之后的元素的个数
for(int i=1;i<=n;i++)
	aa[i]=lower_bound(aa+1,aa+cnt+1,a[i])-aa;//这种方法--->相等的元素离散化后的值是一样的

//法二：
struct node{
    ll data;
    int id;
    bool operator < (const node & b) const {
        return data < b.data;
    }
}a[N];
ll aa[N];
for(int i=1;i<=n;i++)
{
    cin >> a[i].data;
    aa[i]=a[i].data;
}
sort(a+1,a+n+1);
for(int i=1;i<=n;i++)
    aa[a[i].id]=i;
//排在最前面的那个元素最小，找到之前在原序列中的位置--->a[i].id，并让aa[a[i].id]=1,表示这个元素最小
//这种方法--->相等的元素离散化之后的值是不同的
```



# **逆序对**

两种求法：
1.归并排序 	2.树状数组

(注意答案可能爆long long！)

## **1.归并排序**

（可用于序列元素相同的情况）

**思路**：因为归并排序合并的过程实际上就是判断是否为逆序对的过程，因此可以通过在比较的过程中加计数，即可以实现逆序对的数量计算

```c++
// Created by TTT on 2022/11/24.
//给定一个长度为n的序列，其中n个数各不相同，求逆序对的数量（key:各不相同）
#include    <bits/stdc++.h>
using namespace std;
const int N = 1e5 + 3;
typedef long long ll;

ll a[N];//原数组
ll temp[N];//归并过程中的辅助数组
ll ans = 0;//逆序对的数量

void merge(ll l, ll mid, ll r) {
    ll i = l, j = mid + 1, k = l;//i:左区间起点，j:右半区间起点，k:暂存归并结果的辅助数组的区间起点
    while (i <= mid && j <= r) {
        if (a[i] <= a[j]) {
            temp[k++] = a[i++];
        } else {
            ans += (mid - i + 1);
            temp[k++] = a[j++];
        }
    }

    while (i <= mid) {
        temp[k++] = a[i++];
    }
    while (j <= r) {
        temp[k++] = a[j++];
    }
    for (ll i = l; i <= r; i++) {
        a[i] = temp[i];
    }
}

void Merge_sort(ll l, ll r) {
    if (l < r) {
        ll mid = (l + r) >> 1;
        Merge_sort(l, mid); //递归
        Merge_sort(mid + 1, r);
        merge(l, mid, r);//合并
    }
}

ll Count_inversion1() {
    //初始化原数组
    ll n;
    cin >> n;
    for (ll i = 1; i <= n; i++) {
        cin >> a[i];
    }

    //归并排序计算逆序对数量
    Merge_sort(1, n);
    return ans;
}

int main() {
    ll res = Count_inversion();
    cout << res << endl;
    return 0;
}
```



## **2.树状数组**

法一：

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long int ll;
#define double long double
#define lowbit(i) ((i)&(-i))
#define IOS std::ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
const int N=5e5+10;
int n,a[N],c[N],num,tr[N];

void update(int i,int x)
{
	while(i<=n)
	{
		tr[i]+=x;
		i+=lowbit(i);
	}
}
ll query(int x)
{
	ll sum=0;
	while(x>=1)
	{
		sum+=tr[x];
		x-=lowbit(x);
	}
	return sum;
}
int main()
{
    IOS
    //初始化原数组
    cin>>n;
    for(int i=1;i<=n;++i)
    {
        cin>>a[i];
        c[++num]=a[i];
    }
    //离散化
    sort(c+1,c+num+1);
    num=unique(c+1,c+num+1)-c-1;
    for(int i=1;i<=n;++i)
        a[i]=lower_bound(c+1,c+num+1,a[i])-c;
    
    ll sum=0;
    for(int i=1;i<=n;++i)
    {
        sum+=query(n)-query(a[i]);
        update(a[i],1);
    }
    cout<<sum<<endl;
    
    return 0;
}
```

法二：

```c++
//逆序对数 = 总对数 - 正序对的数量（相等对，顺序对）
#include    <bits/stdc++.h>

typedef long long ll;
#define lowbit(x) ((x) & (-x))
using namespace std;
const int N = 5e5 + 7;
ll b[N];//存储离散化后的等效数组
ll tr[N];//树状数组
ll ans = 0;

//原数组，含id
struct node {
    ll val;
    ll id;

    bool operator<(const node &p) const {
        if (val != p.val)
            return val < p.val;
        else {
            return id < p.id;
        }
    }
} a[N];

void update(ll x, ll k) {
    while (x < N) {
        tr[x] += k;
        x += lowbit(x);
    }
}

ll query(ll x) {
    ll res = 0;
    while (x) {
        res += tr[x];
        x -= lowbit(x);
    }
    return res;
}

//树状数组计算逆序对数量
ll Count_inversion2() {
    //初始化原数组
    ll n;
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> a[i].val;
        a[i].id = i;
    }

    //离散化
    sort(a + 1, a + n + 1);
    for (int i = 1; i <= n; i++) {
        b[a[i].id] = i;
    }

    for (int i = 1; i <= n; i++) {
        ans += query(b[i]);
        update(b[i], 1);
        //ans += (i - query(b[i]));
    }
    ans = n * (n - 1) / 2 - ans;
    cout << ans << endl;
    return ans;
}

int main() {
    Count_inversion2();
    return 0;
}
```

## 3.权值线段树

```c++
#include    <bits/stdc++.h>

using namespace std;
#define int long long
#define endl '\n'
#define ls (pos << 1)
#define rs (pos << 1 | 1)
const int INF = 0x3f3f3f3f3f3f3f3f;
const int N = 5e5 + 3;
const int M = 1e4 + 5;
int tr[N << 2];//tr数组的大小是由元素最大值决定的
int maxn = -INF;//代表所有元素的最大值，用区间上的某个点代表某个元素值
int b[N];//离散化后的数组

struct node {
    int val, id;
};
node a[N];//原数组

bool cmp(node p, node q) {
    if (p.val != q.val) return p.val < q.val;
    return p.id < q.id;
}

void pushup(int pos) {
    tr[pos] = tr[ls] + tr[rs];
}

void update(int pos, int l, int r, int val, int cnt) {//将元素val的个数增加/减少cnt个
    if (l == r) {
        tr[pos] += cnt;
        return;
    }
    int mid = (l + r) >> 1;
    if (val <= mid) update(ls, l, mid, val, cnt);
    else update(rs, mid + 1, r, val, cnt);
    pushup(pos);
}

int queryLR(int pos, int l, int r, int s, int e) {//查询值为[s,e]在区间[l,r]的元素出现次数
    if (e < l || s > r) return 0;
    if (s <= l && r <= e) return tr[pos];//当前区间包含在所查询的区间内

    int sum = 0;
    int mid = (l + r) >> 1;
    if (s <= mid) sum += queryLR(ls, l, mid, s, e);
    if (e > mid) sum += queryLR(rs, mid + 1, r, s, e);
    pushup(pos);

    return sum;
}

signed main() {
    int n;
    cin >> n;
    //离散化
    for (int i = 1; i <= n; i++) {
        cin >> a[i].val;
        a[i].id = i;
    }
    sort(a + 1, a + n + 1, cmp);
    for (int i = 1; i <= n; i++) {
        b[a[i].id] = i;
    }

    int ans = 0;
    for (int i = 1; i <= n; i++) {
        ans += queryLR(1, 1, n, b[i] + 1, n);
        update(1, 1, n, b[i], 1);
        //依次将离散化的数组中的元素b[i]加入到权值线段树中
        //并在区间[1,n]中查询[b[i] + 1, n]的元素的个数
        //相当于计算每一个元素前面的大于它的元素的个数
    }
    cout << ans << endl;

    return 0;
}
```



# 素数筛

## 埃氏筛

**作用：**找出一定范围内所有的质数

**原理：**任何数都被可以分解为若干质因数的乘积

**时间复杂度：**近似为O ( n )

几个难点：

为什么从i * i开始筛：

我们会先筛2的所有倍数，然后筛3的所有倍数……但是可以发现，在筛除3的倍数时，如果我们还是从3的2倍开始筛，其实3 * 2 已经被2 * 3时筛过了。
又比如说筛5的倍数时，我们从5的2倍开始筛，但是发现5 * 2会先被2 * 5筛去， 5 * 3会先被3 * 5会筛去，5 * 4会先被2 * 10筛去，所以我们每一次只需要从i * i开始筛，因为(2，3,…,i - 1)倍已经被筛过了。

```c++
const int N = 1e7;
ll prime[N];
bool nprime[N];
//prime[i]表示1~n内的第i个素数
//nprime[i]：代表i是否不是素数，0->是素数，1->不是素数。初始都为0，假设最开始都是素数，在筛的过程中解除素数
//函数作用：得到1~n范围内的所有质数，并将其存储在prime数组当中，返回值为素数的个数cnt
//估算：当n为5e5时，有约5133个
int E_sieve(ll n)
{
    int cnt = 0;    
    nprime[1] = 1;
    for(ll i = 2; i * i <= n; i++)//i代表从1~n的每个数字
    {
        if(nprime[i] == 0)//如果i是素数
        {
            for(int j = i * i; j <= n; j += i)
            {
                nprime[j] = 1;//倍数不是质数
            }
        }
    }
    //因为上面优化了计算次数，所以有一部分数没有计算进去，需要单独计算素数的总个数cnt
    for(ll i = 2; i <= n; i++)
    {
        if(nprime[i] == 0)
        {
            prime[++cnt] = i;
        }
    }
    return cnt;
}
```



## 欧拉线性筛

**时间复杂度：**O ( n )

每个合数都可以被分解成若干个素数的乘积，那么我们在筛掉合数时， 只需保证这个数被其能分解成的最小素数筛掉，如 45  可以分解成 3 * 3 * 5， 我们要让45被3 * 15筛掉， 而不是被5 * 9筛掉。

**优点：**确保每个合数只被最小质因数筛掉。或者说是被合数的最大因子筛掉。（上例中3为最小质因子，15为“最大因子”）

```c++
const int N = 1e5 + 3;
const int M = 1e7 + 3;
int prime[N];//存放1~n内所有的素数
bool nprime[M];//nprime[i]：i是否不是为素数（根据前面的质数、合数推得），为0代表是素数（这样就不用自己去初始化了）

int primes(int n)
{
    int cnt = 0;//素数个数
    for(int i = 2; i <= n; i++)
    {
        if(nprime[i] == 0)//如果该数为素数，则将其存入prime数组中
            prime[++cnt] = i;
        for(int j = 1; j <= cnt && i * prime[j] <= n; j++)//当前质数与前面得到的所有质数的乘积
        {
            nprime[i * prime[j]] = 1;//将其倍数标记为合数
            if(i % prime[j] == 0)//保证每个合数只被最小质因子筛到
                break;
        }
    }
    return cnt;
}
```

```c++
//计算从1~n内所有的素数
vector<int> p;//存放素数
for(int i = 2; i * i <= n; i++)
{
    if (n % i == 0)
    {
        p.push_back(i);
        while(n % i == 0) 
            n /= i;
    }
}
if (n > 1) p.push_back(n);
```



### 应用：求1~n中与m互质的数的个数

**思路：**

1.先求1~n中与m有公因子的数的个数cnt，则与m互质的数的个数为n - cnt

2.那么如何求1~n中与m有公因子的数的个数：

可以先用素数筛把m分解质因数，然后利用容斥原理计算1 ~ n中m的不同质因子的组合的组数。

```c++
const int N = 1e5 + 3;
const int M = 1e7 + 3;
int prime[N];
bool nprime[M];

int getCoprimes(int n, int m)
{
    int cnt = 0;//m的质因子的个数
    for(int i = 2; i <= m; i++)
    {
        if(nprime[i] == 0)
            prime[++cnt] = i;
        for(int j = 1; j <= cnt && i * prime[j] <= n; j++)
        {
            nprime[i * prime[j]] = 1;
            if(i % prime[j] == 0)
                break;
        }
    }

    int _ans = 0;
    for(int i = 1; i < (1 << cnt); i++)//用二进制数代表取质因数的状态
    {
        int t = 0;//返回二进制下数i中1的个数
        ll temp = 1;//存储质因数组合的乘积
        for(int j = 1; j <= cnt; j++)
        {
            if(i << (j - 1) & 1)//如果i的第j位为1，则取到了第j个质因子，则乘积需要乘上prime[j]
            {
                temp *= prime[j];
                t++;
            }
        }
        _ans += (t & 1) ? (n / temp) : (- n / temp);//奇加偶减（n代表选出的因子的个数，如当n = 1时，应该为加
    }
    int ans = n - _ans;
}
```



# 排序

## **1. 冒泡排序**

```c
for(int i = 0;i < n - 1 ;i++)
{
    for(int j = 0; j < n - 1 - i; j++)
        if(a[j] > a[j+1])
        {
            swap();
        }
}
```



# 字符串操作

## KMP

```c++
#include    <bits/stdc++.h>

using namespace std;
const int N = 1e6 + 5;
int nxt[N], ans[N];

void get_next(string p, int len) {
    //初始化
    int j = 0;//前缀的末尾下标，最长公共前缀的值
    nxt[0] = 0;
    for (int i = 1; i < len; i++)//后缀的末尾下标值
    {
        while (j > 0 && p[j] != p[i])
            j = nxt[j - 1];//如果匹配不成功，就不断后退，直到匹配成功为止
        if (p[i] == p[j])//匹配成功则往后移一位，继续比较，长度加一
            j++;
        nxt[i] = j;//更新next数组
    }
}

int main() {
    string s1, s2;
    cin >> s1;//主串
    cin >> s2;//模式串
    int len1 = s1.size();
    int len2 = s2.size();

    get_next(s2, len2);

    int j = 0;
    for (int i = 0; i < len1; i++) {
        while (j > 0 && s2[j] != s1[i])
            j = nxt[j - 1];
        if (s2[j] == s1[i])
            j++;
        if (j == len2)
            cout << i - len2 + 2 << endl;//由于下标，所以+2
    }
    for (int i = 0; i < len2; i++)
        cout << nxt[i] << " ";
    return 0;
}
```

使用str.size()时进行强制类型转换

```
(int)str.size();
```



## Hash

### 字符串hash

```c++
//哈希构造

//单哈希

//1）字符串
string a="abc";
a=' '+a;
n=a.size();//因为后面要使用下标，所以最好添加空字符之后再计算size

//2）设置进制基数和mod
int base=223;
ll mod=1e9+7;

//3）预处理base的i次方====> p[i]=base^i;
p[0]=1;
for(int i=1;i<=n;i++)
    p[i]=p[i-1]*base;

//scanf("%s",a+1);
//3）转换
hash[0]=0;
for(int i=1;i<=n;i++)
{
    hush[i]=(hash[i-1]*base+a[i]-'a'+1)%mod;
    //针对全小写英文字母可以a[i]-'a'+1处理，但是如果含有多个字符直接用ASCII码即可
}

//字符串a的hash值为hash[n]
//4）查询子串[l,r]
ll ans=( (hush[r]-hash[l-1]*p[r-l+1]) % mod + mod ) % mod;

//双哈希，设置两个base和mod,hash两次，将字符串表示成一个二元组<a1,a2>
hash1[i]=hash1[i-1]*base+a[i]-'a'+1;
hash2[i]=hash2[i-1]*base+a[i]-'a'+1;

//素数的选择：
1610612741	(2^30~2^31)

```



**题目**：给定字符串，求有多少个子串可以写成某个字符串与其自身相连接的形式，如"abcabc"=abc+abc

```c++
#include    <bits/stdc++.h>

using namespace std;
typedef long long ll;
const int N = 2e3 + 10;

int main() {
    int base = 29;
    ll mod = 1e9 + 7;
    string a;
    cin >> a;
    a = ' ' + a;
    int n = a.size();
    vector<ll> hash(n);
    vector<ll> p(n);

    p[0] = 1;//初始化
    for (int i = 1; i <= n; i++)
        p[i] = (p[i - 1] * base) % mod;
    hash[0] = 0;
    for (int i = 1; i <= n; i++) {
        hash[i] = (hash[i - 1] * base + a[i] - 'a' + 1) % mod;
    }

    unordered_set<ll> q;//因为求出来的子串可能重复，因此采用unordered_set容器，该容器插入元素时会先查看容器内是否有相同元素，如果有，就不再重复插入，即容器中所有元素都是唯一的，所以可以用来计算不重复的元素出现的总次数
    for (int i = 2; i <= n; i += 2)//长度
    {
        for (int j = 1; j <= n - i + 1; j++)//起点
        {
            int x1 = j - 1, y1 = j + i / 2 - 1;
            int x2 = j + i / 2 - 1, y2 = j + i - 1;
            ll l = ((hash[y1] - hash[x1] * p[y1 - x1]) % mod + mod) % mod;
            ll r = ((hash[y2] - hash[x2] * p[y2 - x2]) % mod + mod) % mod;
            if (l == r)
                q.insert(l);
        }
    }
    cout << q.size() << endl;
    return 0;
}
/*
abcabcabc
leetcodeleetcode
*/
```





# DP动态规划

## 1.线性DP

### 最长上升(不下降)子序列

#### O(n*n)

```c++
#include    <iostream>
using namespace std;
const int N = 1000 + 10;
int a[N], dp[N];

int main() {
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++)
        cin >> a[i];
    
    //dp[i]为以a[i]结尾的最长子序列的长度
    int ans = 1;
    for (int i = 1; i <= n; i++)//对于每一个a[i]，循环比较该数与它前面的数a[j]
    {
        dp[i] = 1;//如果出现a[i]小于前面所有的数的情况，则以a[i]结尾的最长子序列的长度为1，所以dp数组应初始化为1
        for (int j = 1; j < i; j++) {
            if (a[i] > a[j])//如果a[i]>a[j]说明a[i]可以接在a[j]的后面
                dp[i] = max(dp[i], dp[j] + 1);//使用max是为了更新a[i]接到从a[1]~a[j]时不同长度的最大值
            /*
            if(a[i] >= a[j])	//最长不下降子序列
            */
        }
        ans = max(dp[i], ans);//使用max是为了得到结尾为不同a[i]时的最长子序列的最大值
    }
    cout << ans << endl;
    return 0;
}
```

#### O(n*logn)

```c++
int a[N];
int LIS(int a[])
{
    vector<int> dp(N,INF);//dp数组保存取出来的上升子序列，初始化为INF，便于插入操作
    for(int i = 1;i <= n;i++)
    {
        auto it = lower_bound(dp.begin(),dp.end(),a[i]);//第一个大于等于a[i]的位置
        *it = a[i];//将a[i]插入到合适的位置==>可以保持上升子序列的位置
    }
    return lower_bound(dp.begin(),dp.end(),INF) - dp.begin();
}
```



## 2.数位DP

```c
#include	<bits/stdc++.h>
#define IOS ios::sync__with__stdio(false); cin.tie(0); cout.tie(0);
using namespace std;
const int mod=1e9+7;
int s[25],dp[25][205];
dfs(int pos,int lim,int sum)
{
	if(pos==0) return sum % mod;//搜索到最深处，开始返回
	if(!lim && dp[pos][sum] != -1) return dp[pos][sum];//已经被计算过
	int ans=0;//没有被计算过
	int st = lim ? s[pos] : 9;
	for(int i=0;i<=st;i++)
	{
		ans+=dfs(pos-1,lim && (i==s[pos]),(sum+i)%mod);
        ans %= mod;
        //最开始的最高位，limit置1表示有限制
        //但是最高位上的数字也可以变化，i从0到st。当i<st,递归后面的pos-1位上的数字就没有限制,即当lim=0时，lim&&(i==s[pos])=0
        //因此除了最高位之外的递归函数中的limit代表的是 
        //lim（上一位的）&& (i==s[pos])或者 limit=lim（上一位的）&& (i<s[pos])
	}
	if(!lim)	dp[pos][sum]=ans;
	return ans;
}
int cal(int x)//拆分数位
{
	int len=0;
	while(x)
	{
		s[++len]=x%10;//将各位存到s数组当中
		x/=10;
	}
	return dfs(len,1,0);//最高位有限制，sum=0;
}

void solve()
{
	int t;
    memset(dp,-1,sizeof dp);
    //全部初始化为-1,以便于后面的状态判断，不初始化为0是因为可能返回的sum值就为0，导致一直递归下去导致无法得到结果
	while(t--)
	{
		int l,r;
		cin >> l >> r;
		cout << ((cal(r)%mod)-(cal(l)%mod)+mod) % mod <<endl;//减法取模
	}
	return 0;
}
int main()
{
    IOS
    solve();
    return 0;
}
```



# 背包问题（物品的个数有所不同）

## 1）01背包

题目：总共n件物品（物品各不相同，**只有一件**），每件物品的重量为w[i]，价值为v[i]，背包的最大承重量为m

问：如何在背包中放入物品，才能使背包中物品的总价值最大，并且不超过背包的最大承重量？

```c
//二维f[i][j]
#include	<bits/stdc++.h>
const int N=1e7+10;//最多物品数
int w[N],v[N];//如果只指出了w和v之中的一个，那么w[i]=v[i]
ll f[N][N];
int main()
{
    int n;
    cin >> n;
    for(int i=1;i<=n;i++)
        cin >> w[i] >> v[i];//注意w[i]与v[i]的输入顺序、、每次一起输入 or 先全部输w[i]再输v[i]
    for(int i=1;i<=n;i++)
    {
        for(int j=w[i];j<=m;j++)
            f[i][j]=max(f[i-1][j],f[i-1][j-w[i]]+v[i]);
    }
    cout << f[n][m];
    return 0;
}
```

```c
//一维
ll dp[N];
for(int i=1;i<=n;i++)
{
    for(int j=m;j>=w[i];j--)//区别！！倒序，防止dp[i-1][j]被更新
        dp[j]=max(dp[j],dp[j-w[i]]+v[i]);
}
cout << dp[m];
```

**注：**最终背包中不一定会有重量刚好为m的物品，因为可能出现背包没有装满但价值达到最大的情况

**反思：**

1.关于f数组的初始化问题：

因为物品价值都是正数，所以f数组可以全部初始化为0，这样在状态转移取较大值时，每次加入一件物品取到的较大值都是有效的；但是当物品价值可以为负时，我们需要将f初始化为负无穷，这样才不会使正确答案被0错误地更新

## 2）完全背包

每个物品都有**无限个**

```c
ll dp[N];
for(int i=1;i<=n;i++)
{
    for(int j=w[i];j<=m;j++)//不需要倒序
        dp[j]=max(dp[j], dp[j-w[i]]+v[i]);
}
cout << dp[m];
```



## 3）多重背包

每个物品的**数量不相同**但都**有限**，分别为s[i]

```c
int s[N];//数量
for (int i = 1; i <= n; i++) {
        int num = min(s[i], m / v[i]); //可选的最大数量：物品本身的个数和最大容量的情况下至多可放多少个物品i
        for (int k = 1; num > 0; k <<= 1) {
            //因为每一个数都能用2^k的和表示，所以可以用二进制将s[i]化为1+2+4+......多组
            //把每一组当成一个新的物体，此时重量和价值都变为原来的k倍
            if (k > num) k = num;
            num -= k;
            /*当k>num的时候，说明k已经等于min(s[i], m/v[i])的最高一位数了，
             * 此时，这个物品的件数应该等于min(s[i], m/v[i]) - k + 1，
             * 而此时的num等于 min(s[i], m/v[i]) - (k - 1)，就是我们需要的数字，所以把k改成num即可 */
            for (int j = m; j >= k * v[i]; j--)//将k个物体当作一个整体，化为0-1背包，倒序
                f[j] = max(f[j], f[j - k * v[i]] + k * w[i]);//针对这个新物体进行讨论，所以j的循环在k的内部
        }
    }
cout << dp[m];
```



## 4）混合背包

有的物品只有一个，有的有无限个，有的数量不同

```c
for(int i=1;i<=n;i++)
{
    if(s[i]==0)//用s[i]=0表示无限个
    {
        for(int j=0;j<=m;j++)
            dp[j]=max(dp[j],dp[j-w[i]]+v[i]);
    }
    else
    {
        int num= min(s[i],m/w[i]);
        for(int k=1;num>0;k<<=1)//多重背包和0-1背包可以视为0-1背包这一类
        {
            if(k>num)	k=num;
            num -= k;
            for(int j=m;j>=k*w[i];j--)//将k个物体当作一个整体，化为0-1背包，倒序
                dp[j]=max(dp[j],dp[j-k*w[i]]+k*v[i]);
        }
    }
}
cout << dp[m] << endl;
```



## 5）分组背包

给定几组物品n，以及背包最大容量m，只能从每组物品中选择一个物品或是不选，求最大价值

思路：将每组物品用0-1背包的思路考虑，在考虑每组物品的时候对里面的每个物品进行遍历从而选出最优方案

```c
int n,m;
cin >> n >> m;
//input
for(int i=1;i<=n;i++)
{
    cin >> s[i];//第i组的物品数量
    for(int j=1;j<=s[i];j++)
        cin >> w[i][j] >> v[i][j];//第i组每个物品的重量和价值
}
//cal
for(int i=1;i<=n;i++)
{
    for(int j=m;j>=0;j--)//倒序
    {
        for(int k=1;k<=s[i];k++)
        {
            if(j>=w[i][k])
            	dp[j]=max(dp[j],dp[j-w[i][k]]+v[i][k]);//第i组第k个物品
            //二维：dp[i][j]=max(dp[i][j],dp[i-1][j-w[i][k]]+v[i][k])
        }
    }
}
cout << dp[m] << endl;
```



## 求方案数

(注意 f 数组和 c 数组的初始化)

```c++
//01背包求方案数（不装满）
int c[N];	//c[i]背包容量为i时的方案数
int olbag1() {
    int mod = 1e9 + 7;
    for (int i = 0; i <= m; i++) {//初始化
        c[i] = 1;
    }
    for (int i = 1; i <= n; i++) {
        for (int j = m; j >= w[i]; j--) {
            if (f[j - w[i]] + v[i] > f[j]) {//装进一个物品的情况更优
                f[j] = f[j - w[i]] + v[i];
                c[j] = c[j - w[i]];
            } else if (f[j - w[i]] + v[i] == f[j]) {//装进物品但总价值不变
                c[j] = (c[j] + c[j - w[i]]) % mod;
            }
        }
    }
    cout << "方案数为： " << c[m] << endl;
}

//01背包求方案数（装满）
#define FINF 0x80000000
int olbag2() {
    int mod = 1e9 + 7;
    for (int i = 1; i <= m; i++) {//初始化
        f[i] = FINF;
    }
    f[0] = 0,c[0] = 1;
    //c[0]表示把背包容量为0时最优选法的方案数，我们可以理解为把某个物品放入背包容量为0时获得的价值是0是合法方案，因此当i取0时，也算一种方案
    
    for (int i = 1; i <= n; i++) {
        for (int j = m; j >= w[i]; j--) {
            if (f[j - w[i]] + v[i] > f[j]) {//装进一个物品的情况更优
                f[j] = f[j - w[i]] + v[i];
                c[j] = c[j - w[i]];
            } else if (f[j - w[i]] + v[i] == f[j]) {//装进物品但总价值不变
                c[j] = (c[j] + c[j - w[i]]) % mod;
            }
        }
    }
  
```



# DFS

#### **用dfs求连通块（种子填充：floodfill)**

**递归函数的步骤**

1.剪枝：

1）出界
2）不是原来的颜色
3）已访问
4）标记已访问
5）向各个方向递归

2.标记当前位置已访问

3.为联通分量进行编号

4.向各个方向递归

```c++
void ff_dfs(int x,int y,int org)
{
    if(x<1 || x>n || y <1 || y>m)	return ;//出界
    if(col[x][y] != org)	return ;//不是原来的颜色
    if(vis[x][y])	return ;//已访问
    vis[x][y]=1;//标记已访问
    //联通分量编号
    for(int i=0;i<4;i++)//向各个方向递归
    {
        int xx=x+dx[i],yy=y+dy[i];
        dfs(xx,yy,org);
    }
}
//****连通块****
//vis[x][y]==1?0
#include	<bits/stdc++.h>
using namespace std;
const int N=110,M=110;
int n,m,cnt=0;
int dx[8]={1,1,1,-1,-1,-1,0,0};
int dy[8]={0,-1,1,0,1,-1,1,-1};
bool vis[N][M];
char mp[N][M];
int g[N];//g[cnt]的cnt的最大值代表连通块的数量,g[i]的值代表第i个连通块中连通元素的个数

bool check(int sx,int sy)//判断连通块是否结束
{
	int t=0;
	for(int i=0;i<8;i++)
	{
		int x=sx+dx[i],y=sy+dy[i];
		if(x<1 || x>n || y<1 || y>m || !vis[x][y] || mp[x][y]=='.')//vis[x][y]==1?0
			t++;
	}
	if(t==8)
		return 1;
	return 0;
}

void dfs(int sx,int sy)
{
	vis[sx][sy]=1;
	id[sx][sy]=cnt;		//给连通块编号
    g[cnt]++;//增加连通块i中元素的个数，计算每个连通块的大小
	if(check(sx,sy))	//判断当前连通块是否结束
		cnt++;		//结束则编号加1
	for(int i=0;i<8;i++)
	{
		int x=sx+dx[i],y=sy+dy[i];
		if(!vis[x][y] && x>=1 && x<=n && y>=1 && y<=m && mp[x][y]=='W')
			dfs(x,y);  
		else
			continue;
	}
}

int main()
{
	cin >> n >> m;
	for(int i=1;i<=n;i++)
	{
		for(int j=1;j<=m;j++)
			cin >> mp[i][j];
	}
	for(int i=1;i<=n;i++)
	{
		for(int j=1;j<=m;j++)
			if(mp[i][j]=='W' && !vis[i][j])		//遍历图中所有未被访问的"W"
				dfs(i,j);
	}
	cout << cnt << endl;
	return 0;
}
//如果要计算每个连通块中元素的个数，加入数组g[N]
```

#### **迷宫问题**

```c++
//****迷宫问题****
#include	<bits/stdc++.h>
using namespace std;
int n,m,flag=0;
int dx[4]={1,0,-1,0};
int dy[4]={0,-1,0,1};
bool vis[45][45];
char mp[45][45];

void dfs(int sx,int sy)
{
	vis[sx][sy]=1;//标记此点已访问
	if(sx==n && sy==m)
    {
        flag=1;
        return ;
    }
	for(int i=0;i<4;i++)
	{
		int x=sx+dx[i],y=sy+dy[i];
		if(!vis[x][y] && x>=1 && x<=n && y>=1 && y<=m && mp[x][y]!='#')
			dfs(x,y);  //找到一个可以深入的点然后依此点继续递归下去，所以是深度先不断增加，然后通过不断递归返回，使得宽度不断增加
		else
			continue;
	}
}

int main()
{
	cin >> n >> m;
	for(int i=1;i<=n;i++)
	{
		for(int j=1;j<=m;j++)
			cin >> mp[i][j];
	}
	
	dfs(1,1);
	if(flag)
		cout << "YES"<< endl;
	else
		cout << "NO" << endl;
    //或者直接判断点[n,m]是否被访问过
    //if(vis[r][c])	cout << "YES" << '\n';
	//else	cout << "NO" << '\n';
	return 0;
}
```

#### **全排列问题**

```c++
//****全排列问题****
#include	<bits/stdc++.h>
using namespace std;
int n,cnt;
int vis[15];
int ans[15];
void dfs(int x)
{
	if(x>n)
	{
		for(int i=1;i<=n;i++)
			printf("%5d",ans[i]);
		printf("\n");
	}
	for(int i=1;i<=n;i++)
	{
		if(!vis[i])
		{
			vis[i]=1;
			ans[x]=i;
			dfs(x+1);
			vis[i]=0;
		}
	}
}
int main()
{
	cin >> n;
	dfs(1);
	return 0;
}
```



# BFS

bfs===>队列循环，因为队列具有先进先出的特性，所以是宽度先增加

## 迷宫问题

```c++
#include	<bits/stdc++.h>
using namespace std;
struct node{
	int x,y;
};
int r,c,x,y;
char g[45][45];//原数组
int vis[45][45];
int dx[4]={1,0,-1,0};//向各个方向移动
int dy[4]={0,1,0,-1};
int flag=0;

void bfs(int sx,int sy)
{
	queue<node> a;//队列,因为储存两个坐标值，因此使用struct,也可也使用pair
	vis[sx][sy]=1;
	a.push({sx,sy});
	while(!a.empty())
	{
		if(vis[r][c])//如果vis[r][c]==1，即能够走到点(r,c)，就退出循环
		{
			flag=1;
			break;
		}
		node p=a.front();
        a.pop();
		for(int i=0;i<4;i++)
		{
			x=p.x+dx[i],y=p.y+dy[i];
			if(x>=1 && x<=r && y>=1 && y<=c && vis[x][y]==0 && g[x][y] != '#')
			{
				vis[x][y]=1;//需要进行vis的更新因为不会进入递归函数实现开头的vis赋值为1
				a.push({x,y});//入队
			}
		}
	}
}
int main()
{
	cin >> r >> c;
	memset(vis,0,sizeof vis);
	for(int i=1;i<=r;i++)
	{
		for(int j=1;j<=c;j++)
			cin >> g[i][j];
	}
	bfs(1,1);//将起点传入
	if(flag)	cout << "YES" << '\n';
	else	cout << "NO" << '\n';
	return 0;
}
```





# 数据结构

## **st表（稀疏表）sparse table**

**！！！注意移位运算符的优先级和写法！！！**

用于解决可重复贡献问题（a opt a = a) 的数据结构，实现O(nlogn)预处理，O(1)查询

应用如：RMD（Range Minimum/Maximum query区间最值查询）,GCD（最大公因数），最小公倍数，区间按位或、按位与

```c
//dp[i][j]表示从元素a[i]开始，长度为2^j的区间内所有元素的最大值（最小值，根据具体情况而定）

//input
for(int i=1;i<=n;i++)
{
	cin >> a[i];
	dp[0][i]=a[i];//区间长度为2^0=1时，dp[0][i]中只包含a[i]这一个元素，因此最值为该数
}

//对s进行预处理
Log[1]=0;//log[i]表示以2为底，i为真数时对数的值
for(int i=2;i<=n;i++)
    Log[i]=Log[i/2]+1;

//cal
int maxN=log[n]//2^i=r-l+1,求i的最大值
for(int i=1;i<=maxN;i++)//列举区间长度2^i
{
    for(int j=1；j+(1<<i)-1<=n;j++)//列举区间起点j。则区间终点为：j+(1<<i)-1
        dp[j][i]=max(dp[j][i-1],dp[j+(1<<(i-1))][i-1]);
    	// dp[j][i]=gcd(dp[j][i-1],dp[j+(1<<(i-1))][i-1]);
    //以a[j]为起点，长度为2^i的区间内的最值=
    //(以a[j]为起点，长度为2^(i-1)的区间内的最值 
    //与
    //以a[j]+2^(i-1)为起点，长度为2^(i-1)的区间内的最值)
    //两个小区间的最值间再取最值
}

//inquery
for(int i=1;i<=m;i++)
{
    int l,r;
    cin >> l >> r;
    int s=log[r-l+1];
    ans=max(dp[l][s],dp[r-(1<<s)+1][s]);//因为2^s这个区间可能比r-l+1小，所以需要在以左端点为起点和以右端点为终点，长度都为2^s的两个区间的最值间再取最值，即为所查询区间内的最值
    cout << ans << "\n"[i==m] ;
}
//与普通的储存方式不同，我们可以通过倍增的方式将区间划分为两个长度大致相等的两个小区间，因此整个区间的最值=两个区间的最值中较大的那一个，所以最后的区间长度满足2^j <= n
//!!!!!!!!!注意左移右移的写法和优先级!!!!!!
//左移：需要移位的数字<<移位的次数n
//右移：需要移位的数字>>移位的次数n
//注意运算符的优先级，移位运算符的优先级低于算数运算符，因此记得加括号
//输入输出的数据较多时，采用scanf,printf,或快读函数
```



|          | 区间求和 | 区间最值查询 | 区间修改 | 单点修改 |
| -------- | -------- | ------------ | -------- | -------- |
| st表     |          | 1            |          |          |
| 前缀和   | 1        | 0            | 0        | 0        |
| 树状数组 | 1        | 1            | 0        | 1        |
| 线段树   | 1        | 1            | 1        | 1        |

**差分，前缀和：**

要求所求的运算有逆运算

置换，矩阵乘法，加法、减法、乘法、除法（逆元）

线段树，答案可以快速合并

**题目：**已知一个数列，你需要进行下面两种操作：

- 将某一个数加上 x*x*
- 求出某区间每一个数的和

区间查询——前缀和+树状数组维护

## 树状数组

BIT， 二进制下标树

修改和查询的**复杂度**：**log n**========>加速前缀和的更新

tr**数组大小**大致为原数组大小的两倍，即N<<1

### 维护区间和

```c++
#define lowbit(x) ((x)&(-x))//求出一个非负整数n在二进制表示下最低位1及其后面的0构成的数值
const int N=1e5+10;
int n,m,a[N];
ll tr[N<<1];//tr数组大小

//向上求和、向上更新
void update(int x,int k)//x为区间对应位置，k为所增加的数
{
	while(x<=n)
	{
		tr[x] += k;
		x+=lowbit(x);
	}
}

//前缀和+lowbit询问
ll query(int x)
{
	ll ans=0;
	while(x>=1)//不能写成x>=0！当x==0时会陷入死循环
	{
		ans+=tr[x];
		x-=lowbit(x);
	}
	return ans;
}

void solve()
{
	cin >> n >> m;
	for(int i=1;i<=n;i++)
	{
		cin >> a[i];
		update(i,a[i]);//用树状数组维护前缀和
	}
    
	while(m--)
	{
        int opt;
		cin >> opt;
		if(opt==1)//单点修改
		{
			int x,k;//将第x个元素增加k
			cin >> x >> k;
			update(x,k);
		}
		else if(opt==2)//利用前缀和实现区间查询
		{
            int l,r;
			cin >> l >> r;
			cout << query(r)-query(l-1) << endl;//注：不能直接查询区间[l,r]
		}
        else if(opt==3)//单点查询=区间长度为1的区间查询
        {
            cin >> x;
            cout << query(x)-query(x-1) << endl;
        }
	}
}
int main()
{
	Tie
	solve();
	return 0;
}
```

```c
//区间修改，单点查询
//===>引入a[N]的差分数组b[N]，用树状数组维护b[N]的前缀和，b[i]代表a[N]中每个元素的增量
b[i]=a[i]-a[i-1];
update(i,b[i]);//建树
update(l,d);//当区间[l,r]内所有数改变时，使用差分数组，只有区间端点处的增量发生改变
update(r+1,-d);
ans=query(x);//因为a[1]+(a[2]-a[1])+(a[3]-a[2])+……+a[x]=a[x]
```



### 维护区间最值

**时间复杂度：**

单点修改 = O(logn*logn)

区间查询 = O(logn*logn)

```c++
int a[N];//原序列
int tr[N];//树状数组：tr[x]表示区间[x,x-lowbit(x)+1]内的最大值

void update(int x)
{
    int lx=x;
    while(x<=n)
    {
        tr[x]=max(tr[x],a[lx]);//不断向上更新包含lowbit(x)的区间最值
        x+=lowbit(x);
    }
}

void query(int l,int r)
{
    int ans=-INF;
    while(r>=l)
    {
        ans=max(ans,a[r]);
        r--;//1.第一次比较后右端点向左推移一位  2.如果lowbit(r)的移动范围太大，则使用r--进行移动，下一轮循环再进行比较
        for(;r-lowbit(r)>=l;r-=lowbit(r))//前提：要选择的这个区间的左端点大于查询区间的左端点的情况下才能使用tr[r]，即r-lowbit(r)>=l
        {
            ans=max(ans,tr[r]);
        }
    }
    return ans;
}
```



## 线段树

**复杂度：**

**数组大小**：最多为原数组的4倍左右，即**N<<2**

<img src="https://raw.githubusercontent.com/fograinwater/PicGo-img/master/image-20220730103940239.png" style="zoom: 25%;" />

```c++
const int N=1e5+10;
int n,m;
int a[N];

struct node{
	int l,r;
	ll v,mx;
}tr[N<<2];//注意数组大小开到4N

//向上更新sum 、 max
inline void push_up(int pos)
{
	tr[pos].v=tr[pos<<1].l+tr[pos<<1|1].v;
	tr[pos].mx=max(tr[pos<<1].mx,tr[pos<<1|1].mx)
}
//建树
inline void build(ll pos,int l,int r)
{
	tr[pos].l=l,tr[pos].r=r;
	if(l==r)
	{
		tr[pos].v=a[l];//注意不是a[i]!  l、r对应1~n
		tr[pos].mx=tr[pos].v;
		return ;//勿忘return语句
	}
	int mid=(l+r)>>1;//因为是在区间[l,r]上建树，所以是根据l,r
	build(pos<<1,l,mid);
	build(pos<<1|1,mid+1,r);
	push_up(pos);//向上求sum，max
}
//区间修改
inline void update1(ll pos,int l,int r,int x)
{
	if(tr[pos].l>r || tr[pos].r<l)
		return ;
	if(tr[pos].l>=l && tr[pos].r<=r)//为修改的区间之内
	{
		tr[pos].v+=x*(tr[pos].r-tr[pos].l+1);
		return ;//勿忘return语句
	}
	update1(pos<<1,l,r,x);
	update1(pos<<1|1,l,r,x);
	push_up(pos);
}
//单点修改（是因为修改的方式不同，所以设为两个不同的函数；
//事实上单点修改也可以看作区间修改：k==[k,k]
inline void update2(ll pos,int l,int r,int x)
{
	if(tr[pos].l>r || tr[pos].r<l)
		return ;
	if(tr[pos].l>=l && tr[pos].r<=r)
	{
		tr[pos].v*=x;
		return ;//勿忘return语句
	}
	update2(pos<<1,l,r,x);
	update2(pos<<1|1,l,r,x);
	push_up(pos);
}
//区间、单点询问
void query(llpos,int l,int r)
{
	if(tr[pos].l>r || tr[pos].r<l)
		return ;
	if(tr[pos].l>=l && tr[pos].r<=r)
		return tr[pos].v;
	int mid=(tr[pos].l+tr[pos].r) >> 1;//是根据当前区间的mid值与l,r的关系来判断往左还是往右走
	if(r<=mid)
		return query(pos<<1,l,r);//整个判断的大区间[l,r]不变
	else if(l>mid)
		return query(pos<<1|1,l,r);
	else
		return query(pos<<1,l,r)+query(pos<<1|1,l,r);
}

void solve()
{
	IOS
	cin >> n;
	for(int i=1;i<=n;i++)
		cin >> a[i];
	build(1,1,n);//记得建树
	
	cin >> m;//询问次数
	int opt;
	while(m--)
	{
		cin >> opt;
		if(opt==1)
		{
			int l,r,x;//区间修改
			cin >> l >> r >> x;
			update1(1,l,r,x);
		}
		else if(opt==2)//单点修改
		{
			int k,x;
			cin >> k >> x;
			update2(1,k,k,x);
		}
		else
		{
			int l,r;//区间查询
			cin >> l >> r;
			cout << query(1,l,r) << endl;
		}
	}
}
int main()
{
	IOS
	solve();
	return 0;
}
```

```c++
#include    <bits/stdc++.h>

using namespace std;
#define int long long
#define endl '\n'
#define ls (pos << 1)
#define rs (pos << 1 | 1)
const int N = 1e4 + 3;
int tr[N << 1];
int lazy[N << 1];

void mark(int pos, int l, int r, int k) {
    tr[pos] += (r - l + 1) * k;
    lazy[pos] += k;
}

void pushdown(int pos, int l, int r) {
    int mid = (l + r) >> 1;
    mark(ls, l, mid, lazy[pos]);
    mark(rs, mid + 1, r, lazy[pos]);
    lazy[pos] = 0;
}

//当前区间[l,r]待修改区间[x,y]
void update(int pos, int l, int r, int x, int y, int k) {
    int mid = (l + r) >> 1;
    if (x <= l && y >= r) {//当前区间为待修改区间的子区间，则直接修改此区间，并加上标记
        mark(pos, l, r, k);
        return;
    }
    pushdown(pos, l, r);//当前区间的子区间要被更新了，所以需要把之前没有加的数全加上去
    if (x <= mid) update(ls, l, mid, x, y, k);
    if (y > mid) update(rs, mid + 1, r, x, y, k);
    tr[pos] = tr[ls] + tr[rs];//将父节点的值也做相应更新
}

int query(int pos, int l, int r, int x, int y) {
    int ret = 0;
    int mid = (l + r) >> 1;
    if (x <= l && y >= r) return tr[pos];
    pushdown(pos, l, r);
    if (x <= mid) ret += query(ls, l, mid, x, y);
    if (y > mid) ret += query(rs, mid + 1, r, x, y);
    return ret;
}

signed main() {

}
```



## 字典树（前缀树）

关于字符串的数据结构，存储和查询字符串

<img src="https://raw.githubusercontent.com/fograinwater/PicGo-img/master/image-20220730104045503.png" style="zoom: 50%;" />

```c++
const int N=5e3+10;//N=n(字符串最多个数）*m（每个字符串的最长长度）
int trie[N][26];
bool color[N];//标记终点节点
int cnt=0;//cnt是为了标记不同的节点，而不是为了标记不同的字母、单词，因此不必在插入下一个字符串的时候将其清0
//插入
void insert(string s,int len)
{
	int p=0;//每次都从根节点开始判断、插入
    for(int i=0;i<len;i++)
    {
        int c=s[i]-'a';
        if(!trie[p][c])//没有此节点则创建
            trie[p][c]=cnt++;
        p=trie[p][c];//定位当前字符所在节点位置
    }
    color[p]=1;//标记此单词的最后一个字符所在位置为黑色的终点
  //count[p]++ ==> 记录以节点p结尾的单词的个数===>词频统计
}
//查询
int find(string s,int len)
{
    int p=0;
    for(int i=0;i<len;i++)
    {
        int c=s[i]-'a';
        if(!trie[p][c])
            return 0;//根节点下面没有这个字符串的首字符则一定没有这个字符串
        p=trie[p][c];
    }
    return color[p]==1;//如果当前节点为终点则返回1，反之虽然该字符串在字典树中出现了，但是只作为前缀出现，返回0
}//返回值根据题目条件而定
```

## 

## 栈（先进后出）

类似：瓶子

输出给定数列中每个元素右边第一个大于它的元素的下标

```c
const int N=3e6+10;
int n,a[N],ans[N];//a[N]为原来元素组，ans[N]为下标答案组

int main()
{
	cin >> n;
	stack<int> s;//栈
	for(int i=1;i<=n;i++)
	    scanf("%d",&a[i]);
	for(int i=1;i<=n;i++)
	{
	    while(!s.empty() && a[i]>a[s.top()])//栈不为空！(否则陷入死循环)且当前元素大于栈顶元素
	    {
	        ans[s.top()]=i;//将该元素的下标记录为栈顶元素的答案
	        s.pop();//弹出栈顶元素,因为此时栈顶元素对应的答案已经找到了
	    }//循环实现了对栈顶元素的不断访问
	    s.push(i);//将该元素放入栈内
	}
	for(int i=1;i<=n;i++)
		 cout << ans[i] << " ";
	return 0;    
}
```

### **数组对栈的模拟**

```c++
int s[N];
int top;//栈顶元素的下标，初始值为0表示没有任何元素
//插入元素到栈顶
s[++top]=a;//先往后移一个便于判断栈的状态
//弹出栈顶元素
top--;
//查看栈是否为空
if(top==0) return 1;//空
//输出栈顶元素
cout << s[top];
```

# 

# 图论

## 存储方式

### 1. 邻接矩阵

查询边是否存在 ：O(1)

遍历一个点的所有出边：O(n)

遍历整张图：O(n^2^)

空间复杂度：O(n^2^)

只适用于没有重边（或重边可以忽略）的情况，适用于稠密图

```c++
for(int i=1;i<=n;i++)
{
	int u,v,w;
    cin >> u >> v >> w;
    adj[u][v]=w;//adj[u][v]=1
}
```

### 2. 邻接表

```c++
//只存储出边
//vector<int> mp[N]
vector<vector<int> >mp;//mp[u][v],一维就表示以节点u为起点的所有出边的终点v，因为vector是需要多少空间才开多少空间，所以空间浪费小
//存储出边的终点及边权
struct egde{
    int v,w;
    edge()=default;
    edge(int vv,int ww):v(vv),w(ww){}
    bool operator < (const edge &e) const{
        return w<e.w;
    }
}
vector<edge> mp[N];

for(int i=1;i<=m;i++)
{
    int u,v,w;
    mp[u].emplace_back(v, w);
    //mp[v].emplace_back(u,w);
}
```

### 3. 链式前向星

以存边的方式**存储图**的数据结构

```c
const int M=1e5+10;//max边数
const int N=1e4+10;//max节点数
int n,m;//n个节点
int tot=1;//边的编号枚举
int head[N];//公共起点的列举
struct node{
    int to;//终点
    int w;//边的权重
   	int nxt;//后面
}edge[M];
void add_edge(int u,int v,int c)
{
    edge[tot].to=v;
    edge[tot].w=c;
    edge[tot].nxt=head[u];//将当前边链接到最前面
    head[u]=tot;
    tot++;
}
int main()
{
    cin >> n >> m;
    int a,b,c;//起点，终点，边的权重
	for(int i=1;i<=m;i++)//m条边
    {
        cin >> a >> b >> c;
        add_edge(a,b,c);
    }  
    //打印以x为起点的所有边
    int x=4;
    for(int i=head[x];i!=0;i=edge[i].nxt)
        cout << x << "---->" << edge[i].to << endl;
    return 0;
}
/*
4 5
4 2 30
4 3 20
2 3 20
2 1 30
1 3 40
4
*/
```

## 最近公共祖先LCA

```c
const int N=5e5+10;
int tot=1;//边的总数
int head[N];//边的起点
int dep[N],f[N][21];//dep[i]第i个节点的深度，f[i][j]表示从第i个节点向上跳2^j格所到达的节点
struct node{
	int to;//终点
	int next;
}e[N<<1];
//加边函数
void add(int u,int v)
{
	e[tot].to=v;
	e[tot].next=head[u];
	head[u]=tot;
	tot++;
}
//将所有节点的dep和位置表示出来
void dfs(int now,int fa)//fa代表父节点
{
	f[now][0]=fa;
	dep[now]=dep[fa]+1;
	for(int i=1;i<=20;i++)//i从1开始
	{
		f[now][i]=f[f[now][i-1]][i-1];
	}
	for(int i=head[now];i!=-1;i=e[i].next)
	{
		if(e[i].to!=fa)
			dfs(e[i].to,now);
	}
}
int LCA(int x,int y)
{
	if(dep[x]<dep[y])
		swap(x,y);
	for(int i=20;i>=0;i--)
	{
		if(dep[f[x][i]] >= dep[y])	
			x=f[x][i];//使x,y处于同一深度
	}
	if(x==y)
		return x;
	for(int i=20;i>=0;i--)
	{
		if(f[x][i] != f[y][i])
			x=f[x][i],y=f[y][i];
	}//直到跳到LCA的下面一个节点
	return f[x][0];
}
int main()
{
	IOS
	memset(head,-1,sizeof head);
	int n,m,s;
	cin >> n >> m >> s;
	int a,b;
	for(int i=1;i<n;i++)
	{
		cin >> a >> b;
		add(a,b);
		add(b,a);//无向无权图的两条边
	}
	//求出深度
	dfs(s,0);
	int x,y,ans;
	for(int i=1;i<=m;i++)
	{
		cin >> x >> y;
		ans=LCA(x,y);
		cout << ans << endl;
	}
	return 0;
}
```



```c
#include	<bits/stdc++.h>
using namespace std;
int n,cnt;
int vis[15];
int ans[15];//储存结果,ans[x]代表每一行的第几格
void dfs(int x)
{
	if(x>n)//x==n+1
	{
		for(int i=1;i<=n;i++)
			printf("%5d",ans[i]);
		printf("\n");
	}
	for(int i=1;i<=n;i++)
	{
		if(!vis[i])//每递归一层，标记就会多一个，那么可以储存的解也就少一个
		{
			vis[i]=1;
			ans[x]=i;
			dfs(x+1);
			vis[i]=0;//递归返回时撤销标记
		}
	}
}
int main()
{
	cin >> n;
	dfs(1);
	return 0;
}
```



## 并查集DSU

[算法学习笔记(1) : 并查集 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/93647900)

```c++
const int N = 1e5 + 10;
int f[N];
//初始化
void init(int n){
    for (int i = 1; i <= n; i++)
        f[i] = i;
}

//查询
int findf(int x)//寻找x的祖先
{
    if (x == f[x])
        return x;
    else
        return f[x] = findf(f[x]);//路径压缩，记忆化搜索，实现直接从该节点指向根节点
}

//合并
void merge(int i, int j) {
    int fa = findf(i);
    int fb = findf(j);
    f[fa] = fb;//使i的根节点指向j的根节点
}

bool same(int x,int y){
    return findf(x) == findf(y);
}
```

**优化：按秩合并**

合并时比较两个根节点，把rank较小者往较大者上合并

```c++
int f[N];
int rank[N];//记录以结点i为根节点的树的深度

void init(int n){
	for(int i = 1; i <= n; i++){
        f[i] = i;
        rank[i] = 1;
    }
}

void merge(int x,int y){
    int fx = find(x), fy = find(y);
    if(fx == fy)	return ;
    if(rank[fx] > rank[fy])
        f[fy] = fx;
    else{
        f[fx] = fy;
        if(rank[fx] == rank[fy])
            rank[y]++;
    } 
}
```



# 最短路问题

**最短路的定义**

在一个赋权图G中，点u到v有若干条通路，定义u到v的最短路为这些通路中所有**边权值的和最小**的一条。

**最短路问题分为单源最短路和多源最短路**

单源最短路，顾名思义，就是求图 *G* 中一个结点（源点）到其他结点的最短路径

多源最短路，就是求图 *G* 中任意结点到其他结点的最短路径

**dis[i]：**源点到节点i的当前最短路

**松弛（relax）**：对于一条边 *e(u→v : w)* （从 *u* 指向 *v*，边权为 *w*），如果有 *dis*[*v*] > *dis*[*u*] + *w* ，则我们将 *dis*[*v*] 的值更新为 *dis*[*u*] + *w*，这个过程称为对边的松弛

**负环**：若图中存在一个环，并且构成环的边的权值之和为负值，则称该环为一个负环

![https://s2.loli.net/2022/03/15/bhjT63KQnGyut7S.png](https://s2.loli.net/2022/03/15/bhjT63KQnGyut7S.png)

如图所示，环 0 → 2 → 4 → 0 的边权和为  − 1 ，它是一个负环；与之相对，环 0 → 5 → 4 → 0 的边权和为 8 ，它不是一个负环。需要注意的是，有向图中具有负权边不代表着一定有负环，而无向图中具有负权边则一定有负环（因为负权边是双向的）

由于负环的特性，我们每经过一次负环，最短路的长度都会减小，直至负无穷。所以，在具有负环的图上讨论最短路是没有意义的。



## 单源最短路

### 1. 朴素算法：Bellman_ford

**做法：**

1.给定图G(V, E)（其中V、E分别为图G的顶点集与边集），源点s，数组dis[i]记录从源点s到顶点i的路径长度，初始化数组Distant[n]为, Distant[s]为0；

以下操作循环执行至多n-1次，n为顶点数：
2.对于每一条边e(u, v)，如果dis[u] + w(u, v) < dis[v]，则另dis[v] = dis[u]+w(u, v)。w(u, v)为边e(u,v)的权值；
3.若上述操作没有对dis进行更新，说明最短路径已经查找完毕，或者部分点不可达，跳出循环。否则执行下次循环；

4.为了检测图中是否存在负环路，即权值之和小于0的环路。对于每一条边e(u, v)，如果存在dis[u] + w(u, v) < dis[v]的边，则图中存在负环路，即是说改图无法求出单源最短路径。否则数组dis[n]中记录的就是源点s到各顶点的最短路径长度。

**时间复杂度：**

O(n*m) 【n为节点数，m为边数】

**优点：**

可用于带**负权图**中（可能有负环），求一个顶点到另一个顶点的最短路。在源点可以到达的情况下，此算法可以**判断负环是否存在**，但是当以节点s为源点出发没有找到负环不代表不存在负环，只代表从该节点出发不能到负环。

**缺点：**

时间复杂度高

```c++
#include <bits/stdc++.h>

#define int long long
using namespace std;
typedef long long ll;
const int N = 2e5 + 5;
const int inf = 0x3f3f3f3f3f3f3f3fL;
int dis[N]; //dis[i]：结点到源点i的最短距离

typedef struct edge {
    int u, v, w;

    edge(int u_, int v_, int w_) : u(u_), v(v_), w(w_) {}
} edge;

vector<edge> mp;//邻接表，存储边

bool Bellman_Ford(int s, int n) {
    for (int i = 1; i <= n; i++)	//初始化dis数组
        dis[i] = inf;
    dis[s] = 0;                    	//源点到自己的最短路径长度为0
    bool ok = true;                	//标记当前轮是否成功进行一次松弛
    int lim = n;                	//设置松弛轮数的限制，最多只能进行n次松弛，如果限制归零则说明有负环

    while (lim-- && ok) {        	//如果某一轮所有边都不能松弛说明已经求出了所有的最短路
        ok = false;

        //遍历图中的所有边并松弛
        for (int i = 1; i <= n; i++) {
            for (auto &edge: mp) {
                int u = edge.u, v = edge.v, w = edge.w;
                if (dis[v] > dis[u] + w)//结点v能够以结点u作为中转点，对边e(u->v:w)进行松弛操作
                {
                    dis[v] = dis[u] + w;
                    ok = true;
                }
            }
        }

    }//一次whil循环至少能求出一条最短路径

    return lim >= 0;                    //如果有负环则lim == -1，返回false
}

signed main() {
    int n, m, s;
    cin >> n >> m >> s;
    for (int i = 0; i < m; i++) {
        int u, v;
        ll w;
        cin >> u >> v >> w;
        mp.emplace_back(u, v, w);//无向图,要存两条边
        mp.emplace_back(u, v, w);
    }
    Bellman_Ford(s, n);

    for (int i = 1; i <= n; i++)
        cout << dis[i] << " ";
    return 0;
}
```

对lim的理解

对外层循环有n - 1次的理解



### 2. SPFA

**对Bellman_ford的队列优化**

复杂度：最坏的情况下为O(n*m)

```c++
#include    <bits/stdc++.h>

using namespace std;
#define int long long
const int INF = 2147483647;
const int N = 1e4 + 10, M = 5e5 + 10;
int n, m, s, cnt = 0;//n节点数，m边数，s源节点
int dis[N];//dis[i]表示节点i到源节点s的最短距离
bool vis[N];
int head[M];

//**1.链式前向星存储图**
struct node {
    int to;
    int w;
    int nxt;
} edge[M];

//**2.addedge 加边函数**
void addedge(int u, int v, int w) {
    edge[++cnt].to = v;
    edge[cnt].w = w;
    edge[cnt].nxt = head[u];
    head[u] = cnt;
}

//**3.SPFA函数**
void SPFA(int s) {
    queue<int> q;
    for (int i = 1; i <= n; i++)//初始化dis[N]、vis[N]数组
    {
        dis[i] = INF;
        vis[i] = 0;//标记节点i是否在队列中
    }
    q.push(s);//起点入队
    vis[s] = 1;
    dis[s] = 0;//源节点到自己的距离为0
    while (!q.empty()) {
        int u = q.front();
        q.pop();//一定记得出队！！！
        vis[u] = 0;//出队操作
        for (int i = head[u]; i; i = edge[i].nxt)//对以节点u为起点的所有终点v进行访问,最后一个是0
        {
            int v = edge[i].to;
            int w = edge[i].w;
            if (dis[v] > dis[u] + w) {
                dis[v] = dis[u] + w;//松弛
                if (!vis[v]) {
                    vis[v] = 1;
                    q.push(v);//如果u的邻近节点v被松弛，则将其入队，等待while循环的访问
                }
            }
        }
    }
}

signed main() {
    cin >> n >> m >> s;
    for (int i = 1; i <= m; i++) {
        int u, v, w;
        cin >> u >> v >> w;
        addedge(u, v, w);//有向图
        //addedge(v,u,w);	if无向图还得加上另一条边
    }
    SPFA(s);
    for (int u = 1; u <= n; u++) {
        for (int i = head[u]; i; i = edge[i].nxt)//每一次进入while循环的for循环都是对以节点u为起点的所有终点进行访问
        {
            int v = edge[i].to;
            int w = edge[i].w;
            cout << u << "--" << v << ": " << w << endl;
        }
        cout << endl;
    }
    for (int i = 1; i <= n; i++)
        cout << dis[i] << " ";
    return 0;
}
```



### 2. Dijkstra

前置知识：邻接表，链式前向星[轻松搞懂dijkstra算法+堆优化 || 原理+实战 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/34624812)

适用于：**非负权图**中（无负环），求一个顶点到另一个顶点的最短路

**时间复杂度**：使用优先队列进行优化 O(nlogn)

**算法思想：**

1.从初状态开始想，除了源节点到本身的最短路已经求出（为0），其他点到源点的最短路长度都是未知的；

2.但是可以知道，一定存在一些点与源节点s有直接连边（否则源点就是一个孤立点，那么其他点都不能到达该源点，就没有求最短路的必要了），而对于这些与源点有直接连边的结点中，一定存在一个结点f，它到源点的边的边权最小，那么该边就是该点到源点的最短路径。（因为如果这条边不是最短路径，则说明该点可以通过另一个中转点来实现最短路，但这实际上与上面的假设是相悖的，此时另一个中转点才应该是被选出来的到源点的边的边权最小的那个点）。我们将选出来的这第一个点记为f。

3.对于剩下的结点，他们到源点的最短路dis[i]应该产生于两种方式：

1）该点到源点的直接连边 dis[i] = edge[s] [i]；

2）把已求出最短路径的结点作为中转点，dis[i] = dis[u] + edge[u] [i]

取较小值就可以。

4.如何实现上面的过程？

1）存储结构：邻接表存图

2）取最短路径：用**优先队列**存储已求出最短路径的结点和对应的最短路的长度

因为优先队列默认的排序规则是从大到小，而我们每次要取出的是最短的一条路径作为未求出最短路的结点的中转点，所以需要在结构体中改变一下排序规则，按照路径的长度从小到大排序；

3）遍历与取出的结点u有直接连边的结点v，如果满足dis[v]  > dis[u] + w，则更新dis[v]，并将结点v入队，因为它已求出了最短路，也可能作为其他结点的中转点。

4）不断操作直到队列为空即求出所有最短路

**具体操作：**

```c++
#include    <bits/stdc++.h>

using namespace std;
#define int long long
const int INF = 0x3f3f3f3f3f3f3f3f;
const int N = 1e5 + 10;
const int M = 5e5 + 10;
int head[N];
int dis[N];    //dis[i]：结点i到源点的最短距离
bool vis[N];//数组大小
int n, m, s, cnt = 0;

struct eg {
    int to, w, nxt;
} edge[M];

struct node {
    int dis, v;

    bool operator<(const node &x) const {
        return dis > x.dis;
    }
};

void addedge(int u, int v, int w) {
    edge[++cnt].to = v;
    edge[cnt].w = w;
    edge[cnt].nxt = head[u];
    head[u] = cnt;
}

void dijkstra(int s) {
    priority_queue<node> q;
    for (int i = 1; i <= n; i++) {
        dis[i] = INF;    //初始化最短路径为INF,便于之后的更新
        vis[i] = false;    //最初都没被访问过
    }
    dis[s] = 0;            //源点到自己的最短路为0
    q.push((node) {0, s});//源节点入队
    while (!q.empty()) {
        //取出队首结点点，该结点到源点的路径长度是已求出的最短路中最短的
        node temp = q.top();
        int u = temp.v;
        q.pop();
        if (vis[u]) continue;//如果已经走过，则看下一个队列中的节点

        vis[u] = 1;        //标记已访问！！
        for (int i = head[u]; i; i = edge[i].nxt) {
            //取出的这个最短路径对应的结点肯定只能作为与它有直接连边的结点的中转点
            int v = edge[i].to;
            int w = edge[i].w;
            if (dis[v] > dis[u] + w && !vis[v]) {    //松弛
                dis[v] = dis[u] + w;
                q.push((node) {dis[v], v});
                //对结点v求出了最短路，结点v又可以作为中转点，所以将其也入队
            }
        }
    }
}

signed main() {
    cin >> n >> m >> s;
    for (int i = 1; i <= m; i++) {
        int u, v, w;
        cin >> u >> v >> w;
        addedge(u, v, w);//有向图
        //addedge(v, u, w);无向图
    }
    dijkstra(s);
    for (int i = 1; i <= n; i++)
        cout << dis[i] << " ";
    return 0;
}
```



**Dijkstra与SPFA的区别：**

**Dijkstra算法：**是基于贪心和DP的思路，一开始先将所有点到原点的距离设置为无穷大，特别的是dis[s]=0，此处的s为原点，它是每次找到离原点最近的点，放入堆中（成为堆顶）并且标记，再以这个点为起点去更新与它相连的点，这也就决定了Dijkstra算法不能处理负权图，假设第一次找到了离原点最近的点为X，再以X为起点去更新与X相连的点，如果存在负边的话，那当前找到的与X相连的点到X的距离也就不一定是最小了，这就破坏了贪心的思路，算法也就出问题了。
**SPFA算法：**它是要对所有的边去进行一次松弛操作，进行了n-1次更新，先初始化dis数组，起点赋值为0，其余赋值为无穷大，先起点入队列，入了队列的被标记，当队列不为空时循环，队首元素出队，松弛与队首元素相连的边，这些被更新的点如果不在队列中就加入队列， 再次队首元素出队，松弛与队首元素相连的边，它是不**需要去找离原点最近的点**的，所以Dijkstra算法用的是小根堆优化，SPFA直接用的队列。
[(单源最短路) Dijkstra算法与SPFA算法的区别与统一 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/573869455)

![img](https://pic2.zhimg.com/v2-fb81ac516aa923a478ff35687326c125_r.jpg)

例题：

#### P1144 最短路计数（dijkstra)

【有多种方法可做，其他方法可以见其他题解】

```c++
#include    <bits/stdc++.h>

using namespace std;
#define int long long
const int INF = 0x3f3f3f3f3f3f3f3f;
const int N = 1e5 + 10;
const int M = 5e5 + 10;
const int mod = 100003;
int head[N];
int dis[N];    //dis[i]：结点i到源点的最短距离
bool vis[N];
int ans[N];
int n, m, s, cnt = 0;

struct eg {
    int to, w, nxt;
} edge[M];

struct node {
    int dis, v;

    bool operator<(const node &x) const {
        return dis > x.dis;
    }
};

void addedge(int u, int v, int w) {
    edge[++cnt].to = v;
    edge[cnt].w = w;
    edge[cnt].nxt = head[u];
    head[u] = cnt;
}

void dijkstra(int s) {
    priority_queue<node> q;
    for (int i = 1; i <= n; i++) {
        dis[i] = INF;    //初始化最短路径为INF,便于之后的更新
        vis[i] = false;    //最初都没被访问过
        ans[i] = 0;
    }
    dis[s] = 0;            //源点到自己的最短路为0
    ans[s] = 1;
    q.push((node) {0, s});//源节点入队
    while (!q.empty()) {
        //取出队首结点点，该结点到源点的路径长度是已求出的最短路中最短的
        node temp = q.top();
        int u = temp.v;
        q.pop();
        if (vis[u]) continue;//如果已经走过，则看下一个队列中的节点

        vis[u] = 1;        //标记已访问！！
        for (int i = head[u]; i; i = edge[i].nxt) {//取出的这个最短路径对应的结点肯定只能作为与它有直接连边的结点的中转点
            int v = edge[i].to;
            int w = edge[i].w;
            if (dis[v] > dis[u] + w) {    //松弛
                dis[v] = dis[u] + w;
                q.push((node) {dis[v], v});//对结点v求出了最短路，结点v又可以作为中转点，所以将其也入队
                ans[v] = ans[u] % mod;//等于以u为中转的方案数
            } else if (dis[v] == dis[u] + w) { //如果当前最短路径的长度与以u为中转点长度相同，则方案数增加
                ans[v] = (ans[v] + ans[u]) % mod;
            }
        }
    }
}

signed main() {
    cin >> n >> m;
    for (int i = 1; i <= m; i++) {
        int u, v;
        cin >> u >> v;
        addedge(u, v, 1);//有向图
        addedge(v, u, 1);//无向图
    }
    dijkstra(1);
    for (int i = 1; i <= n; i++)
        cout << ans[i] << endl;
    return 0;
}
```



## 多源最短路

### Floyd

**应用：**求任意两个节点之间的最短路

[Floyd算法详解 通俗易懂 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/339542626)

其他应用：求节点间的**最长路**-----floyd, 边权取负

**时间复杂度**：O(n^3^)

**具体思路：**

**dis[i] [j]**：有向边i->j的长度

1.要寻找任意两点间的最短路径，比如点i和点j，很容易想到，i到j的路径有两种情况：

1）i到j有直接连边；

2）i到j之间由一些点来中转

第一种情况可以由输入的边的情况得出，着重考虑第二种情况：

假设i到j的一个中转点为k，那么这个中转点应该满足的条件应该有：

1）i到k有边，k到j也有边；

2）dis[i] [k] + dis[k] [j] < dis[i] [j]

2.所以我们可以遍历起点i和终点j，枚举中转点k，用三层for循环实现即可。

**缺点：**时间复杂度高

**优点：**代码短，容易理解

```c++
#include    <bits/stdc++.h>
using namespace std;
typedef long long ll;
#define INF 0x3f3f3f3f
const int N = 1e5 + 3;
int n, m, t;
ll dis[N][N];//邻接矩阵，dis[i][j]表示从节点i到j的最短路长度
int path[N][N];//i到j最短距离路径的中转点

int main() {
    cin >> n >> m >> t;
    memset(dis, 0x3f, sizeof dis);
    for (int i = 1; i <= n; i++)
        dis[i][i] = 0;//自己到自己的最短路为0
    for (int i = 1; i <= m; i++) {
        int u, v, w;
        cin >> u >> v >> w;
        dis[u][v] = dis[v][u] = w;
    }

    for (int k = 1; k <= n; k++)//枚举中转节点
    {
        for (int i = 1; i <= n; i++)//枚举起点
        {
            for (int j = 1; j <= n; j++)//枚举终点
                if(dis[i][k] + dis[k][j] < dis[i][j]){
                 	dis[i][j] = dis[i][k] + dis[k][j];
                    path[i][j] = k;//路径，存储前面的点（最近中转点）
                }
        }
    }
    for (int i = 1; i <= t; i++)//询问
    {
        int u, v;
        cin >> u >> v;
        if (dis[u][v] == INF)
            cout << -1 << endl;
        else
            cout << dis[u][v] << endl;
    }
    return 0;
}
```

**例题：P1119 灾后重建**

1）因为每次询问的是不同点之间的最短路径，所以肯定要用Floyd求解；

2）不能直接套Floyd，因为有时间的限制；

3）因为编号为0~n-1的村庄的重建时间是依次非递减的，所以村庄也是按照编号从小到大依次建立的；

4）可以根据询问的t，判断村庄是否完成重建，如果某个村庄在询问的时间之前完成了重建，那它就可以作为中转点，去更新dis[i][j]；

5）求解最短路时，还需要判断询问的两个村庄是否完成了重建，如果没有完成则是不可达的。

•**重点**：在于对Floyd最外层循环的理解，某个点何时可以作为中转点

```c++
#include    <bits/stdc++.h>

using namespace std;
typedef long long ll;
#define INF 0x3f3f3f3f3f3f3f3f
const int N = 1e5 + 3;
const int M = 205;

ll dis[M][M];//邻接矩阵，dis[i][j]表示从节点i到j的最短路长度
int path[M][M];//i到j最短距离路径的中转点
int a[M];//每个村庄完成重建的时间
int n, m;

//按照时间限制中转点
void update(int k) {
    for (int i = 0; i < n; i++)//起点
    {
        for (int j = 0; j < n; j++)//终点
            if (dis[i][k] + dis[k][j] < dis[i][j]) {
                dis[i][j] = dis[i][k] + dis[k][j];
                path[i][j] = k;
            }
    }
}

int main() {
    cin >> n >> m;
    memset(dis, 0x3f, sizeof dis);//没有直接连边就预设为INF
    for (int i = 0; i < n; i++) dis[i][i] = 0;//自己到自己的最短路为0
    for (int i = 0; i < n; i++) cin >> a[i];//编号为i的村庄重建完成的时间
    for (int i = 1; i <= m; i++) {
        ll u, v, w;
        cin >> u >> v >> w;
        dis[u][v] = dis[v][u] = w;//双向边
    }

    int q;
    cin >> q;
    int now = 0;//当前完成重建的村庄的数量
    for (int i = 1; i <= q; i++)//询问
    {
        int x, y, t;
        cin >> x >> y >> t;
        while (a[now] <= t && now < n) {//如果在询问时间之前完成了重建，则可以作为中转点
            update(now);
            now++;
        }
        if (a[x] > t || a[y] > t)//没有完成重建
        {
            cout << -1 << endl;
        } else {
            if (dis[x][y] == INF) cout << -1 << endl;
            else cout << dis[x][y] << endl;
        }
    }
    return 0;
}
```



## 最小生成树

在一个无向图 *G* 中，找到一个所有边的无环子集 T包含于 E ，使得所有结点相互联通，且子集 *T* 中所有边的边权和最小。显然，所求无环子集是一棵树，我们称这样的树为（图 *G* ）的最小生成树。

### 1. Kruskal（加边）

复杂度：O(mlogm)

前置知识：并查集，贪心

将图 *G* 中的所有边按边权从小到大排序，然后遍历排序后的边，如果这条边是安全边，则将其添加进边集 *T* 中，否则跳过这条边

安全边的判断：该边所连接的两个顶点是否连通

fa[N] : 存放所有节点的根节点（以此查看两个节点是否属于同一连通块）

```c++
//存图：选择合适的数据结构===>邻接表（结构体）
//因为最开始每个点都是独立的节点，所以将fa[N]数组初始化成每个节点的编号
//排序:将所有边按边权从小到大进行排序
//遍历所有边：判断是否为安全边——>yes加入边权，连接节点
//判断安全边：并查集，与前面所有的边不在同一个连通块中
#include	<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N=1e6+10;

struct edge{
	int u,v;
	ll w;
	
	edge()=default;
	
	edge(int uu,int vv,ll ww) : u(uu),v(vv),w(ww){}
	
	bool operator<(edge &e)const{
		return w<e.w;
	}
};

vector<edge> mp;
vector<int> fa(N);

int find_fa(int x)
{
	if(x==fa[x])//找到了最深处的父亲	
        return x;
    else
        return fa[x]=find(fa[x]);//在寻找根节点root的同时，实现从该节点到根节点的直接连接，即fa[x]=root;
}

void merge_fa(int u,int v)
{
	int faa=find_fa(u);
	int fab=find_fa(v);
	fa[faa]=fab;//要使a,b都指向同一个父亲
}

ll kruskal(int n)
{
	sort(mp.begin(),mp.end());//边权升序
	for(int i=1;i<=n;i++)
		fa[i]=i;
    
	ll ans=0;
	int cnt=0;
	for(auto &[u,v,w]:mp)
	{
		if(find_fa(u) == find_fa(v))
			continue;
		ans+=w;
		cnt++;
		merge_fa(u,v);
		if(cnt==n-1)//已经连接了n-1条边，即已经生成了最小生成树
			return ans;//因为每条边加入的条件是该边所连接的两个节点不能属于同一个连通块，因此每加入一条边就相当于加入一个点（第一条边相当于加入两个点）
	}
	return -1;//没有找到最小生成树
}
int main()
{
	int n,m;
	cin >> n >> m;
	for(int i=1;i<=m;i++)
	{
		int u,v;
		ll w;
		cin >> u >> v >> w;
		mp.emplace_back(u,v,w);
		mp.emplace_back(v,u,w);
	}
	ll ans=kruskal(n);
	if(ans==-1)
		cout << "orz" << endl;
	else
		cout << ans<< endl;
	return 0;
}
```



### 2. Prime（加节点）

复杂度：O(mn)，相比Kruskal更高

维护一个点集，表示目前已经确定加入最小生成树的点，在这个点集所有的出边里，找出边权最小并且指向未加入最小生成树的点的边，将其加入最小生成树，直到所有点都加入最小生成树

```c++
#include	<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N=5e3+10,M=2e5+10;
int n,m;

struct node{
	int v;//与kruskal的区别
	ll w;
	
	edge()=default;
	
	edge(int vv,ll ww) : v(vv),w(ww) {}
	
	bool operator < (const edge &e) const{
		return w > e.w;
	}
}; 

vector<node> mp[N];

int Prime(int s,int n)
{
	priority_queue<node> q;//队列：先进先出
	bitset<N> vis;
	vis[s]=1;
	for(auto e: mp[s])//遍历节点s的所有出边的终点
		q.emplace(e);
	
	ll ans=0;
	int cnt=0;	
	while(!q.empty())
	{
		auto[v,w]=q.top();
		q.pop();
		if(vis[v])
			continue;
		vis[v]=1;
		ans+=w;
		cnt++;
		if(cnt==n-1)
			return ans;
		for(auto e: mp[v])
			q.emplace(e);
        //第二次循环再看：由于优先队列的特性，由节点v加入的所有出边的终点还是会与节点u的所有出边的终点按照边权进行排序，所以每次取出来的都是当前状态下边权最小的那条出边所对应的终点
	}
	return -1;
}

int main()
{
	cin >> n >> m;
	for(int i=1;i<=m;i++)
	{
		int u,v;
		ll w;
		cin >> u >> v >> w;
		mp[u].emplace_back(v,w);
		mp[v].emplace_back(u,w);
	}
	ll ans=Prime(1,n);//可以从任意节点出发
	if(ans==-1)
		cout << "orz" << endl;
	else
		cout << ans << endl;
	return 0;
}
```



## 拓扑排序

用于具有先后依赖顺序的排序问题

```c++
//链式前向星version
#include    <bits/stdc++.h>

using namespace std;
const int N = 1e5 + 10;
const int M = 2e5 + 10;
struct node {
    int to, nxt;
} edge[M];
int indeg[N], ans[N], head[M];
int n, m, cnt = 0;

void addedge(int u, int v) {
    edge[++cnt].to = v;
    edge[cnt].nxt = head[u];
    head[u] = cnt;
}

void Tuopu() {
    queue<int> q;
    for (int i = 1; i <= n; i++) {
        if (indeg[i] == 0) {
            q.push(i);//将入读为0的点入队
            ans[i] = 1;//入度为0的点的结果为1
        }
    }

    while (!q.empty()) {
        int now = q.front();
        q.pop();//一定要记得出队！
        for (int i = head[now]; i; i = edge[i].nxt) {
            int to = edge[i].to;
            indeg[to]--;
            ans[to] = max(ans[to], ans[now] + 1);//节点to的结果取到它的节点的结果加1最大的那一个
            if (indeg[to] == 0)
                q.push(to);
        }
    }
}

int main() {
    cin >> n >> m;
    for (int i = 1; i <= m; i++) {
        int u, v;
        cin >> u >> v;
        addedge(u, v);
        indeg[v]++;
    }
    Tuopu();
    for (int i = 1; i <= n; i++)
        cout << ans[i] << endl;
    return 0;
}
//如果要判断是否所有元素都入队：https://www.luogu.com.cn/problem/P2712?contestId=77991
//====>设置ans变量，在进入while时使ans++,如果最后ans==n，则全员都考虑了
//还可以计算能够构成的链的数量===>继承：https://www.luogu.com.cn/problem/P4017?contestId=77991
```



# STL

## 输入输出cout cin

**cout保留两位小数**

```c++
cout << fixed << setprecision(2) << x << endl;
```



## vector

动态数组

一维：vector<int> a(n,0)	//0处为可以填写的初始化的值,#include<vector>

二维：

```c++
vector< vector<int> > a(n, vector<int> (m));//定义二维vector，初始内外层数组大小为n和m

vector<vector<pair<int, int> > > g(n + 1, vector<pair<int, int> >(m));//定义元素类型为pair的二维vector

vector<vector<int> > v;
v.resize(3);   //指定v有3行
for (int i = 0; i < v.size(); i++) {
    cin >> m;
    v[i].resize(m); //指定v每行的大小
}

for (int i = 0; i < 4; i++){  //向v里面添加四行
    cin >> m;
	v.push_back(vector<int>(m)); //往v3里添加行，行的大小为m
}
```

**vector求元素个数**

```c++
vector<int> a(n);
int cnt = count(a.begin(),a.end(),3);
```

vector的**find**函数

```c++
vector<int>::iterator it = find(a.begin(),a.end(),3);
if(it == a.end())
    cout << "Not Found" << endl;
else
    cout << "Successfully Found" << endl;
```

**unique函数**

去重原理是，比较当前元素a[i]与前面的一个元素a[i - 1]，如果相等，则继续将后面的元素(a[i + 1], a[i + 1] ……a[n]) 与a[i - 1]进行比较，直到找到一个元素a[j] 与a[i - 1]不相等，将这个元素a[j]放在a[i]的位置，之后再从a[j + 1]开始，与a[i]比较，如果相等则继续往后找，直到j == n，但是并不删除重复的元素。

```c++
vector<int>::iterator pos = unique(l, r);
//l为待去重的数组第一个元素的地址，r为待去重数组的最后一个元素的下一个地址
//返回去重后的数组的最后一个元素的下一个位置迭代器
```

```c++
vector<int> a{1, 2, 2, 3, 3, 4, 4, 5};
vector<int>::iterator pos = unique(a.begin(), a.end());

//1.输出去重排序后的原数组
for(int i = 0; i < a.size(); i++)	
    cout << a[i] << " ";
//1,2,3,4,5,4,4,5

//2.输出去重且排序后的数组
for(vector<int>::iterator i = a.begin(); i < pos; i++)
    cout << *i << " ";
//1,2,3,4,5
```



**erase函数**

```c++
vector<int> a;
a.erase(pos, n);//删除从pos开始的n个数[pos, pos + n,适用于string类型
a.erase(position);//输出position处的元素，position为迭代器
a.erase(first, last);//删除从[first, last)的所有元素，为迭代器
```

```c++
vector<int> a{1, 2, 3, 4, 5, 6, 7};
a.erase(a.begin() + 2, a.begin() + 4);
for (int i = 0; i < a.size(); i++)
    cout << a[i] << " ";
//1 2 5 6 7 
```

unique+erase实现数组**排序+去重**

```c++
vector<int> a;
for(int i = 0; i < n; i++)	a.push_back(a[i]);
sort(a.begin(),a.end());
a.erase(unique(a.begin(), a.end()), a.end());
```



## array

array在 C++ 普通数组的基础上，添加了一些成员函数和全局函数。在使用上，它比普通数组更安全，且效率并没有因此变差.

```c++
#include	<array>
array<int, n> a{};//定义一个长度为n，元素都初始化为0的array数组a
array<int, 4> b{1,2,3,4};
vector<array<int,3> > a(n, {1,2,3});//定义二维数组,每一行为一个array,每行都初始化为1,2,3
```



## **priority_queue**

**1.排序规则** 

**priority_queue<type, contain, compare>**

type是元素类型，contain是顺序容器， compare是排序规则 

1.当元素类型为简单类型时，**默认**排序规则为**降序**（大根堆）!!

2.当元素类型为复合类型时，如pair，默认按照pair的first进行降序排序，如果first相同则按照second进行降序排序

如果要按**升序**排序：

```c++
priority_queue<int, vector<int>, greater<int> > que;//队内元素为int类型的升序队列

priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>> >que;//队内元素为pair类型的升序队列
```

```c++
priority_queue<int> q;//定义

void clear(priority_queue<int> &q)//清空
{
	priority_queue<int> empty;
	swap(empty,q);
}

q.push(x);
q.pop();
q.top();//取队首
```

**注：**priority_queue<type, vector<type>, greater<type> > q

！！！！ type要保持一致

```c++
priority_queue<ll, vector<ll>, greater<ll>> q;//类型为long long，升序
```



2.**优先队列元素的遍历**：

因为优先队列元素遍历必须弹出所有元素，最终队列将为空，所以如果不想改变原队列中的内容，可以**创建队列副本**，对副本进行出队输出操作

```c++
priority_queue<int> q;
priority_queue<int> copy {q};

while(!copy.empty()){
    cout << copy.top() << " ";
    copy.pop();
}
```



## **set**

为了保持set容器内元素的有序性，用户不能对容器内的元素进行直接更改，要想达到更改元素的目的，正确的做法是将要修改的元素从容器中删除，然后再将修改后的元素加入到容器中

1. 创建空的set容器

```c++
set<int> myset;
```

2. 创建set容器的同时进行初始化

```c++
set<sint> myset={1,2,3,4};
set<string> str_set={"abc","def","ghi"};
```

3. 提供拷贝功能(将一个容器中的所有元素复制到另一个容器当中)

```c++
set<int> copyset=myset;
//set<int> copyset(myset);
```

4. 支持取已有set容器中的部分元素来初始化新的set容器

```c++
set<int> copyset<++myset.begin(),myset.end);
```

5. set默认升序【less<type>】，但是支持更改排序规则

```c++
set<int,greater<int> >;//降序
```

6. 常见用法

```c++
s.begin()     　返回set容器的第一个元素

s.end() 　　　　 返回set容器的最后一个元素

s.clear()       删除set容器中的所有的元素

s.empty() 　　　 判断set容器是否为空

s.insert()      插入一个元素

s.erase()       删除一个元素

s.size() 　　　　返回当前set容器中的元素个数

s.find()        查找一个元素，如果容器中不存在该元素，返回值等于s.end()

s.count()		返回元素在集合中出现的次数,由于set容器仅包含唯一元素，因此只能返回1或0

s.lower_bound() 返回第一个大于或等于给定关键值的元素

s.upper_bound() 返回第一个大于给定关键值的元素
```

7.set<pair<int,int> > 默认按照pair的first进行升序排序，然后按照second排序



## **map**

```c++
#include <iostream>
#include <map>      // map
#include <string>   // string

using namespace std;

int main() {
    //创建并初始化 map 容器
    map<string, string> myMap{{"S", "http"},
                              {"C", "bian"},
                              {"J", "net"}};
    string cValue = myMap["C"];//使用"[ ]"运算符
    cout << cValue << endl;
    return 0;
}
//output: bian
```

## pair

先按first比较，如果相等再根据second比较

## std::itoa 实现递增序列

(c++11新增)头文件：#include	<numeric>

```c++
template<class ForwardIterator, class T>
void itoa(ForwardIterator first, ForwardIterator last, T value){
    while(first != last){
        *first++ = value;
        ++value;
    }
}

//应用
vector<int> f;
int n = 100;
f.resize(n);
itoa(f.begin(), f.end(), 0);//value是初值

//相当于
for(int i = 0; i < n; i++)	f[i] = i;
```



## **lower_bound 和 upper_bound**

lower_bound(a+1,a+n+1,x)：计算”升序（不降序）“序列中第一个**大于等于x**的元素下标（元素下标从1开始）

lower_bound(a,a+n,x) （下标从0开始）

upper_bound(a+1,a+n+1,x)：计算”升序（不降序）“序列中第一个**大于x**的元素下标（元素下标从1开始）

注：两个函数都采用二分法的原理，因此当原序列不满足升序的特性时，虽然程序仍然能够运行，但是函数会默认序列为升序，仍然用二分法计算，得到结果很有可能是错的。



## 全排列

**1. next_permutation(start, end)**

1）修改数组为**当前序列**的后一个序列。一般在使用前都要对原序列进行**排序**，否则得到的时当前序列之后的序列；

2）**返回值**：如果当前序列的下一个序列存在，则返回true，反之返回false；

3）也可对string类型进行排序（默认按字典序）,可对结构体进行排序（用cmp函数自定义排序规则）

**2. prev_permutation(start, end)**

修改数组为当前序列的前一个序列

```c++
void permutate() {
    //1.
    int a[3] = {1, 5, 4};
    do {
        cout << a[0] << " " << a[1] << " " << a[2] << endl;
    } while (next_permutation(a, a + 3));
    
    //2.
    int b[3] = {2, 3, 1};
    do {
        cout << b[0] << " " << b[1] << " " << b[2] << endl;
    } while (next_permutation(b, b + 3));
    
    //3.
    string s = "abc";
    do {
        cout << s << endl;
    } while (next_permutation(s.begin(), s.end()));
}

//输出：
//1.
1 2 3
1 3 2
2 1 3
2 3 1
3 1 2
3 2 1
    
//2.    
2 3 1
3 1 2
3 2 1
    
//3.
abc
acb
bac
bca
cab
cba
```



## **unique**

包含在头文件：#include <algorithm>

```c++
iterator unique(iterator it_1,iterator it_2);
```

该函数的作用：**“去除”**容器或者数组中区间[it_1，it_2)中**相邻**的重复出现的元素

(**注：区间是前闭后开，即不包含it_2所指的元素**)

(1) “去除”：将重复的元素放到容器的末尾，返回值是一个迭代器去，指向去重后容器中不重复序列的最后一个元素的下一个元素

(2) 针对的是相邻元素，所以对于顺序顺序错乱的数组成员，或者容器成员，需要先调用sort()进行排序



# 数学

## 最大公因子 gcd

更相减损术，辗转相除法gcd(a, b) == gcd(b, a % b)

```c++
ll gcd(ll a,ll b)
{
	if(b == 0)
        return a;
   	else
        return gcd(b, a % b);
}
```



## 最小公倍数 lcm

两个整数的最小公倍数与最大公因数之间有如下的关系：

```c++
ll lcm(ll a, ll b) 
{
    return a * b / gcd(a, b);
}
```



## 分解质因数

```c++
ll f[N];//存储n的所有质数因子
ll num[N];//每个质因子的幂次
int cnt = 0;//质因子个数，从下标0开始访问

void dcmp(ll n) {
    for (int i = 2; i * i <= n; i++) {
        if (n % i == 0) {
            f[cnt] = i;
            int s = 0;//每个质因子的指数
            while (n % i == 0) {
                s++;
                n /= i;
            }
            num[cnt++] = s;
        }
    }
    if (n > 1) f[cnt] = n, num[cnt++] = 1;
}
```



## 判断一个数是否为2的幂次

```c++
bool power2(ll n) {
    return (n & (n - 1)) == 0;
}
```



## 计算组合数

**way1：递归**

```c++
int C[N][N] = {0};

int com(int n, int m) { 
    if (m == 0 || m == n) return 1;
    else if (C[m][n] != 0) return C[m][n];
    else return C[m][n] = com(n - 1, m) + com(n - 1, m - 1);
}
```

**way2：边乘边除，公式转换**

```c++
int com2(int n, int m) {
    int ans = 1;
    for (int i = 1; i <= m; i++)
        ans = ans * (n - m + i) / i;//一定要先乘再除
    return ans;
}
```



# 一些常用函数

## 取整函数

### 1. ceil(double x) 

用于求不小于 x 的最小整数（向上取整）

返回值：不小于 x 的最小整数。

### 2. floor(double x)

求不大于x的最大整数（向下取整）



## **log函数**

```c++
log(a)	//以e为底，a为真数的对数
log2(a)	//以2为底
log10(a)//以10为底
log(n)/log(m)//以m为底，n为真数的对数
```



## 快速乘

如果模数大于2^32说明取模也无法防止溢出

注：在快速幂中的乘法也要使用快速乘

```c++
ll ksc(ll a, ll b) {
    ll ans = 0;
    while (b) {
        if (b & 1)
            ans = (ans + a) % mod;
        a = (a + a) % mod;
        b >>= 1;
    }
    return ans;
}
```

O(1)（用于特殊卡常数）

```c++
ll mul(ll a, ll b, ll mod) {
    return (a * b - (ll)((long double) a / mod * b) * mod + mod) % mod;
}
```



## 快速幂

```c++
ll ksm(int a, int n){
    ll ans = 1;
    while(n)
    {
        if(n & 1)        //如果n的当前末位为1
            ans = (ans * a) % mod ;  //ans乘上当前的a
        a = (a * a) % mod;        //a自乘
        n >>= 1;       //n往右移一位
    }
    return ans;
}
```



### 应用：求a在模mod下的乘法逆元

设a,p互质（即gcd(a,p) = 1)，则必定存在数b，使得


$$
ab \equiv 1 (mod p)
$$
**则称**b为a在模p下的乘法逆元。所以 除以a 就相当于 乘以a的乘法逆元b。

又由费马小定理可知：
$$
a \equiv a^p
$$



```c++
int mod = 1e9 + 7;//全局

ll ksm(int a, int n){
    ll ans = 1;
    while(n)
    {
        if(n & 1)        //如果n的当前末位为1
            ans = (ans * a) % mod ;  //ans乘上当前的a
        a = (a * a) % mod;        //a自乘
        n >>= 1;       //n往右移一位
    }
    return ans;
}

ll rev(int a)
{
	return ksm(a, mod - 2);
}
```



# 交互题



```c++
#include <bits/stdc++.h>

using namespace std;
typedef long long ll;
const int N = 105;
int prime[N];
int isnprime[N];
int cnt = 0;

//素数筛,找出[2,100]所有的素数
void E_sizve(ll n) {
    for (int i = 2; i * i <= n; i++) {
        if (isnprime[i] == 0) {
            for (int j = i * i; j <= n; j += i) {
                isnprime[j] = 1;
            }
        }
    }
    for (int i = 2; i <= n; i++) {
        if (!isnprime[i]) {
            prime[cnt++] = i;
        }
    }
}

bool query(int x) {
    cout << x << endl;
    string s;
    cin >> s;
    if (s == "yes")
        return 1;
    else
        return 0;
}

void solve() {
    E_sizve(50);
    int fnum = 0;
    int factor;
    for (int i = 0; i < 15; i++) {
        if (query(prime[i])) {
            fnum++;
            factor = prime[i];
        }
    }
    if (fnum >= 2) {
        cout << "composite" << endl;
    } else {
        if (factor > 10)
            cout << "prime" << endl;
        else {
            if (query(factor * factor))
                cout << "composite" << endl;
            else
                cout << "prime" << endl;
        }
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    int tt = 1;
    //cin >> tt;
    while (tt--) {
        solve();
    }

    return 0;
}
```



# 位运算

**a | b  + a & b = a + b**

**a ^ b ^ a = b**

**a ^ b + 2 * (a & b) = a + b**



# 贪心

## 活动选择问题（线段覆盖）

**题意：**

有一块体育场，现有n个班需要在这块体育场上课，第i个班的课程开始时间为s[i]结束时间为e[i]，问：如何安排课程才能够使体育场的利用率达到最大？(也就是能上课的班的数量最多)

注：n个班不一定全都能上课

**分析：**

贪心地想要使上课的班的数量更多，那么可以对每个班**按照结束时间排序，因为这样可以给后面的班预留出更多时间，从而使上课的班的数量更多**。

```c++
int n;
typedef struct node{
    int s, e;
}node;

node a[n];

bool cmp(node p, node q){
    return p.e < q.e;
}
int main(){
	for(int i = 0; i < n; i++)	cin >> a[i].s >> a[i].e;
    int end = a[0].e, cnt = 1;
    for(int i = 1; i < n; i++){
        if(a[i].s >= end){
            cnt++;
            end = a[i].e;
        }
    }
    return 0;
}
```

