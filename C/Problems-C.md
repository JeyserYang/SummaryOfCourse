---
layout: post
title:  C Problems (Important)
date:   2019-01-09
categories: [C]
tags: [C]
---

## (most significant) 指针问题
- 空指针的含义
```
int *p = NULL;
//这句话等同于 int *p;
printf("%d", &p);
```
> 什么情况我们才会认为是空指针呢，也就是指针已经存在，但其指向的内容不存在，从计算机的角度看，就像是指针所代表的值全是二进制的0 显然我们不能访问地址为0的内容，也就把这个当作是空指针。从上面的代码还将看出，虽然指针所指向的内容不存在，但指针本身是有地址的。下面的图示会显示的更清楚：

![NULL Point]({{site.url}}/img/NULL_point.jpg)

- 指针常量和常量指针`const int *p, int *const p`
```
 int n = 0, m = 15;
 const int *p = &n;
 //*p = 10;  compile error.
 n = 10;
 int *r = &n;
 *r = 5;
 printf("%d", n);
 int *const q = &n;
 //q = &m;	compile error.
```
> 1：从以上的代码可以看出，`const int *p ` 我们唯一不能做的就是使用`*p`来修改n的值，这通常是应用最广泛的，例如在函数的参数中使用 `const void *a` ，在函数内部无法改变`*a`的值。可以看到，将n重新赋值或者重新生成一个指针指向n都可以改变n的值。
> 2：`int *const q` 中代表指针q是一个常量，q不能指向任何其他的地方，在上面的图中，也就表示q的值不变。但`*q`也就是n的值仍然可以改变。
- 指针结构体问题（具体可参见PAT （Basic Level ）1004.c内容）
(可以扩展为双重指针或更多重指针问题 `int **`）总结如下：
```
typedef struct _STUDENT
{
    char *name;
    char *stuNumber;
    int grade;
}*STUDENT;
```
> 当我们以上面的方式定义结构体指针时,此时的STUDENT就已经是一个指针了,当我们再定义一个数组时,数组名代表的就是`struct _STUDENT **` 类型数据了,当我们使用以下操作分配内存时,具体的分配是什么的内存一定要搞清楚:
```
STUDENT students[5];
STUDENT *stu = (STUDENT *)malloc(sizeof(STUDENT) * 5);

printf("%d ", sizeof(students));
printf("%d ", sizeof(STUDENT));
printf("%d ", sizeof(stu));
printf("%d ", sizeof(struct _STUDENT));

//output:
//20 4 4 12

```
> 上面的代码中为什么STUDENT和stu的大小是相同的呢?事实上,它们都是指针,当然大小相同.而像上面这种语句只是分配了指针的存储空间,因此此时students[i]或者stu[i]都是一个指针,这个指针的内容未知,可以理解为上面的NULL.也正是因为此,我们再初始化时,仍然需要产生分配真正与数据直接相关的结构体`struct _STUDENT`的存储空间,这样我们才能使用这个students指针,也就是像下面这样:
```
for(i=0;i<n;i++)
{
    //也就是说，在给一个结构体指针数组输入值时，需要先定义一个STUDENT *变量，并对其
    //分配内存，然后输入，再将其赋给我们要运算的数组中去。
    STUDENT s = (STUDENT)malloc(sizeof(struct _STUDENT));
    scanf("%s %s %d", s->name, s->stuNumber, &s->grade);
    students[i] = s;    //这句话相当于指针的赋值。
    //下面这个语句在编译时不会报错，但无法正常运行，应当注意。
    //scanf("%s %s %d", students[i]->name, students[i]->stuNumber, &students[i]->grade);
}
```
> 上上面的代码中还有一个值得注意的，就是结构体指针中的数据成员也是一个指针`char *`，此时打印结构体的大小时，可以看到是12个字节，正好等一一个int， 两个指针的大小之和，同样的如果没有对结构体中的指针例如name没有让他指向具体的量，那么它也是不同读的。也就是上面代码中使用%s 来输入 s->name 也是会报错的。
- 指针和数组的细微差别
当我们像下面这样定义一个数组和指针时，大多数情况下操作是可以共通的，也有例外。
```
int a[3] = { 1, 2, 3 };
int *p = a;
int i;
for(i=0;i<3;i++)
    printf("%d ", p[i]);
for(i=0;i<3;i++)
    printf("%d ", a[i]);
for(i=0;i<3;i++)
    printf("%d ", *(a+i));
for(i=0;i<3;i++)
    printf("%d ", *(p+i));
for(i=0;i<3;i++)
    printf("%d ", *p++);
/* compile error!!!
for(i=0;i<3;i++)
    printf("%d ", *a++);*/
```
必须指出，在指针和数组的运算中，前5中情况都是等同的，只有最后一种不可以，也就是数组名不能**自增**。其实也不难理解，如果数组名自增了，整个数组就变化了，这怎么可能呢。

- 一个有趣但错误的示例 （永远也不要这样写）
```
char *name;
//scanf("%s", name)			//compile error!
scanf("%s", &name);
printf("%s\n", &name);
printf("%d\n", sizeof(&name));
printf("%d\n", sizeof(name));

//input:	444556
//output:
444556
4
4
```
> 第一个输入编译错误，显然是由于name没有指向任何东西，可以看作指向NULL。从上面的输出可以看到指针占4个字节的大小，我们在向只有四个字节大小的指针区域写入数据，这有可能导致程序运行崩溃，因为有可能写到程序无法访问的内存空间。name本身就是指针。

- Dynamic memory free
```
int *p = &a;
free(p);    p = null;
```
> free 函数释放的是指针所指向的目标数据的内存，因此free 操作后，指针p仍然在内存中，所以我们在free函数调用后， 应当将p = null 
 
在指针问题中，要善于利用**`sizeof`**方法来具体理解内存的分配。

## EOF, NULL('\0'), 回车('\r'), 换行('\n')的区别。
- EOF 是 end of file 的缩写，表示 “文字流”的意思， 说白了，就是（stream)类型的结尾标志。一般的（stream）包括，标准输入输出（scanf等）， 文件（file）， 打印机等。也就是说如果检查这些东西到没到结尾，应该使用EOF标志。且在C语言中，它是定义在stdio.h的常量， 值为 -1  `#define EOF -1`

- NULL('\0') 显它的值为 0 . 它被用来表示字符串的结尾。也就是说，判断一个字符串到没到结尾，应该使用'\0'

- 回车('\r') && 换行('\n') (了解了历史才能对这个有比较清楚的把握)
> 计算机还没有出现之前，有一种叫做电传打字机（Teletype Model 33），每秒钟可以打10个字符。但是它有一个问题，就是打完一行换行的时候，要用去0.2秒，正好可以打两个字符，要是在这0.2秒里面，又有新的字符传过来，那么这个字符将丢失。 于是，研制人员想了个办法解决这个问题，就是在每行后面加两个表示结束的字符。一个叫做“回车”，告诉打字机把打印头定位在左边界；另一个叫做“换行”，告诉打字机把纸向下移一行。 这就是“换行”和“回车”的来历，从它们的英语名字上也可以看出一二。 后来，计算机发明了，这两个概念也就被般到了计算机上。那时，存储器很贵，一些科学家认为在每行结尾加两个字符太浪费了，加一个就可以。于是，就出现了分歧。Unix 系统里，每行结尾只有“<换行>”，即“\n”；Windows系统里面，每行结尾是“<回车><换行>”，即“ \r\n”；Mac系统里，每行结尾是“<回车>”。一个直接后果是，Unix/Mac系统下的文件在Windows里打开的话，所有文字会变成一行；而Windows里的文件在Unix/Mac下打开的话，在每行的结尾可能会多出一个^M符号。

总结就是， 现在光标已经到了一行末尾，回车是让它到一行开始，换行是让它到下一行。

> gets（直至接受到换行符或EOF，换行符不作为读取串的内容，读取的换行符被转换为null值，并由此来结束字符串）和scanf（tab，空格，换行，回车都作为结束符）都不同。

## 常见问题以及函数使用
1：	获取标准输入的一行，以换行为结束，且希望存入的字符数组 char[] 是不定长的，一般可以使用 `gets()` 函数（以换行为结束标志），`scanf() `函数以空格，换行等为一个缓冲单位也可以使用 。 `getchar()` 函数一个一个输入字符来实现字符串输入。一般当要求输入的正整数较大时，都是考虑使用字符数组的形式输入的。

2：	//将一个正整数转换为字符串
```
void itos(int num,char *str)
{
    int i=0;
    int number = 0;
    while(1)
    {
       	number = num % 10;
        str[i] = number + '0';    i++;
        if(num >= 10)   num /= 10;
        else return ;
    }
    return;
}
```
3： //获得str字符串的字串，放入subString
```
void getSubString(char *str, int beginN, int endN, char *subString)
{
    int i,j=0;
    for(i=beginN;i<=endN;i++)
    {
        subString[j] = str[i];
        j++;
    }
}
```
4:  在1004.c的代码中结构体数组的两种输入方法必须会使用。尤其注意第二种输入的方式以及别忘了使用free函数。

5： //判断一个数是否是素数
```
int IsPrime(int x)
{
    int i, t;
    if(x == 2)    return 1;
    if(x%2 == 0)        return 0;
    t = (unsigned int) sqrt(x) + 1;
    i = 3;
    while(i <= t)
    {
        if(x%i == 0)     return 0;
        i += 2;
    }
    return 1;
}
```
6： 输出格式最后不包含空格的方式
```
for(i=0;i<n-1;i++)
    printf("%d ", a[i]);
printf("%d", a[i]);

for(i=0;i<n;i++)
{
    if(i!=0)
        printf(" ");
    printf("%d", a[i]);
}
```
7:  scanf函数的输入可以这样使用（需要积累），输入一行，也可以使用scanf函数，通过判定是否是EOF来达到gets的作用。
```
    while(scanf("%d %d", &coef, &index) != EOF) if(index)
    {   /* constant terms results in zero term, so no output for index = 0 */
        if(count++ != 0)
          printf(" ");
        printf("%d %d", coef * index, index - 1);
    }
```
8： 一个n位数，每位数都相同，获得数的大小（参与计算时）的方法。
`p_A = 0;`
    然后循环执行下面这条语句就行了。
`P_A = P_A * 10 + D_A;`

9： 在1020.c中，有一种不用删除数组中的元素就可以进行判定的情况。当数组中元素全部大于0时才有意义，那么将我们
    需要删除的位置赋成0直接就可以达到删除的效果。（这是一种删除的等效方法）。

10：在日期的大小比较中，我们可以将yyyy/mm/dd形式的日期转换为int型数
    int birthday = yyyy*10000 + mm*100 + dd;
    这样大小比较时（可以当成数来比较了）就很简单了。排序也简单了。
    同时显然这中方法也可以放在时间上。
    int date = HH * 3600 + MM * 60 + SS；

11： 用于动态分配内存的5个预定义的宏
```
#define NewObject(T)        (T *)malloc(sizeof(T))
#define CreateObject(T, n)  (T *)malloc(sizeof(T) *n)
#define CreateObjects(T, n) (T *)malloc(sizeof(T) *n)
#define DestroyObject(p)    free(p);    p = null
#define DestroyObjects(p)   free(p);    p = null
```
> 我们可以看到二三，四五的宏定义完全一样，这是为了不同的设计情况考虑的。当我们将整个数组中的元素当成一个整体时，我们应该使用CreateObject，典型的就是`Char *p`;被当成字符串来看待。其他的普通数组一般都采用CreateObjects。

12： 抽象数据类型C
```
typedef void *ADT;
typedef const void *CADT;
```
> 在C语言中，没有显示的抽象数据类型，只能使用`void *`来表达任意类型数据的抽象。同时，在上面这种情况，我更推荐使用`void *`，而不是ADT，虽然封装是好的，但是写成`void *`对于我们理解指针更有帮助。

13: 函数指针
```
//返回类型 （*函数指针名称）（参数列表）;
char *(*as_string)(void *object);

char *func(void *object){
    return "yang";
}

as_string = func;

//与下面的赋值等效
char *p;
char a[10];
p = a;
```
> 在上面的函数指针中，我们将as_string看作一个变量，它可以指向任何且参数必须为`void *`，返回值为`char *`的函数。下面的func函数正好符合这样一个定义，所以可以将函数名之间相互赋值。事实上，普通函数名本身也是一个指针， as_string和func之间的赋值与普通的指针赋值没有什么区别。
> 函数指针最大的用途：用于**回调函数**，也就是函数指针作为参数使用，如下所示(qsort)：
```
void qsort(void *base, unsigned int number_of_elements, unsigned int size_of_elements,
    int (*compare)(const void *, const void *));
```
> 上面是C函数库qsort函数的函数原型，其中compare就是一个函数指针，我们可以自定义许多不同的compare函数来进行比较，只要我们自己写的函数是返回值为int， 参数为两个`const void *`。
> `typedef int (*COMPARE)(CADT e1, CADT e2)` 意味着compare不再是函数指针变量名，而是函数指针类型名，同结构体，enum类似。我们就可以像普通数据类型一样使用它们。也就是说可以像下面这样`COMPARE compare;`定义变量，同时可以赋值。

14： 内联函数（inline）
> 在C语言中，如果一些函数被频繁调用，不断地有函数入栈，即函数栈，会造成栈空间或栈内存的大量消耗。为了解决这个问题，特别的引入了inline修饰符，表示为内联函数.
```
#include <stdio.h>  

//函数定义为inline即:内联函数  
inline char* dbtest(int a) 
{  
	return (i % 2 > 0) ? "奇" : "偶";  
}   
  
int main()  
{  
	int i = 0;  
	for (i=1; i < 100; i++) 
	{  
		printf("i:%d    奇偶性:%s /n", i, dbtest(i));      
	}  
}
```
> 上面的例子就是标准的内联函数的用法，使用inline修饰带来的好处我们表面看不出来，其实在内部的工作就是在每个for循环的内部任何调用dbtest(i)的地方都换成了(i%2>0)?"奇":"偶"这样就避免了频繁调用函数对栈内存重复开辟所带来的消耗。
> Note: 关键字inline 必须与函数定义(实现)体放在一起才能使函数成为内联，仅将inline 放在函数声明前面不起任何作用。
```
//This is wrong.
inline void Foo(int x, int y); // inline 仅与函数声明放在一起
void Foo(int x, int y)	{}
//Correct
void Foo(int x, int y);
inline void Foo(int x, int y) 	{}		// inline 与函数定义体放在一起
```
Note: 内联函数的实质就是个空间代价换时间的节省。
> - 所以内联函数一般都是1-5行的小函数
> - 在内联函数内不允许使用for,while和switch语句

15： extern关键字
> 一旦声明了extern关键字，对编译器来意味着：
> - 这个变量声明（即数据类型和变量名，但是编译器并没有分配内存）
> - 这个变量的定义在其他文件中(在定义变量的文件中编译器才会为其分配内存)
> - extern变量可以声明多次，但是只能初始化一次.
> - 最好将extern变量的声明放在头文件中，将变量的定义放在一个源文件中。
Note: extern修饰的变量必须是全局变量，每次使用时都必须声明以下。比如我在demo.h中声明了extern变量a，在demo.c中定义了a的值为10，此时我要在main（其他任意）函数使用它，则只需要在使用的时候声明以下就可以了`extern int a;`

16： `void *`的使用
> void指针可以指向任意类型的数据，就是说可以用任意类型的指针对void指针赋值，例如：
```
int *a;
void *p;
p = a;
```
> 如果要将void指针p输出，则需要强制类型转换，就本例而言：`printf("%d", *(int *)p);`。
> 我们经常使用的malloc函数就是一个返回void指针的函数，由于我们不知道该指针指向的内存存放的数据类型，因此需要强制类型转换，`(int *)malloc(sizeof(int));`