一个存储单元（cell）存储1bit数据的NAND FLASH，我们叫它为SLC (Single Level Cell)，2bit为MLC (Multiple Level Cell) ，3bit为TLC (Triple Level Cell)。

对SLC来说，一个存储单元存储两种状态，浮栅极里面的电子多于某个参考值的时候，我们把它采样为0，否则，就判为1.

![](https://i-blog.csdnimg.cn/blog_migrate/75ebfbac26d5f8826e7a9e15205c2c4a.jpeg)

对MLC来说，一个存储单元存储四个状态，一个存储单元可以存储2bit的数据。通俗来说就是把浮栅极里面的电子个数进行一个划分，比如低于10个电子，判为0；11-20个电子，判为1；21-30，判为2；多于30个电子，判为3.

![](https://i-blog.csdnimg.cn/blog_migrate/a04d86f7eb2052074ccb4915d886a7d3.jpeg)

依次类推TLC，它的一个存储单元有8个状态，可以存储3bit的数据，它在MLC的基础上对浮栅极里面的电子数又进一步进行了划分。

![](https://i-blog.csdnimg.cn/blog_migrate/dc23c969e5033273fc06a244077ef664.jpeg)

同样面积的一个存储单元，SLC，MLC和TLC，依次可以存储1,2,3bit的数据，所以在同样面积的LUN上，NAND FLASH容量依次变大。

但同时，一个存储单元电子划分的越多，那么在写入的时候，控制进入浮栅极的电子的个数就要越精细，所以写耗费的时间就加长；同样的，读的时候，需要尝试用不同的参考电压去读取，一定程度上加长读取时间。所以我们会看到在性能上，TLC不如MLC,MLC不如SLC.