title: C++类内成员访问控制中一个容易忽略的地方
date: 2016-02-03 11:10:42
tags:
---
今天看到了运算符重载，在动手实践的过程中遇到了一个疑惑。

> main.cpp

```
class c{
    	
    	private:
    		int a,b;
    	public:
    		c(void){
	            a=b=2;
            }
    		c(int){
            	a = in; b = 1;
            }
    		
			c operator+(c cadd){
	            c csum;
	            csum.a = a + cadd.a;
	            csum.b = b + cadd.b;
            	return csum;
            }
		
 };

```
在这段代码中，14行新建了一个对象，为何在15和16行中能直接访问其私有成员呢，这不是在对象外么。
经过查找资料，结果如下：

> A member of a class can be
— private; that is, its name can be used only by members and friends of the class in which it is
declared.
— protected; that is, its name can be used only by members and friends of the class in which it is
declared, and by members and friends of classes derived from this class (see 11.5).
— public; that is, its name can be used anywhere without access restriction.

就是说，访问限制是针对于类的，而不是针对于对象的。
类的对象在类的内部默认是友元，同一个类的不同对象可以互相访问其私有成员，而不属于这个类的其他对象和函数才不能访问其私有成员。
原来我一直理解有误，认为访问控制针对于对象，这样是错的。

