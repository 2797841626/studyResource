`std::vector<int> computeNext(const std::string &pattern) {`  
    `int m = pattern.length();`  
    `std::vector<int> next(m);`  
    `int j = 0; // j 是前缀的长度（最长相等前后缀长度）`  
    `next[0] = 0; // 第一个元素特殊处理`  
  
    `for (int i = 1; i < m; ++i) {`  
        `// 当匹配不成功时，回退 j 的值`  
        `while (j > 0 && pattern[i] != pattern[j]) {`  
            `j = next[j - 1]; //此为精华 例子[c b c] a [c b c] b,当求最后一个b的next值时，由于a!=b,匹配失败，所以需要考虑b前面的字符串和开头字符串中有几个相同。这个相同个数即为next[j-1].`  
        `}`  
        `// 如果字符匹配，增加 j 的长度`  
        `if (pattern[i] == pattern[j]) {`  
            `j++;`  
        `}`  
        `next[i] = j; // 更新 next[i] 为当前的 j    }`  
    `return next; // 返回计算得到的 next 数组`  
`}`  
`void kmpSearch(const std::string &text, const std::string &pattern) {`  
    `int n = text.length();`  
    `int m = pattern.length();`  
    `std::vector<int> next = computeNext(pattern); // 计算 next 数组`  
  
    `int j = 0; // 模式字符串的指针`  
    `for (int i = 0; i < n; ++i) {`  
        `// 当匹配失败时，使用 next 数组回退 j`       
         `while (j > 0 && text[i] != pattern[j]) {`  
            `j = next[j - 1]; //直接从pattern[j] 与text[i]进行比较`  
        `}`  
        `// 如果字符匹配，j 增加`  
        `if (text[i] == pattern[j]) {`  
            `j++;`  
        `}`  
        `// 如果 j 达到模式字符串的长度，表示找到了匹配`  
        `if (j == m) {`  
            `std::cout << "模式字符串在文本中的起始位置: " << i - m + 1 << std::endl;`  
            `j = next[j - 1]; // 继续查找下一个匹配`  
        `}`  
    `}`  
`}`