`deque`：双端队列，头尾插入删除很快。支持随机访问
`vector`：尾部之外的位置插入删除很慢。支持随机访问
`list`：双向链表。只支持顺序访问，任何位置插入元素都很快。
`forward_list`:单向链表，仅支持单向顺序访问。
`array`：固定大小数组，支持快速随机访问。

**list不能用下标操作，只能用迭代器。所以就用公共操作，迭代器咯。**

### 容器操作：（所有容器都可以用）

````c++
类型别名：
    size_type
    iterator
    value_type
    
大小：
    c.size()	//
    c.empty()	//
    
添加删除元素：
    c.insert(args)  args：可以是一个迭代器	/	一个迭代器范围	/	
    a.insert(a.begin(),2);
    c.erase(args)
    c.clear()//也常用

容器的初始化：
    C c (b,e)//b和e是迭代器。要求：范围中元素类型和c的元素类型相容。比如：const char*可以转换为string
    C c ={，，，};//更多的用列表初始化
	c1.swap(c2)
    C seq(n,t):只有顺序容器才能用这种初始化方法。且array不能用
        
除了无序关联容器外的所有容器都支持比较运算符。实际是进行逐队比较。都相同再比较大小。
````

### 顺序容器操作：

````c++

    c.push_back()
    c.push_front()       //vector/string不支持。用这个就好：       .insert(.begin(),"hello");
    c.insert(p,t)  //像p指向的元素  ！之前！  插入一个值为t的元素。    返回值：指向新添加的元素的迭代器。
    c.insert(p,n,t)//插入n个值为t                                 返回值：指向新添加的第一个元素的迭代器。
    c.insert(p,b,e)//在p指向元素之前，插入迭代器所指元素。		    返回值：指向新添加的第一个元素的迭代器。
    c.insert(p,i1) //插入花括号{}元素。比较常用                     返回值：指向新添加的第一个元素的迭代器。
    
    访问元素：
    成员函数：front back.     array无front  forward_list无back
    返回的是首元素和尾元素的引用，比如：auto value = c.front();  或者：  auto &value = c.front();
	c[n]:同样也是返回下标为n的元素的引用、  c.at[n]也是
    
    删除元素：
        c.pop_back():返回void
        c.pop_front():返回void
        c.erase(p):       返回值：指向删除的（最后一个）元素之后位置的迭代器。比如 i,j，。。。那么删除i，返回的是j
        c.erase(b,e)//都是迭代器
        c.clear()
        
额外的string操作：p323.处理字符串的
        apend:末尾插入操作简写
        replace:erase+insert
        find:
		find_first_of:  find_last_of:
````

###### String：p323

### stack queue 优先队列

````C++
几个老熟人还能用：
    size_type
    value_type
    a.empty()
    a.size()
    swap(a,b)
    
默认情况：stack和queue；用deque实现的。    priority_queue:用vector
    可以重载参数类型：stack<string , vector<string>>用第二个参数：比如这里选择了vector！！举个例子！
    
    栈的操作：
    	s.pop():删除栈顶元素，但不返回该元素值
    	s.push(item)
    	s.top():返回栈顶元素
    队列操作：
    	q.pop()：删除元素但不返回元素。
    	q.front():返回首元素，但不删除元素。不适用于优先队列！
    	q.back():只适用于 queue ，返回尾巴元素。优先队列没有！
    
    	q.top():只适用于priority_queue:返回最高优先级元素
    	q.push(item):添加元素。queue的末尾 或者 priority_queue的适当位置。
    
    
````

#### 优先队列补充：

和其他类型一样，使用的时候：**priority_queue<结构类型> 队列名;**

基本操作：和上面说的一样~

````c++
q.size();//返回q里元素个数
q.empty();//返回q是否为空，空则返回1，否则返回0
q.push(k);//在q的末尾插入k
q.pop();//删掉q的第一个元素
q.top();//返回q的第一个元素


//问题来了？怎么访问优先队列的元素呢。。基本功要有。包括队列，栈也是这样。 用一个元素temp=c.top()！
访问优先队列的元素：用pair
 比如:pair<int,char> temp=arr.top();
````



最常用的：

````C++
priority_queue <node> q;
//node是一个结构体
//结构体里重载了‘<’小于符号
priority_queue < int,vector<int>,greater<int> > q; //重载了参数类型。priority默认的也是vector。
//注意后面两个“>”不要写在一起，“>>”是右移运算符
priority_queue <int,vector<int>,less<int> >q;
````



**举个例子：**

````C++
priority_queue <int> q;
int main()
{
	q.push(10),q.push(8),q.push(12),q.push(14),q.push(6);
	while(!q.empty())
		printf("%d ",q.top()),q.pop();
}
输出： 14 12 10 8 6  ->说明他默认从大到小排序!!!在系统默认的＜号下，她是这样操作的。
    
#include<cstdio>
#include<queue>
using namespace std;
struct node
{
	int x,y;
	bool operator < (const node & a) const
	{
		return x<a.x;
	}
}k;
priority_queue <node> q;
int main()
{
	k.x=10,k.y=100; q.push(k);
	k.x=12,k.y=60; q.push(k);
	k.x=14,k.y=40; q.push(k);
	k.x=6,k.y=80; q.push(k);
	k.x=8,k.y=20; q.push(k);
	while(!q.empty())
	{
		node m=q.top(); q.pop();
		printf("(%d,%d) ",m.x,m.y);
	}
}

程序大意就是插入(10,100),(12,60),(14,40),(6,20),(8,20)这五个node。
再来看看它的输出：
(14,40) (12,60) (10,100) (8,20) (6,80)

它也是按照重载后的小于规则，从大到小排序的。
↑好好康康这句话！（摘眼镜）！！！小于规则默认从大到小排序

    
    
声明的时候可以这样写！
注意的地方：必须有 vector<int>  而且>> 中间必须有空格！    
priority_queue <int,vector<int>,less<int> > p;
priority_queue <int,vector<int>,greater<int> > q;
int a[5]={10,12,14,6,8};
int main()
{
	for(int i=0;i<5;i++)
		p.push(a[i]),q.push(a[i]);
		
	printf("less<int>:")；
	while(!p.empty())
		printf("%d ",p.top()),p.pop();	
		
	printf("\ngreater<int>:")；
	while(!q.empty())
		printf("%d ",q.top()),q.pop();
}
less<int>:14 12 10 8 6 greater<int>:6 8 10 12 14
    总结->less是从大到小，greater是从小到大.看我们需要哪一种。
    
    
    
可能也会用！自己构建规则~
1. 先看sort：按不同的方式排序！更好用的方法还是用lambda比较好。
    struct node
{
	int fir;
    int sec;
}arr[2030];

bool cmp1(node x,node y)
{
	return x.fir<y.fir;  //当一个node x的fir值小于另一个node y的fir值时，称x<y
}
bool cmp2(node x,node y)
{
	return x.sec<y.sec;  //当一个node x的sec值小于另一个node y的sec值时，称x<y
}
bool cmp3(node x,node y)
{
	return x.fir+x.sec<y.fir+y.sec;  //当一个node x的fri值和sec值的和小于另一个node y的fir值和sec值的和时，称x<y
}

int main()
{
	scanf("%d",&n);
	for(int i=1;i<=n;i++) scanf("%d %d",&arr[i].fir,&arr[i].sec);
	
	puts("\n--------------------");
	sort(arr+1,arr+1+n,cmp1); for(int i=1;i<=n;i++) printf("%d. {%d %d}\n",i,arr[i].fir,arr[i].sec);
}

	puts("\n--------------------");
	sort(arr+1,arr+1+n,cmp2); for(int i=1;i<=n;i++) printf("%d. {%d %d}\n",i,arr[i].fir,arr[i].sec);
}

	puts("\n--------------------");
	sort(arr+1,arr+1+n,cmp3); for(int i=1;i<=n;i++) printf("%d. {%d %d}\n",i,arr[i].fir,arr[i].sec);
}
但是优先队列可没有sort那么灵活想用什么作小于规则用什么作小于规则，它只会用一个固定的小于规则。
    
    
    
    
    
    
所以如果想把一个队列按不同的方式优先，就要：
2. 看一下queue  //input.read()是调用成员函数的方法，要不然也不知道调用到哪里去了！然后push(input)!!注意是如何使用的哈哈哈
    
using namespace std;

int n;
struct node
{
	int fir,sec;
	void Read() {scanf("%d %d",&fir,&sec);}
}input;

struct cmp1
{
	bool operator () (const node &x,const node &y) const
	{
		return x.fir<y.fir;
	}
};//当一个node x的fir值小于另一个node y的fir值时，称x<y

struct cmp2
{
	bool operator () (const node &x,const node &y) const
	{
		return x.sec<y.sec;  
	}
};//当一个node x的sec值小于另一个node y的sec值时，称x<y

struct cmp3
{
	bool operator () (const node &x,const node &y) const
	{
		return x.fir+x.sec<y.fir+y.sec; 
	}
};//当一个node x的fri值和sec值的和小于另一个node y的fir值和sec值的和时，称x<y
调用的时候：
priority_queue< node , vector<node> ,cmp1> q1;
priority_queue< node , vector<node> ,cmp2> q2;
priority_queue< node , vector<node> ,cmp3> q3;


上面的方法很好，和下面的方法一样！用的时候直接在public里面定义struct就好了~
class Solution {
public:
    struct cmp
	{
		bool operator()(pair<int,char> a,pair<int,char> b)
		{
			return a.first<b.first;    
		}      
	};
	priority_queue<pair<int,char>,vector<pair<int,char>>,cmp> arr;
````



### 泛型算法：

> 就是通用的，可以用于不同的容器不同类型的元素。

泛型算法依赖于元素类型，比如：accumulate函数：可以用于string（因为有定义加法）但是不能用于char*

还有back_insert:

###### 重排元素的算法：sort

**sort(.begin(),.end())**    **接受二元谓词**：**stable_sort(words.begin(),words.end(),isShorter);  isShorter是一个两个参数的函数**
稳定就用：**stable_sort**

````c++
		//sort默认升序。1 2 3 4 5 6 
		//grater:从大到小，递减。6 5 4 3 2 1。和优先队列正好反过来了。
		sort(a.begin(),a.end());
		sort(b.begin(),b.end(),greater());

sort：自己构建sort的时候的方法！！用lambda，别用传函数的哪种方法不太方便。
比如：我们想按first降序排列！
vector<pair<int, char>> arr = {{a, 'a'}, {b, 'b'}, {c, 'c'}};
只需要：
sort(arr.begin(),arr.end(),[](pair<int,char> & p1,pair<int,char> & p2){return p1.first > p2.first;})  ;
````



**unique:**使不重复的元素出现在开头部分，后面是什么不知道。函数返回：一个指向不重复范围末尾的迭代器。



定制操作：有点类似于之前写的cmp函数。见p345；。

本章内容还有lambda表达式和bind参数绑定。大概格式：

###### lambada：

**[捕获列表]（参数列表）->return type {function body}**

参数列表和returntype往往可以省略。捕获列表可以用  **[&](表示引用   和 [=](表示传递值**，不用具体写出局部变量



### 关联容器：

一般如果没有顺序要求，或者重复的要求就用：**unordered_map和unorderd_set.否则加上multi**

下面的这些是通用的。

````c++
类型别名：
    size_type
    iterator
    value_type
    
大小：
    c.size()
    c.empty()
    
添加删除元素：
    c.insert(args)  args：可以是一个迭代器/一个迭代器范围/
    c.erase(args)
    c.clear()//也常用
    
容器的初始化：
    C c (b,e)//b和e是迭代器。要求：范围中元素类型和c的元素类型相容。比如：const char*可以转换为string
    C c ={，，，};//更多的用列表初始化
//比如：
unordered_multimap<int,char> a={{1,'a'},{2,'b'}};

	c1.swap(c2)
    C seq(n,t):只有顺序容器才能用这种初始化方法。且array不能用
        
除了无序关联容器外的所有容器都支持比较运算符。实际是进行逐队比较。都相同再比较大小。
````

###### map和set：

比较常用的：如果下标未在map中，则会自动创建。

每一个map是一个pair类型，用first和second来分别访问关键字和词。比如：

````c++
cout<<w.first<<w.second<<endl

//find函数也常用：
    if(w.find(word)==w.end())//这种情况说明没有找到

//map列表初始化：
    map<string,string> authors  = {{"a","books"},{"c","books"},{"e","books"}};//fisrt是关键字
    //还有这种初始化：
    vector<int> ivec;
    set<int> iset   (ivec.begin(),ivec.end());//元素类型需要相同
    //关键字类型的要求：必须要严格弱序。见p379
    //pair类型：
    pair<string,string> a = {"a","c"};//就和我们前面map的成员是一样的。

    //关联容器操作：对于map
    auto map_it = authors.begin();  
    //使用方法：
    cout<<map_it->first;
    cout<<map_it->second;
    ++map_it->second;//这个也挺常用哈哈哈，不过这里string不能++
    ++map_it;//这个也比较常用
    //如果不用auto：那就要用：
   Q map<string,string> ::iterator map_it = authors.begin();

    //关联容器的insert操作：返回值：返回一个pair，first成员是一个迭代器，指向插入元素。second显示插入是否成功。如果关键字不存在，则说明可以插入，返回true
    //但是multi就直接返回迭代器了，因为一定插入成功。
    c.insert(v);//插入一个value_type型的元素。
    c.insert(b,e);//插入一个迭代器范围。
    c.insert(il);//插入花括号，这个最常用！！！最简单。比如插入一个pair元素：c.insert({v1,v2});
    c.insert(p,v);//具体作用不太懂，见p384。
    举个例子：
    word_count.insert({word,1});//还是花括号的方法最简单
    word_count.insert(make_pair(word,1));//让其为pair类型。
    word_count.insert(pair<string,size_t>(word,1));//zhe'yang
    word_count.insert(map<string,size_t>::value_type (word,1));

    //删除元素：erase(元素或者是迭代器)!往往用迭代器更省时间。具体删除的时候要根据是否是multi来选择。
	//其次如果我们是先调用的find函数~已经找到了，直接把find的迭代器传进去就好。就不要用元素了，节省时间

    先看如果是元素：
    word_count.erase("hello");//返回值是实际删除元素的数量，map就是0和1，有multi可能是一下子删除了好几个。
    再看如果是迭代器：
    word_count.erase(p);//返回一个指向p后面的迭代器！
    word_count.erase(b,e);//迭代器范围，返回e
    map下标操作妙用：word_count["hello"]=1;

    访问元素：
    find
    c.find(k);//返回值为第一个关键字为k的元素的迭代器
    c.count(k);//返回关键字＝k的数量！multi可能是很多个。
    c.lower_bound(k);//返回第一个关键字不小于k的元素
    c.upper_bound(k);//返回第一个关键字不大于k的元素
    c.equal_range(k);返回一个pair，表示指向k的元素的范围

    对map用find代替下标操作！因为下标操作可能会自动创建map。
    p390：如何在multi里面查找元素
        
    //遍历map的方法：作为意识！结构绑定C++17标准。这个方法可以用来访问map里面的元素。同样，->first / -> second /->下摆奥访问也可以。
        for (auto &[num, c] : cnt) {
            if (c == 1) {
                ans += num;
            }
        }

map和set会根据关键字的 key （而不是关键字的值！！） 自动排序：
map<int,int,less<int> > mp; //按key升序。1 2 3 4 5 6（默认，和sort一样，和优先对联相反）
map<int,int,greater<int> > mp; //按key降序.6 5 4 3 2 1 


//map和set函数有专有的find和count，也就是调用的时候不用按照(begin,end,value)的格式。直接.find()这样效率更高。但不知道有没有专用的find_if函数。
    find：返回一个迭代器，指向end
    count：返回关键字等于k的元素的数量。对于不允许重复的map和set，返回值永远是1或0。
    启示：不要随便用下标，会出问题的。
	1.尤其是范围for循环里面，写成cnt[k]的形式可能会添加一项。是范围for循环失效。
    2.有迭代器的循环也不能添加/删除元素！
    此时要借助count：下面这种写法是对的！！！不能直接访问cnt[num+k]!
            if(cnt.count(num+k)==1){
                ans+=count*cnt[num+k]; 
            }
            if(cnt.count(num-k)==1){
                ans+=count*cnt[num-k];
            }

````

###### 访问map：

1. 结构绑定
2. 下标访问/小心创建。可以先`find`或者`count`一下
3. 用find取出来之后用->`second` 和 ->`firsst`。可以用pair作为临时变量
4. 每一个map是一个pair类型，用first和second来分别访问关键字和词。比如：`cout<<w.first<<w.second<<endl`

###### map遍历：

```C++
    //第一种
    for(auto &t : m){
        cout<<t.first<<t.second<<endl;	//用 '.' 来访问。
    }
    //第二种
    for(iter = m.begin(); iter != m.end(); ++iter){
        cout<<iter->first<<iter->second<<endl;
    }
    //第三种
    while(iter != m.end()){
        cout<<iter->first<second<<endl;
        ++iter;
    }
	//引用绑定：
        for (auto &[num, c] : cnt) {
            if (c == 1) {
                ans += num;
            }
        }
```



关于count函数：

### 结构体/类里面构造函数：



