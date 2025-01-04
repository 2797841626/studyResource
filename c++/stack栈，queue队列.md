```cpp
stack<int> q;	//以int型为例
int x;
q.push(x);		//将x压入栈顶
q.top();		//返回栈顶的元素
q.pop();		//删除栈顶的元素
q.size();		//返回栈中元素的个数
q.empty();		//检查栈是否为空,若为空返回true,否则返回false

```
若其中存储pair
```
stack<pair<int, int>> s;
s.emplace(1,2);
```

### queue常用函数
```c++
1. push() 在队尾插入一个元素
2. pop() 删除队列第一个元素
3. size() 返回队列中元素个数
4. empty() 如果队列空则返回true
5. front() 返回队列中的第一个元素
6. back() 返回队列中最后一个元素
```
##### 双端队列deque
```cpp
#include <iostream>
#include <deque>
#include <string>
using namespace std;
 
int main(void) {
	deque<string> q;
	q.push_back("world");		//将"world"入队尾
	cout << "将'world'入队尾后q.back(): " << q.back() << endl;	//输出队列尾部元素，就是"world"（此时队列中只有一个元素，队列头=队列尾）
	q.push_front("Hello ");		//将"Hello "入队首
	cout << "将'Hello '入队首后q.front(): " << q.front() << endl;	//输出队列头部元素，就是"Hello "
	cout << "此时队列的尾部元素: " << q.back() << endl;	//输出队列中尾部元素，就是"world"
	q.push_back("!");			//将"!"入队尾
	q.pop_front();			//将队列头部的元素删除（删除"Hello "），无返回值
	cout << "删除队列头部元素后，新队列的头部元素： " << q.front() << endl;	//此时队列头部元素为"world"
	cout << "队列是否为空（0：非空；1：空）: " << q.empty() << endl;	//empty()返回bool值，表示队列是否为空，此时不为空，返回0
	q.pop_back();			//将队列尾部的元素删除（删除"!",此时队列中只有一个"world"），无返回值
	cout << "又删除了队尾元素‘!'，现在队列是否为空（0：非空；1：空）: " << q.empty() << endl;	//empty()返回bool值，表示队列是否为空，此时队列不为空，返回0
	cout << "此时队列的大小: " << q.size() << endl;	//返回队列的大小，此时队列大小为1，输出1
 
	return 0;
}
```