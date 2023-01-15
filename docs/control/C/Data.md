![](https://img-blog.csdnimg.cn/e5d63372eb8544fda48754e24b9b0091.png#pic_center)

# 基本类型

最常用的**整型, 实型与字符型**(char,int,float,double):

![![在这里插入图片描述](https://img-blog.csdnimg.cn/d53b58cf6cc244b5b03683db6ba99057.png#pic_center)](https://img-blog.csdnimg.cn/3efa3bdbc8754947ae7634f3a93b16e3.png#pic_center)



**整型数据**是指不带小数的数字(int,short int,long int, unsigned int, unsigned short int,unsigned long int):

![](https://img-blog.csdnimg.cn/6a8032093c6743de8d058499dc6e9038.png#pic_center)

**浮点数据**是指带小数的数字。

因为精度的不同又分为3种(float,double,long double)：

![在这里插入图片描述](https://img-blog.csdnimg.cn/d53b58cf6cc244b5b03683db6ba99057.png#pic_center)

定义变量：数据类型 变量名 = 赋值；

![在这里插入图片描述](https://img-blog.csdnimg.cn/c433ececa5d84ea78417209e59c4d513.png#pic_center)

# 构造类型

**数组**是用于储存多个相同类型数据的集合。

一维数组定义及初始化是有三种形式：

1、数据类型 数组名称[长度n] = {元素1,元素2…元素n};

2、数据类型 数组名称[] = {元素1,元素2…元素n};

3、数据类型 数组名称[长度n]; 数组名称[0] = 元素1; 数组名称[1] = 元素2;

注：数组的下标均以0开始；数组在初始化的时候，数组内元素的个数不能大于声明的数组长度；元素个数小于数组的长度时，多余的数组元素初始化为0。

多维数组定义及初始化是两种：

1、数据类型 数组名称[常量表达式1][常量表达式2]…[常量表达式n] = {{值1,…,值n},{值1,…,值n},…,{值1,…,值n}};

2、数据类型 数组名称[常量表达式1][常量表达式2]…[常量表达式n]; 数组名称[下标1][下标2]…[下标n] = 赋值;

如：![在这里插入图片描述](https://img-blog.csdnimg.cn/8b3ed6a7be0f4d09a4c0ebde0417c8a8.png#pic_center)

定义了一个名称为num，数据类型为int的二维数组。其中第一个[3]表示第一维下标的长度；第二个[3]表示第二维下标的长度。

![在这里插入图片描述](https://img-blog.csdnimg.cn/5c4b902435544ea19ee7e300b941132c.png#pic_center)

注：二维数组定义的时候，采用第一种初始化时可以不指定行的数量，但是必须指定列的数量。采用第二种初始化时数组声明必须同时指定行和列的维数。

**结构体**

结构体是把一些基本类型数据组合在一起形成的一个新的复合数据类型

结构体定义的3种方式

第一种方式

![在这里插入图片描述](https://img-blog.csdnimg.cn/e648b10258454f03b2dabe8601d48ff3.jpeg#pic_center)

第二种方式

![在这里插入图片描述](https://img-blog.csdnimg.cn/c640fc4c69ad4568a5858f7fd00bd5d2.png#pic_center)

第三种方式

![在这里插入图片描述](https://img-blog.csdnimg.cn/64500cd2a05d4018806f02d65289a740.png#pic_center)

结构体赋值和初始化

定义的同时可以整体赋初值

如果定义完之后,则只能单个的赋初值

取出结构体变量中的每一个成员

1．结构体变量名.成员名

2．指针变量名->成员名

指针变量名->成员名在计算机内部会被转化成(*指针变量名).成员名的方式来执行，所以说这两种方式是等价的

例子:![在这里插入图片描述](https://img-blog.csdnimg.cn/49bad602d1fc4b9e876bf1fe60deff78.png#pic_center)

## 指针类型

**指针**就是地址， 地址就是指针

指针变量就是存放内存单元编号的变量，或者说指针变量就是存放地址的变量指针和指针变量是两个不同的概念，但通常我们叙述时会把指针变量简称为指针，实际它们含义并不一样

指针定义![在这里插入图片描述](https://img-blog.csdnimg.cn/3077016ffe1d4b4dbcd05dbdb64c8fb1.png#pic_center)