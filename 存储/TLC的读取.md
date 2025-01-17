
我们知道Nand Flash使用浮栅晶体管作为存储单元（memory cell）来存储数据，浮栅晶体管物理结构如图1所示：

![[Pasted image 20240912164730.png]]

图1 浮栅晶体管

对于普通的晶体管（去掉浮栅晶体管中的浮栅层，floating gate），暂时先忽略其他因素，我们只看control gate施加的电压（这里暂时称为Vg），==当Vg增大到一定值得时候，channel中有电子流过（导通），此时的电压大小称为阈值电压Vth==；当Vg小于Vth时，channel中没有电子流过（截止）。

对于浮栅晶体管：

==如果浮栅层存有电子，它会抑制Vg对channel中的电子流动（导通）的影响；==
==浮栅层电子越多抑制效应就越明显，导致让channel导通的阈值电压越大；==
所以，理解阈值电压很重要，因为阈值电压可以表明浮栅里含有电子多少的程度。

对于TLC Nand Flash，每个存储单元里可以存储3个bit数据，也就说有个种状态，即浮栅晶体管里电子的数量有8种程度数量的状态：


![[Pasted image 20240912164657.png]]
图2 TLC的8种电子状态

那如何读取TLC里的数据呢？如图2所示，其实如果知道浮栅里面的电子数量处于哪个状态就知道是什么数据了。就如上面所描述的，浮栅里的电子数量可以反映到阈值电压Vth上面（阈值电压Vth越大浮栅里的电子越多），而阈值电压可以通过逐渐增大Vg查看channel是否导通来确定。


![[Pasted image 20240912164710.png]]
图3 阈值电压的分布

从原理上分析，在图3中（与图2不同的一种编码方式），逐渐从最小值增大Vg（也被称为读参考电压Vread），如果Vg=Va，此时如果memory cell导通，它的编码值就为二级制111，以此类推。

实际操作是这样的，将memory cell里的3个bit划分为：MSB，CSB，LSB。只要分别确定了这3个bit的值，memory cell的电压状态也随之被确定，其值也就被读出来了。具体过程是：

通过一个读参考电压Vd来区分LSB的值；
再通过二个读参考电压Vb和Vf区分CSB的值；
最后通过四个读参考电压Va，Vc，Ve，Vg区分MSB的值。
理论上只需要三次阈值电压的尝试即可判断三位是什么。实质上就是二分法。
###### 例子：
111  Va 110  Vb  101  Vc 100  Vd 011 Ve  010  Vf 001 Vg  000
使用Vd即可区分最高位MSB是否是1
若导通则代表电压为左边的序列，没导通则为右边的序列。
导通情况下：使用Vb区分CSB是否是1，Vb左边的CSB全为1，右边的为0.
未导通情况下:使用Vf区分SCB是否为1.
之后再根据导通情况选择Va，Vc，Ve，Vg来区分LSB.
理论情况下只需要三次尝试便可得到浮栅层的存储的电子状态。


