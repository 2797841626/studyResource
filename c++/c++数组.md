数组的一些神奇操作
`LinkList* a[length];`
代表创建了指针数组，里面的值都为LinkList的指针
a代表该数组的头指针，* a与a[0]指代相同，都表示数组的第一个数。
想要将数组的指针传入函数，函数需写成
`void test(LinkList* a[])`
这样在调用函数时就可以这样调用:
`LinkList* a[length];`
`test(a);` //a为数组的指针，直接写入。

###### vector初始化
vector<boolean> list(8,false); //创建一个布尔数组，8个false //leetcode用这个会报错
list.resize(8,false); //lecode用这个不会报错
vector二维数组的初始化
vector<vector<bool>> bool_board(rows,vector<bool>(cols,false));

###### vector排序算法：
sort(res.begin(),res.back()) 排序算法
 `sort(points.begin(), points.end(), [](const vector<int>& u, const vector<int>& v) {return u[0] < v[0];});`
     

或
`bool comp(const int &a,const int &b)`  
`{`  
`return a>b;`  
`}`  
`int main()`  
`{`  
`vector<int>v;`  
`v.push_back(13);`  
`v.push_back(23);`  
`v.push_back(03);`  
`v.push_back(233);`  
`v.push_back(113);`  
`sort(v.begin(),v.end(),comp);`


访问最后一个元素 res.back()