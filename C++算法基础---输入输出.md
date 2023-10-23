*说在前面：*
类似于 **ACM**、**蓝桥杯** 等这种算法类竞赛中，更多的使用的是输出结果，而不是**LeetCode**中简单的函数返回（大多数题目是这样）。如果对具体实现有思路，却不知如何输出结果，无疑是一种遗憾。
##### *所以，这里就来基于个人的掌握的知识说说这些输入输出技巧。*   
 *再次强调标题：在本篇文章中，讨论的都是C++的算法竞赛所需的很简单很简单的输入/输出技巧。* 
##### 输入篇
###### 1.a*b问题   
输入两个数，输出他们的乘积。  

*(-50000<=a,b<=50000)* 

 
输入样例：
```
2 2  
22222 22222
```
输出样例：
```
4
493817284
```
题目很简单，只要同时获取输入的两个数之后输出结果，注意：题目中给出了范围，大家都知道一般`int`是4 Byte，所以`int`的上限是`21474836473`。  由于这篇是讲输入输出，关于这个我们下次再说。
解答：
```cpp
#include <iostream>
using namespace std;
int main(){
    long long a,b;
    cin>>a>>b;
    //没必要提示别人输入，因为是机器自动输入
    cout<<a*b<<endl;
    //没必要再用变量存直接输出就行
    return 0;
    //在有些比赛中，常常需要返回0
}
``` 
这里要说的就是，`std::cin`和C中的`printf`一样，遇到空格和换行符就会停止，C++的`cin`使用流式传输（写出来的样子也像流过去），要同时通过输入给两个变量赋值，直接这样就可以了。   


###### 2.整数数组和问题   
输入任意个整数，输出他们的和。  

*结果保证不超过int范围* 

 
输入样例：
```
99 7 8
19 198 10 233 54
```
输出样例：
```
114
514
```
题目还是很简单，只要获取完输入一个数组之后输出结果。
解答如下：
```cpp
#include <iostream>
using namespace std;
int main(){
    int sum;
    sum=0;
    //存放和,初始化0
    int a;
    //没必要特意定义数组
    while(cin>>a){
        sum+=a;
    }
    cout<<sum<<endl;
    return 0;
}
``` 
这里，使用while循环不断获取输入就可以了。自己测试的时候怎么办？试试在输入结束按下`Ctrl+Z`再回车。   

这题是每接受到一个空格结束一次输入就会给`a`赋值一次。这时会联想到我们刚刚提到的:
>`std::cin`和C中的`printf`一样，遇到空格和换行符就会停止。   

那么，阁下又将如何应对呢？   

在此之前，我们先来熟悉下字符串用法。
###### 3.粗心的小川   
  小川在用计算器做两个数字的加法作业的时候向计算机输入算式时总是把0打成了o，由于马上就要交作业，来不及了！请你帮他改正，并且以行输出正确的算式和结果。

*结果保证不超过int范围* 

 
输入样例：
```
1oooo+3 4o9o+520
```
输出样例：
```
10000+3=10003
4090+520=4610
```
题目还是很简单，扫描一遍字符串把其中的o修改为0即可。
解答如下：
```cpp
#include <iostream>
#include <string>
using namespace std;
void setNum(char c, int &a)
//引用传递a，区分一下这里并不叫地址传递(C中），而是“起别名”。
{
    //分析每个char并记录，因为是从高位到低位记录的，所以如此
    if (c == 'o')
    {
        a = a * 10;
    }
    else
    {
        a = a * 10 + (c - '0');
    }
}
int main()
{
    string str;
    cin >> str;
    bool flag = 1;
    //区别被加数和加数
    int a, b;
    a = b = 0;
    for (auto c : str)
    { //增强for
        if (flag)
        {
            if (c == '+')
            {
                flag = !flag;
                continue;
            }             //碰到加号之后就会记录加数
            setNum(c, a); //记录被加数
        }
        else
        { //加数
            setNum(c, b);
        }
    }
    cout << a << '+' << b << '=' << a + b << endl;
    return 0;
}
``` 
这里，想要强调的仅仅是`a = a * 10 + (c - '0');` 这里主要是由于字符型是由`ASCII码`表示，要计算数字是什么只要当前字符序号减掉`0`所在序号即可得到需要的数字。注意一定先减后加，不然可能产生意想不到的奇奇怪怪的溢出。



下面再来解决我们的留下的疑惑：   

>`std::cin`和C中的`printf`一样，遇到空格和换行符就会停止。  

###### 4.幸运笔记 Lucky Note
据说，想着一个人的样子并在幸运笔记本上写下这个人的名字，这个人就会幸福永远。
但幸运笔记本要求必须将其名字的英文单词表示或是拼音字母表示写为大写，否则幸运笔记本对所写之人无效。
请你编写一个程序，将输入字符中的小写全部转化为大写，并输出正确内容。 
  *输入保证不会太离谱。* 
 
输入样例：
```
amane miSa,zhang sAn,Li si,everyone
```
输出样例：
```
AMANE MISA,ZHANG SAN,LI SI,EVERYONE
```
题目还是很简单，扫描一遍字符串把其中的小写字母修改为大写即可。但是很明显名字的个数我们不知道，名字几个空格也不知道。先看代码，待会就懂了。
实际上呢有两种常见方法：
方法一：
```cpp
#include <iostream>
using namespace std;
int main()
{
    char c;
    while((c=getchar())!=EOF){
        // 循环读取字符，遇到换行符或EOF时结束
        if('a'<=c&&c<='z')
            c-=32;//ASCII码特点
        cout<<c;//putchar(c);
    }
    return 0;
}
``` 
方法二：
```cpp
#include <iostream>
#include <string>
using namespace std;
int main()
{
    string names;
    getline(cin,names);
    for(auto c:names){
        if('a'<=c&&c<='z')
            c-=32;//ASCII码特点
        cout<<c;
    }
    cout<<endl;//刷新缓冲区
    return 0;
}
``` 
方法一中，如果直接`cin>>c`，那么所有的空格无法被获取，导致结果错误。这里使用C中的`getchar`，可以保存下来空格了，至于`EOF`是个啥，可以去了解一下C/C++的文件操作，这里不再赘述。   
方法二中，使用`getline`获取一整行包括空格的输入赋值给C++字符串，使用`cin`作为输入函数。  
你可能会问：获取不知道多少行时，存在换行又怎么办呢？随机应变，套在while里面就行了。
```cpp
while(getline(cin,names))
```

至此，基本的输入环节结束了。相信了解这些后读者对于获取算法比赛中的输入应该没什么问题了。  

##### 输出篇


在C语言中，`printf`是一个功能很强大的输出函数。C++的`cout`亦是如此。 
###### 进制转换
C语言中，可以在使用`printf`时设置，`%d`表示十进制（所以可以理解为什么C中是%d），`%o`表示八进制，`%x`则表示十六进制。
```c
printf("%d",128);
//128
printf("%o",128);
//200
printf("%x",128);
//80
```
C++中，可以在使用`cout`时设置，`dec`表示十进制（所以可以理解为什么C中是%d），`oct`表示八进制，`hex`则表示十六进制。
```cpp
cout<<dec<<128;
//128
cout<<128;
//128
cout<<oct<<128;
//200
cout<<hex<<128;
//80
```
很显然，这个特性可以用来进行10、8、16进制之间的相互转换。
###### 5.老八爱数字
老八很喜欢可以被八整除的数，并且会把他们除以8之后的数字的八进制表示记录下来,对不喜欢的数，他会说NO。
编写一个程序，若该数可以被8整除，按照老八的喜好记录，否则输出NO。
  *输入保证不会太离谱。* 
 
输入样例：
```
298
648
8848
```
输出样例：
```
NO
121
2122
```
当然题目有别的做法，这里只是展示功能：
```cpp
#include <iostream>
using namespace std;
int main()
{
    int a;
    cin>>a;
    if(a%8==0){
        cout<<oct<<a/8<<endl;
    }
    else
        cout<<"NO"<<endl;
    return 0;
}
``` 


注意：下面的操作需要包含`iomanip`头文件。

即在程序开头加上：   
```cpp
#include <iomanip>
```
###### 设置宽度
C语言中`%d`中间加上一个数字就可以表示宽度（设置宽度后默认右对齐）。注意，小数点也算一个宽度。
```cpp
printf("%5d",66);
//不足5位补空格
//   66
```
C++则使用`setw`函数。（与C一样设置宽度后默认右对齐）注意，小数点也算一个宽度。
```cpp
cout<<setw(5)<<66;
//不足5位补空格
//   66
```


###### 设置左右对齐
C语言中，默认左对齐，但若设置了宽度则右对齐，此时在数字前面（没有的话就是数字那个位置）加个‘-’即可左对齐。
```cpp
printf("%5d",66);
//右侧对齐
//   66
printf("%-5d",66);
//66   （有空格的！）
```
C++也是默认左对齐，设置宽度后右对齐，使用`left`来表示左对齐，`right`表示右对齐。
```cpp
cout<<left<<setw(5)<<66;
//不足5位右侧补空格
//66
cout<<right<<setw(5)<<66;
//不足5位左侧补空格
//   66
```



###### 设置填充字符
C中在数字前面加0表示补0，否则补空格。
```cpp
printf("%05d",66);
//不足5位左侧补（右对齐）
//00066
```
C++中则是用`setfill`来指定填充的字符。
```cpp
cout<<setfill('*')<<setw(5)<<66;
//不足5位左侧补‘*’
//***66
```
###### 浮点数输出
C中在‘%’后面加个小数点，后面接的数字表示保留的小数位,小数点前加数字则设置右对齐宽度，不足补空格（加0则补0）,会自动四舍五入。
```cpp
printf("%09.2f",3.146);
//不足5位左侧补0
//000003.15
```
C++ 使用`setprecision`可以指定保留小数点后位数，若也使用了`fixed`则可进行四舍五入。
```cpp
cout<<setfill('*')<<setw(6)
    <<3.146<<endl;
cout<<fixed<<setprecision(2)<<setfill('*')<<setw(9)
    <<3.146;
//不足5位左侧补*
//*3.146
//*****3.15
//不使用fixed上面的数字就是3.14了
```
其实也都比较简单就不继续举出例题了。
##### 其他要说的
输入输出说完了，顺便提一嘴string吧。
###### string的其他用法   

`stoi`、`stod`、`stof`函数,返回值是将字符串转化为`int`、`double`、`float`
```cpp
#include <iostream>
#include <string>
using namespace std;
int main() {
    string str = "42";
    int number = stoi(str);
    cout << "Number: " << number << endl;
    //Number: 42
    str = "3.14159";
    double doubleNumber = stod(str);
    cout << "Double Number: " << doubleNumber << endl;
    //Double Number: 3.14159
    str = "2.71828";
    float floatNumber = stof(str);
    cout << "Float Number: " << floatNumber << endl;
    //Float Number: 2.71828
    return 0;
}
```
`to_string`，将其他类型转化成字符串。
```cpp
#include <iostream>
#include <string>
using namespace std;
int main() {
    int number = 42;
    double pi = 3.14159;
    float floatValue = 2.71828f;

    string numberStr = to_string(number);
    string piStr = to_string(pi);
    string floatStr = to_string(floatValue);

    cout << "Number as string: " << numberStr << endl;
    cout << "Pi as string: " << piStr << endl;
    cout << "Float value as string: " << floatStr << endl;
    //Number as string: 42
    //Pi as string: 3.141590
    //Float value as string: 2.718280
    return 0;
}
```
使用`sstream`头文件，可以使字符串进行流处理，对于处理输入输出、数据转换和字符串操作非常有用。
```cpp
#include <iostream>
#include <sstream>
using namespace std;

int main() {
    string str = "123";
    int number;
    stringstream ss(str);
    ss >> number;  // 字符串转换为整数，传入number中
    
    double pi = 3.14159;
    stringstream ss2;
    ss2 << pi;  // 浮点数转换为字符串，传入ss2中
    string piStr = ss2.str();
    
    cout << "Num: " << number << endl;
    //Num: 123
    cout << "Pi(string): " << piStr << endl;
    //Pi(string): 3.14159
    return 0;
}
```
###### using namespace std;
本文代码都使用了`using namespace std;`，但实际开发中可能不这么做，具体原因可自行查阅。
###### `cin`、`cout`和`scanf`、`printf`
大家可能都听说过，`cin`和`cout`还有C++字符串`string`有关操作速度上比C要慢很多。   
但有一种说法是，C++是因为需要兼容C的输入输出才减慢速度。
这里分享一个很多*ACMer*都在用的技巧：
```cpp
ios::sync_with_stdio(false);
cin.tie(0);
cout.tie(0);
```
目的就是关闭C++的输入输出与C的同步，~~（听说这样比C中的输入输出更快）~~如果要这么使用，请确保你的输入输出只有C++版本或C版本，并且C++中需要记得在下一次输入前刷新你的缓冲区，否则输出结果可能出现错误。   

最简单的方法就是换行：
```cpp
cout<<endl;
```
或者直接刷新：
```cpp
cout.flush();
```
纸上得来终觉浅，绝知此事要躬行。   
本文如果对你有所帮助，记得多加练习。
