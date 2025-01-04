###### lower_bound
在有序数组中找到第一个大于等于val的位置
```cpp
lower_bound (ForwardIterator first, ForwardIterator last, const T& val, Compare comp);
```
first,last: 迭代器在排序序列的起始位置和终止位置，使用的范围是[first,last).包括first到last位置中的所有元素
val: 在[first,last)下，也就是区分（找到大于等于val值的位置，返回其迭代器）
comp： 主要针对于原型二，传一个函数对象，或者函数指针，按照它的方式来比较
返回值：返回一个迭代器，指向第一个大于等于val的位置

###### upper_bound
在有序数组中找到第一个大于val的位置。

==注：要求数组是升序==

若数组降序，则需要填充comp函数
```cpp
bool comp(const int & val,const int &element){  
    return val > element;  
}  
int main()  
{  
    vector<int> wow = {5,4,3,2,1};  
    int val =3;  
    //返回第一个满足comp情况的位置，但lower_bound会自动加上等于
    //输出3，因为element：3是第一个小于等于3的元素
    std::cout<<*std::lower_bound(wow.begin(),wow.end(),val,comp)<<std::endl;
    //输出位置为2
    int i = std::lower_bound(wow.begin(),wow.end(),val,comp) - wow.begin(); 

    //输出2，因为element：2是第一个小于3的元素
    std::cout<<*std::upper_bound(wow.begin(),wow.end(),val,comp)<<std::endl;
    //输出位置为3
    int i = std::upper_bound(wow.begin(),wow.end(),val,comp) - wow.begin();
    std::cout<<"i:"<<i<<std::endl;  
    return 0;  
}
```