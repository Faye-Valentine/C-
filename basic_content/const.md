### const

1. #### 类型修饰符const表示常量或对象的值是不可以改变的

2. #### const的作用

   1. 定义常量

      `const int a=100`

   2. 类型检查

      `const与#define宏定义的区别:const具有类型可以被执行安全检查,而宏定义只是做简单的字符替换,没有安全检查`

   3. 防止修改起保护作用

   4. 节省空间

      `汇编语言的角度来看,const在程序运行的过程中,仅有一份内存拷贝,而#define定义的常量在运行的过程中,有若干个拷贝`

3. #### const对象默认为文件的局部变量

   **非const变量默认是extern 要使const变量在其他文件中进行访问 必须显式指为extern**

   `未被const修饰变量在不同文件中的访问`

   ```C++
   //file1.cpp
   int ext
   //file2.cpp
   #include<iostream>
   extern int ext;
   int mian(){
       stb::cout<<(ext+10)<<stb::endl;
   }
   ```

   `被const修饰变量在不同文件中的访问`

   ```c++
   //extern_file1.cpp
   extern const int ext=12;
   //extern_file2.cpp
   #include<iostream>
   extern const int ext;
   int main(){
        stb::cout<<(ext+10)<<stb::endl;   
   }
   ```

   **const常量需要显式声明extern 并且需要初始化(常量在定义后不可被修改)**

4. #### 定义常量

   ```C++
   const int b=10;
   b=0;//error
   const string s="helloworld"
   const int i,j=0//error
   ```

   错误：

   * b为常量不可更改
   * i为常量，在**定义时就要进行初始化**

5. #### 指针与const

   与指针相关的const四种：

   ```c++
   const char *a;//指向const对象的指针or指向常量的指针
   char const *a;//同上
   char * const a;//指向类型对象的const指针or常指针,const指针
   const char * const a;//指向const对象的const指针
   ```

   **const位于`*`的左侧,即指针指向常量 const位于`*`的右侧,指针本身为常量**

   

   使用:

   1. 指向常量的指针

      ```c++
      const int *ptr;
      *ptr=10;//error
      ```

      ptr为一个指向int类型const对象的指针，const定义是int类型，也就是ptr所指向的对象类型，而不是ptr本身，所以ptr不用赋初始值。但不能通过ptr去修改对象的值。

      除此之外，**也不可使用`void *`指针来保存const对象的地址,必须使用`const void *`类型的指针保存const对象的地址**

      ```c++
      const int p = 10;
      const coid * vp = &p;
      void *vp = &p;//error
      ```

      另:**允许把非const对象的地址赋给指向const对象的指针**

      ```c++
      const int *ptr
          int val = 3;
      ptr = &val;//ok
      ```

      我们不能通过ptr指针来修改val的值，即使它指向的是非const的对象！

      *因为指针ptr指向的是const int，指针地址可变，而指向的值不可变*

      我们不能使用指向const对象的指针修改基础对象，而若指针指向了非const对象，可以通过其他方式来修改其所指对象。可以修改const指针指向的值，但不可以通过const对象指针来进行，如下修改：

      ```c++
      int *ptrr1 = &val;
      *ptr1 = 4;
      cout<<*ptr<<endl;
      ```

      > 小结：
      >
      > 1. 对指向常量的指针，不可通过指针或直接赋值来修改对象的值
      > 2. 不能用`void *`指针保存const对象的地址，必须使用`const void *`类型的指针
      > 3. 允许将非const对象的地址赋给const对象的指针,如果要修改指针所指向的对象值,必须通过其他方式修改,不能直接通过当前指针直接修改

   2. 常指针

      const指针必须进行初始化，且const指针的值不可被改变

      ```c++
      #include<iostream>
      using namespace std;
      int main(){
          int num = 0;
          int * const ptr = &num;//const指针必进行初始化
          int * t = &num;
          *t = 1;
          cout<<*ptr<<endl;
      }
      ```

      上述修改ptr指针所指向的值,可以通过非const指针来进行修改

      最后,当把一个const常量的地址赋给ptr的时候,由于ptr指向的是一个常量,而不是const常量,所以会报错,出现:`const int * -> int *`的错误

      ```c++
      #include<iostream>
      using namespace std;
      int main(){
          const int num = 0;
          int * const ptr = &num;//error const int* -> int *
          cout<<*ptr<<endl;
      }
      ```

      上述若改为`const int * ptr`or`const int * const ptr`,都可以恢复正常

    3. 指向常量的常指针

       ```c++
       const int p = 3;
       const int * const ptr = &p;
       ```

       ptr为一个const指针，然后指向了一个int类型的const对象

6. #### 函数中使用const

   > const 修饰函数的返回值

   1. **const int**

      ```c++
      const int func1();
      ```

      本身无意义,因为参数返回本身就是赋值给其他变量

   2. **const int ***

      ```c++
      const int * func2();
      ```

      指针指向内容不变

   3. int *const

      ```c++
      int *const func3();
      ```

      指针本身不可变

    > const修饰函数参数

   1. 传递过来的参数及指针本身在函数内不可变,**无意义**

      ```c++
      void func(const int var);//传递过来的参数不可变
      void func(int *const var);//指针本身不可变
      ```

      表明参数在函数体内无法修改，但在这里没有什么意义

      因为var本身就是形参，在函数体内不会改变，包括传入的形参是指针也是一样的，都是形参

      输入参数采用“值传递”，由于函数将自动产生临时变量用于复制该参数，该输入参数本来就无需保护，故不需要const来修饰

   2. **参数指针所指内容为常量不可改变**

      ```c++
      void StringCopy(char *dst,const char *src);
      ```

      其中src为输入参数，dst为输出参数。给src加上const后，函数内的语句试图改动src的内容，编译器将指出错误。

   3. **参数为引用，为了增加效率的同时防止修改**

      ```c++
      void func(const A &a)
      ```

      对于非内部数据类型的参数而言，像`void func(A a)`这样的声明函数效率是很低的

      因为函数体内将产生A类型的临时对象用于复制参数a,而临时对象的构造,复制,析构过程都会消耗时间。

      为了提高效率，可以将函数声明为`void func(A &a)`，因为“引用传递”仅借用了一下参数的别名而已，不需要产生临时变量。

      > 但是函数 void func(A &a)存在缺点:
      >
      > "引用传递"有可能改变参数a，这是我们不希望的。解决这个问题很容易，加const即可，因此函数最终成为了 void func(const A &a)。

      *以此类推，void func(int x)是否应该改写为 void func(const int &x)?* 

      > 没有必要，因为内部数据类型的参数并不存在构造，析构的过程，复制也很快。
      >
      > 值传递=引用传递

      > 小结：
      >
      > 1.  对于非内部数据类型的输入参数，应该将“值传递”改为“const引用传递”，以提高效率，即将`void func(A a)`改为`void func(const A &a)`
      > 2.  对于内部数据类型的输入参数，不要将“值传递”改为“const引用传递”，否则既没有效率，也降低了函数的可理解性

      以上解决了两个面试问题：

      - 如果函数需要传入一个指针，是否需要为该指针加上const，把const加在指针不同的位置有什么区别；

      > 需要加上const，例 `func(int * a)` 
      >
      >   1. `int * const a`输入参数采用“值传递”，由于函数将自动产生临时变量用于复制该参数，该输入参数本来就无需保护，故不需要const来修饰
      >
      >      ***传递过来的参数及指针本身在函数内不可变,无意义***
      >
      >   2. `const int * a`给a加上const后，函数内的语句试图改动src的内容，编译器将指出错误。
      >
      >      ***参数指针所指内容为常量不可改变***

      - 如果写的函数需要传入的参数是一个复杂类型的实例，传入值参数或者引用参数有什么区别，什么时候需要为传入的引用参数加上const。

      > 区别：值传递需要产生与形参相同类型的临时对象复制形参，会降低效率，而引用参数仅是引用形参的名称，进行下来的函数操作，效率会大大提高
      >
      > 函数内可能改变形参的时候要使用const前缀

7. #### 类中使用const

   在一个类中,任何不会修改数据成员的函数都应该声明为const类型.如果在编写const成员函数的时候，不慎修改了 数据成员，或者调用了其他非const成员函数，编译时将指出错误，这无疑会提高程序的健壮性。

   使用const关键字进行说明的成员函数，称为常成员函数。只有常成员函数才有资格操作常量或常对象，没有使用const关键字进行说明的成员函数不能用来操作常对象。

   对于类中的const成员变量必须通过初始化列表进行初始化，如下所示：

   ```c++
   class Apple{
       private:
       int people[100];
       public:
       Apple(int i);
       const int apple_number；
   }；
       
   Apple::Apple(int i):apple_number(i)
   {
       
   }
   ```

   const对象只能访问const成员函数，而非const对象可以访问任意的成员函数，包括const成员函数。

   例如：

   ```c++
   //apple.cpp
   class Apple{
       private:
       int people[100];
       public:
       Apple(int i);
       const int apple_number;
       void take(int num) const;
       int add(int num);
       	int add(int num) const;
       int getCount() const;
   };
   //main.cpp
   #include<iostream>
   #include"apple.cpp"
   using namespace std;
   
   Apple::Apple(int i):apple_number(i)
   {
   
   }
   int Aple::add(int num){
       take(num);
   }
   int Apple::add(int num) const{
       take(num);
   }
   void Apple::take(int num) const{
   	cout<<"take func"<<num<<endl;
   }
   int Apple::getCount() const{
       take(1);
       //add(); //error
      	return apple_number;
   }
   int main(){
       Apple a(2);
       cout<<a.getCout()<<endl;
       a.add(10);
       const Apple b(3);
   	b.add(100);
       return 0;
   }
   ```

   result:

   ```txt
   take func 1
   2
   take func 10
   take func 100
   ```

   上面getCount()方法中调用了一个add方法，而add方法并非const修饰，所以运行报错。也就是说**const对象只能访问const成员函数**。

   而add方法又调用了const修饰的take方法，证明**非const对象可以访问任意的成员函数，包括const成员函数**。

   除此之外，我们也看到add的一个重载函数，也输出了两个结果，说明**const对象默认调用const成员函数。**

   我们除了上述的初始化const常量用初始化列表方式外，也可以通过下面方法：

   第一：将常量与static结合，也就是：

   ```c++
   static const int apple_number
   ```

   第二：在外面初始化：

   ```c++
   const int Apple::apple_number = 10;
   ```

   当然，在C++11中可直接在定义中直接初始化：

   ```c++
   static const int apple_number = 10;
   // or
   const int apple_number = 10;
   ```

   
