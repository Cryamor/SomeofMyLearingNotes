# STL

## STL序列式容器

### vector

#### 创建

```cpp
std::vector<double> values;
value.reserve(20);
std::vector<int> primes {2, 3, 5, 7, 11, 13, 17, 19};
std::vector<double> values(20); // 默认0
std::vector<double> values(20, 1.0); // 默认1.0
// 可以变量
int num=20;
double value =1.0;
std::vector<double> values(num, value);

std::vector<char>value1(5, 'c');
std::vector<char>value2(value1);

int array[]={1,2,3};
std::vector<int>values(array, array+2);//values 将保存{1,2}
std::vector<int>value1{1,2,3,4,5};
std::vector<int>value2(std::begin(value1),std::begin(value1)+3);//value2保存{1,2,3}
```

#### 迭代器

![2024-03-30-14-06-40-image.png](/Users/zmc/Documents/LessonNotes/SomeofMyLearingNotes/C:C++/assets/2024-03-30-14-06-40-image.png)

#### 访问

```cpp
#include <iostream>
#include <vector>
using namespace std;
int main()
{
    vector<int> values{1,2,3,4,5};
    //获取容器中首个元素
    cout << values[0] << endl;
    //修改容器中下标为 0 的元素的值
    values[0] = values[1] + values[2] + values[3] + values[4];
    cout << values[0] << endl;

    cout << values.at(0) << endl;
    //修改容器中下标为 0 的元素的值
    values.at(0) = values.at(1) + values.at(2) + values.at(3) + values.at(4);
    cout << values.at(0) << endl;
    //下面这条语句会发生 out_of_range 异常
    //cout << values.at(5) << endl;

    cout << "values 首元素为：" << values.front() << endl;
    cout << "values 尾元素为：" << values.back() << endl;
    //修改首元素
    values.front() = 10;
    cout <<"values 新的首元素为：" << values.front() << endl;
    //修改尾元素
    values.back() = 20;
    cout << "values 新的尾元素为：" << values.back() << endl;
    
    //输出容器中第 3 个元素的值
    cout << *(values.data() + 2) << endl;
    //修改容器中第 2 个元素的值
    *(values.data() + 1) = 10;
    cout << *(values.data() + 1) << endl;
    
    return 0;
}
```

#### 尾插&遍历

```cpp
#include <iostream>
#include <vector>
using namespace std;
int main()
{
    //初始化一个空vector容量
    vector<char>value;
    //向value容器中的尾部依次添加 S、T、L 字符
    value.push_back('S');
    value.push_back('T');
    value.push_back('L');
    //调用 size() 成员函数容器中的元素个数
    printf("元素个数为：%d\n", value.size());
    //使用迭代器遍历容器
    for (auto i = value.begin(); i < value.end(); i++) {
        cout << *i << " ";
    }
    cout << endl;
    //向容器开头插入字符
    value.insert(value.begin(), 'C');
    cout << "首个元素为：" << value.at(0) << endl;
    return 0;
}
```

`emplace_back()` 和 `push_back()` 的区别在于底层实现的机制不同。`push_back()` 向容器尾部添加元素时，首先会创建这个元素，然后再将这个元素拷贝或者移动到容器中（如果是拷贝的话，事后会自行销毁先前创建的这个元素）；而 `emplace_back()` 在实现时，则是直接在容器尾部创建这个元素，省去了拷贝或移动元素的过程。

#### 插入

```cpp
#include <iostream> 
#include <vector> 
#include <array> 
using namespace std;
int main()
{
    std::vector<int> demo{1,2};
    //第一种格式用法
    demo.insert(demo.begin() + 1, 3);//{1,3,2}
    //demo.emplace(demo1.begin() + 1, 3);
    //第二种格式用法
    demo.insert(demo.end(), 2, 5);//{1,3,2,5,5}
    //第三种格式用法
    std::array<int,3>test{ 7,8,9 };
    demo.insert(demo.end(), test.begin(), test.end());//{1,3,2,5,5,7,8,9}
    //第四种格式用法
    demo.insert(demo.end(), { 10,11 });//{1,3,2,5,5,7,8,9,10,11}
    for (int i = 0; i < demo.size(); i++) {
        cout << demo[i] << " ";
    }
    return 0;
}
```

#### 删除

```cpp
#include <vector>
#include <iostream>
#include <algorithm>
using namespace std;

int main()
{
    vector<int>demo{ 1,2,3,4,5 };
    demo.pop_back();
    auto iter = demo.erase(demo.begin() + 1);//返回一个指向删除元素所在位置下一个位置的迭代器
    auto iter = demo.erase(demo.begin()+1, demo.end() - 2);   
    demo.clear();

    vector<int>demo{ 1,3,3,4,3,5 };
    auto iter = std::remove(demo.begin(), demo.end(), 3); 
    cout << "size is :" << demo.size() << endl;
    cout << "capacity is :" << demo.capacity() << endl;
    demo.erase(iter, demo.end()); 

    return 0;

```

`remove()` 的实现原理是，在遍历容器中的元素时，一旦遇到目标元素，就做上标记，然后继续遍历，直到找到一个非目标元素，即用此元素将最先做标记的位置覆盖掉，同时将此非目标元素所在的位置也做上标记，等待找到新的非目标元素将其覆盖。因此，如果将上面程序中 demo 容器的元素全部输出，得到的结果为 `1 4 5 4 3 5`。



### deque

和 vector 不同的是，deque 还擅长在序列头部添加或删除元素，所耗费的时间复杂度也为常数阶`O(1)`。并且更重要的一点是，deque 容器中存储元素并不能保证所有元素都存储到连续的内存空间中。



### list

又称双向链表容器，即该容器的底层是以双向链表的形式实现的。这意味着，list 容器中的元素可以分散存储在内存空间里，而不是必须存储在一整块连续的内存空间中。



## STL关联式容器

### pair

```cpp
#include <iostream>
#include <utility>      // pair
#include <string>       // string
using namespace std;
int main() {
    pair <string, double> pair1;
    pair <string, string> pair2("key","value");  
    pair <string, string> pair3(pair2);
    //调用移动构造函数
    pair <string, string> pair4(make_pair("k", "v")); 
   
    cout << "pair1: " << pair1.first << " " << pair1.second << endl;
    cout << "pair2: "<< pair2.first << " " << pair2.second << endl;
    cout << "pair3: " << pair3.first << " " << pair3.second << endl;
    cout << "pair4: " << pair4.first << " " << pair4.second << endl;
   
    return 0;
}
```

### map

- 默认情况下，map 容器选用`std::less<T>`排序规则（其中 T 表示键的数据类型），根据键的大小对所有键值对升序排序，也可以手动指定 map 容器的排序规则（`std::greater<T>` 或自定义）。

- 键的值既不能重复也不能被修改。

- map 容器配备的是双向迭代器（bidirectional iterator）。这意味着，map 容器迭代器只能进行 ++p、p++、--p、p--、*p 操作，并且迭代器之间只能使用 == 或者 != 运算符进行比较。

#### 迭代器&遍历

```cpp
#include <iostream>
#include <map>      // pair
#include <string>       // string
using namespace std;
int main() {
    //创建并初始化 map 容器
    std::map<std::string, std::string>myMap{ {"1","2"},{"2","3"} };
    //调用 begin()/end() 组合，遍历 map 容器
    for (auto iter = myMap.begin(); iter != myMap.end(); ++iter) {
        cout << iter->first << " " << iter->second << endl;
    }
    return 0;
}
```

map 类模板中还提供了 `find()` 成员方法，查找指定 key 值的键值对，如果成功找到，则返回一个指向该键值对的双向迭代器；反之，其功能和 `end()` 方法相同。

#### `[]`运算符

当 map 容器中存有包含该指定键的键值对，重载的 `[ ]` 运算符才能成功获取该键对应的值；反之，若当前 map 容器中没有包含该指定键的键值对，则此时使用 `[ ]` 运算符将变成向该 map 容器中增添一个键值对。







### set

set 容器类模板中未提供 `at()` 成员函数，也未对 `[]` 运算符进行重载。因此，要想访问 set 容器中存储的元素，只能借助 set 容器的迭代器。

```cpp
for (auto iter = myset.begin(); iter != myset.end(); ++iter) {
    cout << *iter << endl;
}
```





## STL无序容器（哈希容器）

### unordered_map





### unordered_set





## STL容器适配器



## STL迭代器适配器



## 常用算法

### sort()

`sort()` 函数位于`<algorithm>`头文件中，默认以元素值的大小做升序排序，除此之外也可以选择标准库提供的其它排序规则（比如`std::greater<T>`降序排序规则），也可以自定义排序规则。

仅适用于普通数组和部分类型的容器：

1. 容器支持的迭代器类型必须为随机访问迭代器。这意味着，sort() 只对 array、vector、deque 这 3 个容器提供支持。
2. 如果对容器中指定区域的元素做默认升序排序，则元素类型必须支持`<`小于运算符；同样，如果选用标准库提供的其它排序规则，元素类型也必须支持该规则底层实现所用的比较运算符；
3. sort() 函数在实现排序时，需要交换容器中元素的存储位置。这种情况下，如果容器中存储的是自定义的类对象，则该类的内部必须提供移动构造函数和移动赋值运算符。

对于指定区域内值相等的元素，sort() 函数无法保证它们的相对位置不发生改变。

平均时间复杂度为`O(N*logN)`（其中 N 为指定区域 `[first, last)` 中 last 和 first 的距离）。

两种语法格式：

```
//对 [first, last) 区域内的元素做默认的升序排序
void sort (RandomAccessIterator first, RandomAccessIterator last);
//按照指定的 comp 排序规则，对 [first, last) 区域内的元素进行排序
void sort (RandomAccessIterator first, RandomAccessIterator last, Compare comp);
```

e.g.

```cpp
#include <iostream>     // std::cout
#include <algorithm>    // std::sort
#include <vector>       // std::vector
//以普通函数的方式实现自定义排序规则
bool mycomp(int i, int j) {
    return (i < j);
}
//以函数对象的方式实现自定义排序规则
class mycomp2 {
public:
    bool operator() (int i, int j) {
        return (i < j);
    }
};

int main() {
    std::vector<int> myvector{ 32, 71, 12, 45, 26, 80, 53, 33 };
    //调用第一种语法格式，对 32、71、12、45 进行排序
    std::sort(myvector.begin(), myvector.begin() + 4); //(12 32 45 71) 26 80 53 33
    //调用第二种语法格式，利用STL标准库提供的其它比较规则（比如 greater<T>）进行排序
    std::sort(myvector.begin(), myvector.begin() + 4, std::greater<int>()); //(71 45 32 12) 26 80 53 33
   
    //调用第二种语法格式，通过自定义比较规则进行排序
    std::sort(myvector.begin(), myvector.end(), mycomp2());//12 26 32 33 45 53 71 80
    //输出 myvector 容器中的元素
    for (std::vector<int>::iterator it = myvector.begin(); it != myvector.end(); ++it) {
        std::cout << *it << ' ';
    }
    return 0;
}
```

### stable_sort()

除了可以实现排序，还可以保证不改变相等元素的相对位置。

当可用空间足够的情况下，该函数的时间复杂度可达到`O(N*log2(N))`；反之，时间复杂度为`O(N*log2(N)^2)`，其中 N 为指定区域 `[first, last)` 中 last 和 first 的距离。



### partial_sort()

部分排序，具体来说，`partial_sort()` 会将 [first, last) 范围内最小（或最大）的 middle-first 个元素移动到 [first, middle) 区域中，并对这部分元素做升序（或降序）排序。

用法：

```
//按照默认的升序排序规则，对 [first, last) 范围的数据进行筛选并排序
void partial_sort (RandomAccessIterator first,
                   RandomAccessIterator middle,
                   RandomAccessIterator last);
//按照 comp 排序规则，对 [first, last) 范围的数据进行筛选并排序
void partial_sort (RandomAccessIterator first,
                   RandomAccessIterator middle,
                   RandomAccessIterator last,
                   Compare comp);
```

e.g.

```cpp
#include <iostream>     // std::cout
#include <algorithm>    // std::partial_sort
#include <vector>       // std::vector
using namespace std;
//以普通函数的方式自定义排序规则
bool mycomp1(int i, int j) {
    return (i > j);
}
//以函数对象的方式自定义排序规则
class mycomp2 {
public:
    bool operator() (int i, int j) {
        return (i > j);
    }
};

int main() {
    std::vector<int> myvector{ 3,2,5,4,1,6,9,7};

    //以默认的升序排序作为排序规则，将 myvector 中最小的 4 个元素移动到开头位置并排好序
    std::partial_sort(myvector.begin(), myvector.begin() + 4, myvector.end());
    cout << "第一次排序:\n";
    for (std::vector<int>::iterator it = myvector.begin(); it != myvector.end(); ++it)
        std::cout << *it << ' ';
    cout << "\n第二次排序:\n";

    // 以指定的 mycomp2 作为排序规则，将 myvector 中最大的 4 个元素移动到开头位置并排好序
    std::partial_sort(myvector.begin(), myvector.begin() + 4, myvector.end(), mycomp2());
    for (std::vector<int>::iterator it = myvector.begin(); it != myvector.end(); ++it)
        std::cout << *it << ' ';
    return 0;
}
```

平均时间复杂度为`N*log(M)`，其中 N 指的是 [first, last) 范围的长度，M 指的是 [first, middle) 范围的长度。

### partial_sort_copy()

和 partial_sort() 唯一的区别在于，前者不会对原有数据做任何变动，而是先将选定的部分元素拷贝到另外指定的数组或容器中，然后再对这部分元素进行排序。

```cpp
#include <iostream>     // std::cout
#include <algorithm>    // std::partial_sort_copy
#include <list>       // std::list
using namespace std;
bool mycomp1(int i, int j) {
    return (i > j);
}
class mycomp2 {
public:
    bool operator() (int i, int j) {
        return (i > j);
    }
};
int main() {
    int myints[5] = { 0 };
    std::list<int> mylist{ 3,2,5,4,1,6,9,7 };
    //按照默认的排序规则进行部分排序
    std::partial_sort_copy(mylist.begin(), mylist.end(), myints, myints + 5);
    cout << "第一次排序：\n";
    for (int i = 0; i < 5; i++) {
        cout << myints[i] << " ";
    }
    //以自定义的 mycomp2 作为排序规则，进行部分排序
    std::partial_sort_copy(mylist.begin(), mylist.end(), myints, myints + 5, mycomp2());
    cout << "\n第二次排序：\n";
    for (int i = 0; i < 5; i++) {
        cout << myints[i] << " ";
    }
    return 0;
}
```
