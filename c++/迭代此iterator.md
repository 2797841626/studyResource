
迭代器重载了运算符++，--，使得可通过++iterator访问到下一个元素。
begin指向的是第一个元素，而end指向的是最后一个元素的后一位，但* end 输出的结果仍然是最后一个元素的值。
可以通过begin == end来判断容器是否为空。
验证方法：
`int main() {`  
    `std::set<int> mySet = {1,2,3,4,5};`  
  
    `// 遍历集合`  
    `for (auto it = mySet.begin(); it != mySet.end(); ++it) {`  
`//        std::cout << *it << " "; // 输出: 1 2 3 4 5`  
    `}`  
    `auto it = mySet.end();`  
    `std::cout << *it<<std::endl;`  
    `std::cout << *(--it)<<std::endl;`  
    `std::cout << *(it--)<<std::endl;`  
    `std::cout << *(it)<<std::endl;`  
    `auto item = mySet.begin();`  
    `std::cout << *item<<std::endl;`  
    `std::cout << *(++item)<<std::endl;`  
    `// 说明 end() 的用法`  
    `if (mySet.end() != mySet.begin()) {`  
        `std::cout << "\nThe set is not empty." << std::endl;`  
    `}`  
  
    `return 0;`  
`}`

判断集合中是否存在某值
`int main() {`
`std::set<int> mySet = {1, 2, 3, 4, 5};`
`int number = 3;` 
`auto it = mySet.find(number);` 
`if (it != mySet.end()) {` 
    `std::cout << number << " is in the set." << std::endl;` 
     `} else {` 
     `std::cout << number << " is not in the set." << std::endl;`
`}` 
`return 0;` 
`}`