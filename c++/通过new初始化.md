`int main() {`  
    `// 使用 new 动态分配`  
    `int *w = new int(5);//初始化w`  
    `std::cout<<*w<<std::endl;`  
    `int* chipID = new int[50]; //初始化数组`  
    `for(int i=0;i<50;i++){`  
        `*(chipID+i) = i;`  
    `}`  
    `for(int i=0;i<50;i++){`  
        `std::cout << "Chip ID: " << *(chipID+i) << std::endl;`  
    `}`  
      
    return 0;
`}`