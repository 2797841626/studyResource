
###### set
每个元素的键值都唯一，不允许两个元素有相同的键值。
所有元素都会根据元素的键值自动排序（默认从小到大）。
set 中的元素不像 map 那样可以同时拥有实值(value)和键值(key)，只能存储键，是单纯的键的集合。
set 中元素的值不能直接被改变。
set 支持大部分的map的操作，但是 set 不支持下标的操作，而且没有定义mapped_type类型。

###### unordered_set
底层由哈希表实现的set
###### mutiset
multiset是一个底层由红黑树组成的有序集合，其允许重复元素出现
multiset<int> ms
插入元素 ms.insert(num)
删除元素
用元素删除ms.erase(num)
用迭代器删除 ma.erase(ms.find(num))


