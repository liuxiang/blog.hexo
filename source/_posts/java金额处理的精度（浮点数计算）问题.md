title:  java金额处理的精度( 浮点数计算 )问题

date: 2017-07-20 00:00:02
tags: [浮点数计算 ]



---
# 经典示例
```
System.out.println(0.1 + 0.2); //输出：0.30000000000000004
System.out.println(1.1 + 1.2); //输出：2.3
System.out.println(0.1f + 0.2f);//输出：0.3
```
- 原因：
`IEEE754`的浮点数世界里，0.1(单精度或双精度浮点数)并不是真正的0.1，0.2(单精度或双精度浮点数)也并不是真正的0.2，所以相加的值并不完全等于0.3。 或

问题处在`IEEE 754 floating-point arithmetic`，虽然在java是遵循这个规则的，但是java语言的实现，并不是使用小数点或者十进制来表示数字，相反，它是`采用分数和指数来表示`，而且是
```
0.5 = 1/2
0.75 = 1/2 + 1/(2^2)
0.85 = 1/2 + 1/(2^2) + 1/(2^3)
0.1 = 1/(2^4) + 1/(2^5) + 1/(2^8) + ... ```
0.1只能是无限循环下去的，这就意味着0.1在java里面不能够准确的用浮点数来表示，也就造成了浮点数运算上面的误差。

-  IEEE754标准

存储格式：`符号位`+`指数位偏移`+`尾数位`
   
- 单精确度（float 32位）、双精确度（double 64位）



- `规约形式`的浮点数：
如果浮点数中指数部分的编码值在0 < exponent < 2e-2之间，且尾数部分最高有效位（即整数字）是1，那么这个浮点数将被称为规约形式的浮点数。“规约”是指用唯一确定的浮点形式去表示一个值。
由于这种表示下的尾数有一位隐含的二进制有效数字，为了与二进制科学计数法的尾数相区别，IEEE754称之为有效数（significant）。
 
- `非规约形式`的浮点数：
如果浮点数的指数部分的编码值是0，尾数为非零，那么这个浮点数将被称为非规约形式的浮点数。
 
- `IEEE 754标准规定`：
非规约形式的浮点数的指数偏移值比规约形式的浮点数的指数偏移值大1.例如，最小的规约形式的单精度浮点数的指数部分编码值为1，指数的实际值为-126；而非规约的单精度浮点数的指数域编码值为0，对应的指数实际值也是-126而不是-127。实际上非规约形式的浮点数仍然是有效可以使用的，只是它们的绝对值已经小于所有的规约浮点数的绝对值；即所有的非规约浮点数比规约浮点数更接近0。规约浮点数的尾数大于等于1且小于2，而非规约浮点数的尾数小于1且大于0.
 
- `精度`
在二进制，第一个有效数字必定是“1”，因此这个“1”并不会存储。
单精和双精浮点数的有效数字分别是有存储的23和52个位，加上最左手边没有存储的第1个位，即是24和53个位。
 
- `浮点数的比较`
浮点数基本上可以按照符号位、指数域、尾数域的顺序作字典比较。显然，所有正数大于负数；正负号相同时，指数的二进制表示法更大的其浮点数值更大。




# Float toString 处理
```
    public static String toString(float f) {
        return FloatingDecimal.toJavaFormatString(f);
    }
```


#  浮点数精度
```
/**
* 浮点数精度
*/
static void floatDubleAccuracy(){
    Integer i = 2147483647 + 1;
    System.out.printf("[%s] %s", i.getClass(), i).println();
 
    Float f = 2147483647f, f2 = 999.0001f, f3 = 999.00001f;// 支持到后4位精度
    System.out.printf("[%s] %s", f.getClass(), f).println();
    System.out.printf("[%s] %s", f2.getClass(), f2).println();
    System.out.printf("%s] %s", f3.getClass(), f3).println();
 
    Double d = 2147483647d, d2 = 999.0000000000001d, d3 = 999.00000000000001d;// 支持到后13位精度
    System.out.printf("[%s] %s", d.getClass(), d).println();
    System.out.printf("[%s] %s", d2.getClass(), d2).println();
    System.out.printf("[%s] %s", d3.getClass(), d3).println();
 
    BigDecimal bigDecimal = new BigDecimal("999.0000000000000000000000000000000001");// 最高精度
    System.out.printf("[%s] %s", bigDecimal.getClass(), bigDecimal).println();
    System.out.println(bigDecimal.add(new BigDecimal("0.1")));// BigDecimal计算 999.1000000000000000000000000000000001
}
```


# BigDecimal  高精度二进制
```
/**
* 高精度二进制
*/
static void bigDecimal() {
    System.out.println(new BigDecimal(0.1f));// 0.100000001490116119384765625
    System.out.println(new BigDecimal("0.1"));// 0.1
    System.out.println(0.1f);// 0.1
 
    System.out.println(new BigDecimal(0.2d));
    System.out.println(new BigDecimal(0.1d).add(new BigDecimal(0.1d)));
 
    System.out.println(new BigDecimal(0.2f));
    System.out.println(new BigDecimal(0.1f).add(new BigDecimal(0.1f)));
 
    System.out.println(new BigDecimal(0.3d));
    System.out.println(new BigDecimal(0.1d).add(new BigDecimal(0.1d)).add(new BigDecimal(0.1d)));
 
    System.out.println(new BigDecimal(0.3f));
    System.out.println(new BigDecimal(0.1f).add(new BigDecimal(0.1f)).add(new BigDecimal(0.1)));
 
    System.out.println("计算：");
    //正确的姿势：
    System.out.println(new BigDecimal("0.1").add(new BigDecimal("0.2")));//输出：0.3
 
    //错误的姿势：
    System.out.println(new BigDecimal(0.1).add(new BigDecimal(0.2)));    //输出：0.3000000000000000166533453693773481063544750213623046875
}
```


---
# 业务系统的金额处理小方法
数据库 存储 与金额计算都已`分`为单位（或业务需要的精度位），数值为`int`型.
展示，业务使用等场景格式化为目标所需要的单位。
- 好处： 避免`浮点类型(float,double)`的计算精度问题


---
**参考**
` 为什么1.0 - 0.7 != 0.3????? - jiqikewang的专栏 - CSDN博客 `
http://blog.csdn.net/jiqikewang/article/details/7031738


` Java浮点数计算精度问题总结 - 简书 `
http://www.jianshu.com/p/1d6ba0047a5b


` Java几个细节和误区 - chuan1191330700的专栏 - CSDN博客 `
http://blog.csdn.net/chuan1191330700/article/details/53322525


` java中float和double精度问题 - 简书 `
http://www.jianshu.com/p/c51041a791bd


`Java 使用BigDecimal类处理高精度计算 - 简书`
http://www.jianshu.com/p/25b1921ddcd7

` 关于Java中浮点数精度丢失 - 简书 `
http://www.jianshu.com/p/9f46c523d5ab


` 单精度与双精度是什么意思，有什么区别？ - 知乎 `
https://www.zhihu.com/question/26022206



