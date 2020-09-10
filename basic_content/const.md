### const

1. 类型修饰符const表示常量或对象的值是不可以改变的

2. const的作用

   1. 定义常量

      `const int a=100`

   2. 类型检查

      `const与#define宏定义的区别:const具有类型可以被执行安全检查,而宏定义只是做简单的字符替换,没有安全检查`

   3. 防止修改起保护作用

   4. 节省空间

      `汇编语言的角度来看,const在程序运行的过程中,仅有一份内存拷贝,而#define定义的常量在运行的过程中,有若干个拷贝`

3. const对象默认为文件的局部变量

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
   
4. 定义常量

   ```C++
   const int b=10;
   b=0;//error
   const string s="helloworld"
   const int i,j=0//error
   ```

   错误：

   * b为常量不可更改
   * i为常量，在**定义时就要进行初始化**

5. 指针与const

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
