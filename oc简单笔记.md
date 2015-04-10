#OC简单笔记

```
记得看李明杰老师的视频，老师说过，凡是nsstring，就用copy，定义一个模型对象，就用strong，只是赋值的，例如int、double、char 以及CGRect类似的就用assign。
自己的笔记如下~
这些关键字基本上是针对属性的set方法。
当用copy时，set方法会先release旧值，再copy一个新的对象，reference count 为1（减少了对上下文的依赖）；当用assign，直接赋值，无retain操作。当用retain，release旧值，retain新值；


而strong与weak的区别
strong类似于retain，会将对象的引用计数器+1，分配内存地址。
weak类似于指针，只是单纯的指向某个地址，但是本身并未分配内存地址。当指向的地址被销毁时，该指针会自动nil。
例子：
  1.    @synthesize string1;   
  2.    @synthesize string2;  

再来猜一下，下面输出是什么？ 



    1.    self.string1 = [[NSString alloc] initWithUTF8String:"string 1"];   
    2.    self.string2 = self.string1;   
    3.    self.string1 = nil;  
    4.    NSLog(@"String 2 = %@", self.string2);  


结果是：String 2 = null

分析一下，由于self.string1与self.string2指向同一地址，且string2没有retain内存地址，而 self.string1=nil释放了内存，所以string1为nil。声明为weak的指针，指针指向的地址一旦被释放，这些指针都将被赋值为 nil。这样的好处能有效的防止野指针。在c/c++开发过程中，指针的空间释放了后，都要将指针赋为NULL. 在这儿用weak关键字做了这一步。

```