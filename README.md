# Glimmer-CS--MEDIUM---01-
1.因为基本数据类型的内存范围有限，无法存储非常大的数。‌ 当数值超过int、long、long long等类型所能表示的最大值时，就需要使用其他方法来存储和计算这些大数。字符数组是一种有效的解决方案，因为它可以将大数分解为多个字符进行存储和计算，从而避免溢出问题‌。

2.原则还是10进1，但因为是字符串，所以加的时候是从字符数组的最大位开始，实际上就是数的最小位，计算时要用“-‘0’”来让字符变成数，之后再加回来，结果需要另外一个字符数组来存储，从该字符数组首位开始存储，不过减法中由于借位的缘故可能会出现前导零，所以还要清除无意义的前导零，又因为是倒序的所以最后还有一个逆序的过程。但这里我还了解到了另外一种办法，我之前的写法是不需要将数组初始化的，但若是将数组都初始化为零的话，那么逆序的步骤就能够直接省掉，可以从结果数组的尾部开始按顺序存入结果，然后再用memove函数将前面的前导零覆盖。

3.负数就在字符数组中通过判断“-”来计算，我个人的想法是根据两个数的符号来进行不同形式的运算。但我现在感觉之前的办法比较麻烦，而且有很多重叠步骤，我是直接将“-”的判断放在了运算函数中，但其实更好的做法应该是在函数外就进行判断，然后再使用一些手段来实现符号的变化，比如创建一个量来用于判断结果是否为负数，通过它的值来代表。
##step1
~~~c
#include<stdio.h>
int main()
{
    char a[40];
    scanf("%s",a);
    printf("%-39s",a);
    return 0;
}
~~~
<img width="278" alt="CS-MEDIUM-00" src="https://github.com/user-attachments/assets/39c3e36b-664a-4389-960c-86e9c068e50a">
##step2
~~~c
#include<stdio.h>
#include<string.h>
int main()
{
    char a[40];
    char b[40];
    char result[41];
    scanf("%s",a);
    scanf("%s",b);
    int len1=strlen(a);
    int len2=strlen(b);
    int add=0;
    int c=0;
    int i=len1-1;
    int j=len2-1;
    while (i>=0||j>=0||add)
    {   int sum=0;
        if(i>=0)
        {
            sum+=a[i--]-'0';
        }
        if(j>=0)
        {
            sum+=b[j--]-'0';
        }
        sum+=add;
        result[c++]=(sum%10)+'0';
        add=sum/10;
    }
    for(int d=0;d<c/2;d++)
    {
        char num=result[d];
        result[d]=result[c-1-d];
        result[c-1-d]=num;
    }
    result[c]='\0';
    printf("%s",result);
    return 0;
}
~~~
