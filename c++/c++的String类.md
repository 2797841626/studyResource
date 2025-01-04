String的初始化
`string s1; // 构造空的string类对象s1 
`string s2("hello bit"); // 用C格式字符串构造string类对象s2` 
`string s3(s2); // 拷贝构造s3`

push_back
[push_back](https://legacy.cplusplus.com/reference/string/string/push_back/)在字符串后尾插字符c
![[Pasted image 20240228004725.png]]
pop_back();
删除源字符串的最后一个字符，有效的减少它的长
string str("hello world!"); str.pop_back();

string的长度：s.size();s.length();

###### string类的元素索引：

- str[1]——返回值为 str 中位置1处的字符（char&），从 0 开始索引
- string.at(1)——同上
-
string类的裁剪
s.substr(start,length);

string.replace(i,j,"a")将i到j的字符串替换成字符串a，注意j为开区间 

string.find("a") 寻找字符串中的子字符串a的位置 

s1.erase(1, 3); //删除子串(1, 3)，1为起始位置  3为长度
s1.erase(5); //删除下标5及其后面的所有字符，此后 s1 = "R Ste"

  
s1.insert(2, "123"); //在下标 2 处插入字符串"123" 注意是插入string！

string可以比大小，按字典序比大小。

```cpp
class Solution {
public:
    string removeStars(string s) {
        string ans;
        for (char c : s) {
            if (c == '*') ans.pop_back();
            else ans += c;
        }
        return ans;
    }
};
```

```cpp
        //1.尾部插入及删除数字
	vec2.push_back(1);  //尾部插入元素
        vec2.pop_back()     //删除尾部元素
 
	//2.使用下标访问元素，
	cout << vec2[0] << endl; //记住下标是从0开始的。
 
	//3.使用迭代器访问元素.
	vector<int>::iterator it;
	for (it = vec2.begin(); it != vec2.end(); it++)
		cout << *it << endl;
 
	//4.插入元素：    
	vec2.insert(vec2.begin() + i, a); //在第i + 1个元素前面插入a;
 
	
	//5.删除元素：    
	vec2.erase(vec2.begin() + 2); //删除第3个元素
 
	vec2.erase(vec2.begin() + i, vec2.end() + j); //删除区间[i, j - 1]; 区间从0开始
 
	//6.求数组大小:
	vec2.size();
 
	//7.清空 : 
	vec2.clear();
	//四种在末尾添加元素的方式
	string a = "abc";
	//+既可以char，也可以string，但注意的是放char时只能有一个字母
	a = a+'a';  
	a=a+"bcd";  
	//push_back()放的是char
	a.push_back('a');  
	//append放置的是string类
	a.append("b");
```