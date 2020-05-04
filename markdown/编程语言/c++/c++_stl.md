## STL
### 顺序容器

||说明|
|:-|:-|
|vector|可变大小数组。支持快速随机访问。在尾部之外的位置插入或删除元素可能很慢。|
|deque|双端队列。支持快速随机访问。在头部或尾部插入元素很快。|
|list|双向链表。只支持双向顺序访问。在list中任何位置进行插入/删除操作速度都很快。|
|forward_list|单向链表。只支持单向顺序访问。在链表任何位置进行插入/删除操作速度都很快。|
|array|固定大小的数组。支持快速随机访问。不能添加或删除元素。|
|string|与vector相似的容器，但专门用于保存字符。随机访问快。在尾部插入/删除速度快。|

#### 容器通用操作

||说明|
|:-|:-|
|类型别名||
|iterator|此容器的迭代器类型|
|const_iterator|可以读取元素，但不能修改元素的迭代器类型|
|size_type|无符号整数类型，足够保存此种容器类型最大可能容器的大小|
|difference_type|带符号整数类型，足够保存两个迭代器之间的距离|
|value_type|元素类型|
|reference|元素的左值类型，与value_type&含义相同|
|const_reference|元素的const左值类型，const  value_type&|
|构造函数||
|C c;|默认构造函数，构造空容器|
|C c1(c2);|构造c2的拷贝c1|
|C c(b, e);|构造c，将迭代器b和e指定的范围内的元素拷贝到c|
|C c{a,b,c...};|列表初始化c|
|赋值与swap||
|c1 = c2||
|c1 = {a,b,c...}||
|a.swap(b)||
|swap(a,b)||
|大小||
|c.size()||
|c.max_size()|c可保存的最大元素数目|
|c.empty()|若c中存储了元素，返回false，否则返回true|
|添加/删除元素(不适用array)||
|c.insert(args)||
|c.emplace(args)||
|c.erase(args)|删除args指定的元素|
|c.clear()|删除c中的所有元素|
|关系运算符||
|==, !=|所有容器都支持相等，不相等操作|
|<,<=,>,>=|关系运算符|
|获取迭代器||
|c.begin(),c.end()||
|c.cbegin(),c.cend()||
|反向容器的额外成员||
|reverse_iterator||
|const_reverse_iterator||
|c.rbegin(),c.rend()||
|c.crbegin(),c.crend()||

顺序容器(array除外)还定义了一个名为assign的成员，允许我们从一个不同但相容的类型赋值，或者从容器的一个子序列赋值。
```c++
list<string> names;
vector<const char*> oldstyle;
names.assign(oldstyle.cbegin(),oldstyle.cend());
```

### 泛型算法
### 关联容器