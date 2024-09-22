###### ObjectFactory：
函数式接口：可以将lambda表达式作为参数放到方法的实参中，在方法执行的时候，并不会调用当前lambda表达式，只有调用getObject方法的时候才会调用lambda表达式。
###### 三级缓存：
singletonObjects:一级缓存
earlySingletonObjects:二级缓存
singletonFactories:三级缓存，三级缓存存储的值是lambda表达式

![[Pasted image 20240201195050.png]]
###### getSingleton():
![[Pasted image 20240201195942.png]]

调用时传入lambda表达式，表达式内容为返回createBean。
![[Pasted image 20240201200326.png]]

先从一级缓存中尝试获取对象，如果没有则调用lambda表达式
![[Pasted image 20240201200108.png]]

调用lambda表达式：即执行createBean
![[Pasted image 20240201200017.png]]

访问三级缓存：
重点在调用访问第三级缓存时调用lambda表达式，因为先前getEarlyreference的表达式中已经存储了A的实例对象了（通过参数的方式），所以该表达式可以直接返回实例对象或者改成代理对象。
![[Pasted image 20240202141510.png]]

###### craeteBean：
![[Pasted image 20240202135602.png]]
doCreateBean  -> createBeanInstance:使用反射机制创建实例。先创建包装类，再获取实例对象

###### addSingletonFactory:
![[Pasted image 20240202135810.png]]
该函数的调用是在doCreateBean中的，即在调用完createBeanInstance后再调用addSingletonFactory。
添加到三级缓存
###### getEarlyBeanReference:
![[Pasted image 20240202141251.png]]
该方法作为objectFactory（接口函数）传递到三级缓存，是在getsingleton中得到调用，用来返回实例对象。这也是三级缓存存在的目的，三级缓存储存lambda表达式，可以即返回对象，也可以返回代理对象。
在该lambda表达式正在得到执行后，返回的未初始化但实例化的对象会被存储到二级缓存中earlysingletonObjects。同时删除三级缓存中的lambda表达式。
###### populateBean:
完成属性的赋值。

getBean -> doGetBean -> getSingleton