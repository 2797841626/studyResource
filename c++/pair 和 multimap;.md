###### pair
`std::pair` 是 C++ 标准库中提供的一个模板类型,用于将两个值组合成一个单一的实体。

具体来说:

1. `std::pair` 是一个模板类,它可以存储任意两种类型的数据。其定义如下:
    ```
    template <class T1, class T2>
    struct pair {
        T1 first;
        T2 second;
        // 构造函数、赋值运算符等成员函数
    };
    ```
    
2. 使用 `std::pair` 可以很方便地在函数间传递两个相关的值,而不需要创建一个自定义的结构体或类。
    
3. 常见的使用场景包括:
    
    - 返回函数的多个值
    - 作为关联容器（如 `std::map`）的键值对
    - 存储一对相关数据

例如:
```
#include <utility>

std::pair<int, std::string> getPersonInfo() {
    return std::make_pair(25, "John Doe");
}

int main() {
    auto person = getPersonInfo();
    std::cout << "Age: " << person.first << ", Name: " << person.second << std::endl;
    return 0;
}
```

在这个例子中,`getPersonInfo()` 函数返回一个 `std::pair<int, std::string>` 类型的对象,表示一个人的年龄和姓名。

总之, `std::pair` 是一个非常实用的 C++ 标准库类型,它可以方便地组合和操作两个相关的值。

###### multimap
与pair相反，它是键值对的集合。
`std::multimap<std::string, int> grades; // 单个键值对插入` 
`// 插入键值对`
`grades.emplace("Alice", 90);` 
`grades.emplace("Alice", 85);`
`grades.insert(std::make_pair("Alice", 90));` 
`grades.insert({"Alice", 85});`

`// 遍历` 
`multimap for (const auto& pair : grades) {` 
    `std::cout << pair.first << ": " << pair.second << std::endl;` 
`}`
`// 查找某个键关联的所有值` 
`auto range = grades.equal_range("Alice");` 
`for (auto it = range.first; it != range.second; ++it) {` 
    `std::cout << "Alice's grades: " << it->second << std::endl;`
`}`