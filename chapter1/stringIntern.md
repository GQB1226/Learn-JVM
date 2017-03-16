### 由String.intern()引发的思考

***

*   String不属于Java的8种基本数据类型，它是一个对象
*  有关常量池的概念: constant pool 是指编译期就被确定的各种字面量和符号引用，存在于方法区的Runtime Constant Pool  
*  字符串常量存放在常量池，
	* 直接由双引号声明的String对象是字符串常量，编译期间直接放到常量池
	* 其它方式创建的String对象可以通过String.intern()方法放入常量池，但是注意：==**JDK1.7之后,intern()不会再复制实例，只是在常量池中记录首次出现的实例的引用**==，以下如无特殊说明，都是在jdk1.7+
* 由new String()方式创建的是String对象，不是**常量**！！！这就是String.equal()和==的区别
* String.intern()首先会在常量池中查找是否存在相同的字符串常量，若有则返回其引用；如果没有则在常量池中记录首次出现的实例的引用。

***
#### 例子分析
```java
		String str1=new StringBuilder("计算机").append("软件").toString();
		System.out.println(str1.intern()==str1);
		
		String str2=new StringBuilder("ja").append("va").toString();
		System.out.println(str2.intern()==str2);

```
结果输出:true,false
* str1是一个String对象，“计算机软件”首次出现，常量池中没有该引用，所以返回true
* str2这里有很多疑问，如果将java改成java1之后得到的结果就是true.书上的解释是“java”这个字符串在执行StringBuilder之前就已经出现过，在常量池中有了它的引用。但是在程序代码里我们并没有定义啊，很多人说是jvm启动时就存在了，但是没道理啊，不能因为是java语言就得有个java字符串啊。目前网上我认为比较合理的解释是：
	1. jvm从启动，到执行main里面的第一条代码，要经历很多的，比如加载rt.jar里面所有的Class，加载一个class肯定要执行static{}中内容，况且rt.jar中的jdk的类里面有很多xxx.startWith("java")或者其他用到"java"的代码，jvm启动的时候直接按照常量加载进来。[来源于知乎(https://www.zhihu.com/question/32672669/answer/87514393)
	2. 我个人猜想是不是jvm默认存在的，就和其它的基本类型的封装类Integer，Long默认的有[-128,127]那样
