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
<img width="443" alt="CS-MEDIUM-02" src="https://github.com/user-attachments/assets/b33f8ba3-ba44-4531-98e4-831657a57e2f">

##step3
~~~c
#include<stdio.h>
#include<string.h>
void delete(char a[])
{   
    for(int i=0;a[i]!='\0';i++)
    {
        a[i]=a[i+1];
    }
}
void minus(char a[],char b[],char result[])
{
    int len1=strlen(a);
    int len2=strlen(b);
    int borrow=0;
    int c=0;
    int N=0;
    int i=len1-1;
    int j=len2-1;
    if (len1 < len2 || (len1 == len2 && strcmp(a, b) < 0)) {
        N = 1;
        char *x = a;
        a=b;
        b=x;
        int t=len1;
        len1=len2;
        len2=t;
    }
    while (i>=0||j>=0)
    {   
        int diff=borrow;
        diff+=a[i--]-'0';
        if(j>=0)
        diff-=b[j--]-'0';
        if(diff<0)
        {
            diff+=10;
            borrow=-1;
        }else borrow=0;
        result[c++]=diff+'0';
    }
    while (c>1&&result[c-1]==0)
    {
        c--;
    }
    if(N==1)
    result[c++]='-';
    result[c]='\0';
    for(int d=0;d<c/2;d++)
    {
        char num=result[d];
        result[d]=result[c-1-d];
        result[c-1-d]=num;
    }
}
void plus(char a[],char b[],char result[])
{   
    int N=0;
    if(a[0]!='-'&&b[0]=='-')
    {
        delete(b);
        minus(a,b,result);
    }
    else if(a[0]=='-'&&b[0]!='-')
    {   
        delete(a);
        char*x=a;
        a=b;
        b=x;
        minus(a,b,result);
    }else
    {
        if(a[0]=='-'&&b[0]=='-')
        {
            delete(a);
            delete(b);
            N=1;
        }
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
    if(N==1)
    result[c++]='-';
    result[c]='\0';
    for(int d=0;d<c/2;d++)
    {
        char num=result[d];
        result[d]=result[c-1-d];
        result[c-1-d]=num;
    }
    }
}

int main()
{
    char a[41];
    char b[41];
    char result[42];
    int n=0;
    scanf("%s",a);
    scanf("%s",b);
    char a1=a[0];
    char b1=b[0];
    plus(a,b,result);
    printf("%s",result);
    return 0;
}
~~~
这个地方我是直接就进行了加减的运算，因为有负数的话，那加法可能就直接变成减法了  
<img width="443" alt="CS-MEDIUM-02" src="https://github.com/user-attachments/assets/27ce3c18-3766-4cc4-b444-7a819302b298">

##*part3*
~~~c
#include<stdio.h>
#include<string.h>
void circle(char* a)
{   int i=0;
    if(a[0]=='(')
    {
        while(a[i]!='\0')
        {
        a[i]=a[i+1];
        i++;
        if(a[i]==')')
        a[i-1]='\0';
        }
    }
}
int main()
{   char result[45];
    char a[100];
    char n1[43];
    char n2[43];
    char order;
    scanf("%s",a);
    char* search=strpbrk(a,"+-");
    strncpy(n1,a,search-a);
    circle(n1);
    order=*search;
    strcpy(n2,search+1);
    circle(n2);
    printf("%s\n",n1);
    printf("%c\n",order);
    printf("%s\n",n2);
}
~~~
<img width="572" alt="CS-MEDIUM-03" src="https://github.com/user-attachments/assets/9c900968-a179-4bd2-8cc3-efdf5f55a7f7">

##*part4*
~~~c
#include<stdio.h>
#include<string.h>
void delete(char a[])//删去负数前的负号，方便之后的计算
{   
    for(int i=0;a[i]!='\0';i++)
    {
        a[i]=a[i+1];
    }
}
void plus(char a[],char b[],char result[]);
void minus(char a[],char b[],char result[])
{    //判断符号
     if(a[0]!='-'&&b[0]=='-')
    {   
        delete(b);
        plus(a,b,result);
    } 
    else
    {
    if (a[0]=='-'&&b[0]=='-')
    {
        delete(b);
        delete(a);
    }
    if(a[0]=='-'&&b[0]!='-')
    {
        delete(a);
    }
    int len1=strlen(a);
    int len2=strlen(b);
    int borrow=0;
    int c=0;
    int N=0;
    int i=len1-1;
    int j=len2-1;
    if (len1<len2||(len1==len2&&strcmp(a, b)<0)) {
        N = 1;
        char *x = a;
        a=b;
        b=x;
        int t=len1;
        len1=len2;
        len2=t;
    }
    while (i>=0||j>=0)
    {   
        int diff=borrow;
        diff+=a[i--]-'0';
        if(j>=0)
        diff-=b[j--]-'0';
        if(diff<0)
        {
            diff+=10;
            borrow=-1;
        }else borrow=0;
        result[c++]=diff+'0';
    }
    while (c>1&&result[c-1]=='0')
    {
        c--;
    }
    if(N==1)
    result[c++]='-';
    result[c]='\0';
    for(int d=0;d<c/2;d++)
    {
        char num=result[d];
        result[d]=result[c-1-d];
        result[c-1-d]=num;
    }
    }
}
void plus(char a[],char b[],char result[])
{   
    int N=0;
//判断符号
    if(a[0]!='-'&&b[0]=='-')
    {
        delete(b);
        minus(a,b,result);
    }
    else if(a[0]=='-'&&b[0]!='-')
    {   
        delete(a);
        char*x=a;
        a=b;
        b=x;
        minus(a,b,result);
    }else
    {
        if(a[0]=='-'&&b[0]=='-')
        {
            delete(a);
            delete(b);
            N=1;
        }
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
    if(N==1)
    result[c++]='-';
    result[c]='\0';
    for(int d=0;d<c/2;d++)
    {
        char num=result[d];
        result[d]=result[c-1-d];
        result[c-1-d]=num;
    }
    }
}
void multiply(char a[],char b[],char result[])
{   int N=0;
//判断符号
    if(a[0]=='-'&&b[0]!='-')
    {
        N=1;
        delete(a);
    }
    if(a[0]!='-'&&b[0]=='-')
    {
        N=1;
        delete(b);
    }
    if(a[0]=='-'&&b[0]=='-')
    {
        delete(a);
        delete(b);
    }
    int len1=strlen(a);
    int len2=strlen(b);
    int res[100]={0};
    int k=0;
    for(int i=len1-1;i>=0;i--)
    {
        for(int j=len2-1;j>=0;j--)
        {   k=len1+len2-i-j-2;
            res[k]+=(a[i]-'0')*(b[j]-'0');
            res[k+1]+=res[k]/10;
            res[k]%=10;
        }
    }
    for(int i=0;i<=k+1;i++)
    {
        result[i]=res[i]+'0';
    }
    if(N==1)
    {   if(result[k+1]=='0')
        result[k+1]='-';
        else{
        result[++k+1]='-';
        result[++k+1]='\0';
        }
    }
    else if(result[k+1]=='0')
    result[k+1]='\0';
    else result[++k+1]='\0';
    for(int d=0;d<(k+1)/2;d++)
    {
        char num=result[d];
        result[d]=result[k-d];
        result[k-d]=num;
    }
}
void division(char a[],char b[],char result[])
{   int N=0;
//判断符号
    if(a[0]=='-'&&b[0]!='-')
    {
        N=1;
        delete(a);
    }
    if(a[0]!='-'&&b[0]=='-')
    {
        N=1;
        delete(b);
    }
    if(a[0]=='-'&&b[0]=='-')
    {
        delete(a);
        delete(b);
    }
    int len1=strlen(a);
    int len2=strlen(b);
    int original=len2;
    if(len1<len2||(len1 == len2 && strcmp(a, b) < 0))
    {
        result[0]='0';
        result[1]='\0';
    }else
    { 
        if(len1>len2)
        {
            for(int i=len2;i<len1;i++)
            {
                b[i]='0';
            }
            b[len1]='\0';
        }
        len2=len1;
        int s=0;
        int res[50]={0};
        while(original<=len2)
        {
        while(len1>len2||(len1 == len2 && strcmp(a, b) >= 0))
        {   char f[50];
            minus(a,b,f);
            int i;
            for(i=0;f[i]!='\0';i++)
            {
                a[i]=f[i];
            }
            a[i]='\0';
            res[s]++;
            len1=strlen(a);
        }   
            b[--len2]='\0';
            s++;
        }
        int p=0;
        int g=0;
        if(N==1)
        {
            result[0]='-';
            p++;
        }
        while(g!=s)
        {
            result[p++]=res[g++]+'0';
        }
        result[p]='\0';
    }
}
//去掉括号
void circle(char* a)
{   int i=0;
    if(a[0]=='(')
    {
        while(a[i]!='\0')
        {
        a[i]=a[i+1];
        i++;
        if(a[i]==')')
        a[i-1]='\0';
        }
    }
}
int main()
{
    char result[50];
    char a[100];
    char n1[43];
    char n2[43];
    char order;
    scanf("%s",a);
    char* search=strpbrk(a,"+-*/");
    strncpy(n1,a,search-a);
    n1[search-a]='\0';
    order=*search;
    strcpy(n2,search+1);
    circle(n2);
    switch(order)
    {
        case '+':
            plus(n1,n2,result);
            break;
        case '-':
            minus(n1,n2,result);
            break;
        case '/':
            division(n1,n2,result);
            break;
        case '*':
            multiply(n1,n2,result);
            break;
    }
    printf("%s",result);
}
~~~
<img width="451" alt="CS-MEDIUM-04" src="https://github.com/user-attachments/assets/d2903f28-c838-46ba-911f-57de11785aff">

这里就像我之前提到的，确实没怎么处理好，能跑但不大舒服。就比如对于括号的处理我完全没弄好，我就只是简单地删去了，其实应该是因为有了括号就可以判断是负数了，所以就不用再在运算中去判断了，但我没有声明一个全局变量来进行对符号的判断，之前的确没有认真思考。



