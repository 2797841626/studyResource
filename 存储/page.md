一个wordline对应多个page，SLC对应一个，MLC对应两个，TLC对应三个。
但page与CSB,MSB,LSB没有必然联系。在TLC中有这三个概念是因为一个cell可以表示三位的信息，它的大小也许仅有几bit，而页是由多个cell组成的，页的大小有多个kb。
==同一字线上所有闪存单元的 MSB 组合成 MSB 页，同一字线上所有闪存单元的LSB 组合成 LSB 页；类似地，TLC NAND 闪存中将每个字线上所有闪存单元的 MSB 组合成 MSB 页、CSB 组合成 CSB 页、LSB 组合成 LSB 页。==
![[Pasted image 20241111110408.png]]