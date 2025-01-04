对应ASCII码
‘A'~'Z':65~90 
'a'~'z':97~122
’0‘ ~ ’9‘  48~57
刚好差32
换行符 ‘/n’

char数字转int数字
char a = '1';
int n = a - '0';

int数字转char数字
char a = 4 + '0';

将字母转换成小写，若是数字则不变
tolower(char a)
toupper(char a)
用上述两个函数的时候注意，返回的是ASCII码，即int，可以char c = toupper(char a)强转为相应的char类型
islower(char a)判断是否为小写
isupper(char a)判断是否为大写  
isspace(s[i]) 判断字符串是否为空格，若是返回true
reverse(s.begin(),s.end())
isdigit(expression[i]) 判断是否为数字
isalpha(s[i]) 判断是否为字母
isalnum(char a)判断char是否为字母或数字
isVowel(s[i])判断是否为元音字母

char类型大小写转换可直接+32或者-32即可
char a = 'a';
a =a-32;

char字符相加是ascii码相加
stirng字符串相加就是拼接
string与char是可以直接相加的，就是拼接