### 常量池
***
==**常量池水比较深！！！**==网上目前存在各种说法，还是以书上为准呗。
常量池大体可以分为两种：静态常量池和运行时常量池。


* 静态常量池，也就是*.class文件中的常量池，主要存放编译期生成的各种字面量和符号引用。字面量（Literal）相当于java层面的常量概念，比如字符串和final声明的常量等。符号引用（Symbolic References）可以理解为编译原理方面的，包括：类和接口的全限定名、字段名和描述符、方法名称和描述符。

* 运行时常量池。存在于方法区，个人理解就是在类被加载之后，原来的静态常量池存放到方法区的运行时常量池。除此之外，运行时常量池一个重要特征就是动态性。

***
常量池是为了避免频繁的创建和销毁对象而影响系统性能，其实现了对象的共享。
**在很多面试题中关于常量池的花样百出**
1. #####关于String的equal和==比较的
	```java
    	String s1="test";
		String s2="test";
		String s3=new String("test");
		System.out.println(s1==s2);
		System.out.println(s1==s3);
		System.out.println(s1.equals(s3));
	```
    以上结果的输出依次为：true,false,true
	首先需要明确一点：****直接由==双引号==声明的String对象是字符串常量，编译期间直接放到常量池****，其它方式创建的都是字符串对象！时刻记住这点，基本上就能解决大部分问题。另外一点就是java中直接使用==操作符，比较的是两个字符串的引用地址，并不是比较内容，equals比较的是内容。s1的**引用地址**在方法区里，s3是一个对象，其地址在堆里，所以不相等。其它的情形，比如使用+连接的情况只需要记住只要有new就有新对象就可。但是需要注意一个特例：
    ```java
        private static final String s1="ab";
        private static final String s2="cd";
        private static final String s3;
        private static final String s4;
        static{
            s3="ab";
            s4="cd";
        }

        public static void main(String[] args) {	
            String s5=s1+s2;
            String s6=s3+s4;
            String s="abcd";
            System.out.println(s==s5);//true
            System.out.println(s==s6);//false

        }
	```
    这里涉及到类的加载初始化等，所以在这里不再分析，待到学习到class加载章节再分析。
2. ##### 关于String s = new String("test"); 创建了几个对象？
	分析：考虑到整个类的加载过程，“test”本身在编译期就被放到了常量池里，在执行new String()时把常量池里的“test”复制到堆中，并把这个对象的引用赋值给s,因此有两个：一个是new String(),另一个是匿名的“test”
3. ##### 关于String.Inter()
	具体见[由String.intern()引发的思考](stringIntern.md)
4. ##### 关于java的8中基本类型
	java中基本类型的包装类的大部分都实现了常量池技术,即除了两种浮点数类型外的其余六种：Character,Byte,Short,Integer,Long,Boolean.但是需要注意，除了Boolean之外的五种封装类只有在[-128,127]范围内才在常量池内有对象。这也就出现了这样的面试题：
    ```java
    	Integer a=1;
		Integer b=1;
		Integer c=129;
		Integer d=129;
		System.out.println(a==b);//true
		System.out.println(c==d);//false
```

