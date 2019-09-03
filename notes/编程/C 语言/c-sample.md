# C 语言经典程序例子

## 数字组合问题

题目：有1、2、3、4个数字，能组成多少个互不相同且无重复数字的三位数？都是多少？ 
1.程序分析：可填在百位、十位、个位的数字都是1、2、3、4。组成所有的排列后再去 
　　　　　　掉不满足条件的排列。  
2.程序源代码： 
```c
main() 
{ 
int i,j,k; 
printf("\n"); 
for(i=1;i<5;i++)　　　　／*以下为三重循环*/ 
　for(j=1;j<5;j++)　 
　　for (k=1;k<5;k++) 
　　　{ 
　　　　if (i!=k&&i!=j&&j!=k) 　　　/*确保i、j、k三位互不相同*/ 
　　　　printf("%d,%d,%d\n",i,j,k); 
　　　} 
} 
```

## 利润提成

题目：企业发放的奖金根据利润提成。利润(I)低于或等于10万元时，奖金可提10%；利润高 
　　　于10万元，低于20万元时，低于10万元的部分按10%提成，高于10万元的部分，可可提 
　　　成7.5%；20万到40万之间时，高于20万元的部分，可提成5%；40万到60万之间时高于 
　　　40万元的部分，可提成3%；60万到100万之间时，高于60万元的部分，可提成1.5%，高于 
　　　100万元时，超过100万元的部分按1%提成，从键盘输入当月利润I，求应发放奖金总数？ 
1.程序分析：请利用数轴来分界，定位。注意定义时需把奖金定义成长整型。　　　　　　 
2.程序源代码： 
```c
main() 
{ 
long int i; 
int bonus1,bonus2,bonus4,bonus6,bonus10,bonus; 
scanf("%ld",&i); 
bonus1=100000*0.1;bonus2=bonus1+100000*0.75; 
bonus4=bonus2+200000*0.5; 
bonus6=bonus4+200000*0.3; 
bonus10=bonus6+400000*0.15; 
　if(i<=100000) 
　　bonus=i*0.1; 
　else if(i<=200000) 
　　　　　bonus=bonus1+(i-100000)*0.075; 
　　　　else if(i<=400000) 
　　　　　　　　bonus=bonus2+(i-200000)*0.05; 
　　　　　　　else if(i<=600000) 
　　　　　　　　　　　bonus=bonus4+(i-400000)*0.03; 
　　　　　　　　　　else if(i<=1000000) 
　　　　　　　　　　　　　　bonus=bonus6+(i-600000)*0.015; 
　　　　　　　　　　　　　else 
　　　　　　　　　　　　　　bonus=bonus10+(i-1000000)*0.01; 
printf("bonus=%d",bonus); 
}
``` 

## 满足双重条件的数字

题目：一个整数，它加上100后是一个完全平方数，再加上168又是一个完全平方数，请问该数是多少？ 
1.程序分析：在10万以内判断，先将该数加上100后再开方，再将该数加上268后再开方，如果开方后 
　　　　　　的结果满足如下条件，即是结果。请看具体分析： 
2.程序源代码： 
```c
#include "math.h" 
main() 
{ 
long int i,x,y,z; 
for (i=1;i<100000;i++) 
　{ x=sqrt(i+100); 　　/*x为加上100后开方后的结果*/ 
　　y=sqrt(i+268); 　　/*y为再加上168后开方后的结果*/ 
　　　if(x*x==i+100&&y*y==i+268)/*如果一个数的平方根的平方等于该数，这说明此数是完全平方数*/ 
　　　　printf("\n%ld\n",i); 
　} 
} ```
============================================================== 
【程序4】 
题目：输入某年某月某日，判断这一天是这一年的第几天？ 
1.程序分析：以3月5日为例，应该先把前两个月的加起来，然后再加上5天即本年的第几天，特殊 
　　　　　　情况，闰年且输入月份大于3时需考虑多加一天。 
2.程序源代码： 
```c
main() 
{ 
int day,month,year,sum,leap; 
printf("\nplease input year,month,day\n"); 
scanf("%d,%d,%d",&year,&month,&day); 
switch(month)/*先计算某月以前月份的总天数*/ 
{ 
　case 1:sum=0;break; 
　case 2:sum=31;break; 
　case 3:sum=59;break; 
　case 4:sum=90;break; 
　case 5:sum=120;break; 
　case 6:sum=151;break; 
　case 7:sum=181;break; 
　case 8:sum=212;break; 
　case 9:sum=243;break;
  case 10:sum=273;break; 
　case 11:sum=304;break; 
　case 12:sum=334;break; 
　default:printf("data error");break; 
} 
sum=sum+day; 　/*再加上某天的天数*/ 
　if(year%400==0||(year%4==0&&year%100!=0))/*判断是不是闰年*/ 
　　leap=1; 
　else 
　　leap=0; 
if(leap==1&&month>2)/*如果是闰年且月份大于2,总天数应该加一天*/ 
sum++; 
printf("It is the %dth day.",sum);} ```
============================================================== 
【程序5】 
题目：输入三个整数x,y,z，请把这三个数由小到大输出。 
1.程序分析：我们想办法把最小的数放到x上，先将x与y进行比较，如果x>y则将x与y的值进行交换， 
　　　　　　然后再用x与z进行比较，如果x>z则将x与z的值进行交换，这样能使x最小。 
2.程序源代码： 
```c
main() 
{ 
int x,y,z,t; 
scanf("%d%d%d",&x,&y,&z); 
if (x>y) 
{t=x;x=y;y=t;} /*交换x,y的值*/ 
if(x>z) 
{t=z;z=x;x=t;}/*交换x,z的值*/ 
if(y>z) 
{t=y;y=z;z=t;}/*交换z,y的值*/ 
printf("small to big: %d %d %d\n",x,y,z); 
} ```
============================================================== 
【程序6】 
题目：用*号输出字母C的图案。 
1.程序分析：可先用'*'号在纸上写出字母C，再分行输出。 
2.程序源代码： 
```
#include "stdio.h" 
main() 
{ 
printf("Hello C-world!\n"); 
printf(" ****\n"); 
printf(" *\n"); 
printf(" * \n"); 
printf(" ****\n"); 
} 
```
============================================================== 
【程序7】 
题目：输出特殊图案，请在c环境中运行，看一看，Very Beautiful! 
1.程序分析：字符共有256个。不同字符，图形不一样。　　　　　　 
2.程序源代码： 
#include "stdio.h" 
main() 
{ 
char a=176,b=219; 
printf("%c%c%c%c%c\n",b,a,a,a,b); 
printf("%c%c%c%c%c\n",a,b,a,b,a); 
printf("%c%c%c%c%c\n",a,a,b,a,a); 
printf("%c%c%c%c%c\n",a,b,a,b,a); 
printf("%c%c%c%c%c\n",b,a,a,a,b);} 
============================================================== 
【程序8】 
题目：输出9*9口诀。 
1.程序分析：分行与列考虑，共9行9列，i控制行，j控制列。 
2.程序源代码： 
#include "stdio.h" 
main() 
{ 
　int i,j,result; 
　printf("\n"); 
　for (i=1;i<10;i++) 
　　{ for(j=1;j<10;j++) 
　　　　{ 
　　　　　result=i*j; 
　　　　　printf("%d*%d=%-3d",i,j,result);/*-3d表示左对齐，占3位*/ 
　　　　} 
　　　printf("\n");/*每一行后换行*/ 
　　} 
} 
============================================================== 
【程序9】 
题目：要求输出国际象棋棋盘。 
1.程序分析：用i控制行，j来控制列，根据i+j的和的变化来控制输出黑方格，还是白方格。 
2.程序源代码： 
#include "stdio.h" 
main() 
{ 
int i,j; 
for(i=0;i<8;i++) 
　{ 
　　for(j=0;j<8;j++) 
　　　if((i+j)%2==0) 
　　　　printf("%c%c",219,219); 
　　　else 
　　　　printf(" "); 
　　　printf("\n"); 
　} 
} 
==============================================================  
【程序10】 
题目：打印楼梯，同时在楼梯上方打印两个笑脸。  
1.程序分析：用i控制行，j来控制列，j根据i的变化来控制输出黑方格的个数。 
2.程序源代码： 
#include "stdio.h" 
main() 
{ 
int i,j; 
printf("\1\1\n");/*输出两个笑脸*/ 
for(i=1;i<11;i++) 
　{ 
　for(j=1;j<=i;j++) 
　 　printf("%c%c",219,219); 
　printf("\n"); 
　} 
}  
 
 
  
 作者： zhlei81   2005-1-22 11:29 　 回复此发言    
 
--------------------------------------------------------------------------------
 
3 回复：经典Ｃ源程序100例  
 【程序11】 
题目：古典问题：有一对兔子，从出生后第3个月起每个月都生一对兔子，小兔子长到第三个月 
　　　后每个月又生一对兔子，假如兔子都不死，问每个月的兔子总数为多少？ 
1.程序分析：　兔子的规律为数列1,1,2,3,5,8,13,21.... 
2.程序源代码： 
main() 
{ 
long f1,f2; 
int i; 
f1=f2=1; 
for(i=1;i<=20;i++) 
　{ printf("%12ld %12ld",f1,f2); 
　　　if(i%2==0) printf("\n");/*控制输出，每行四个*/ 
　　　f1=f1+f2; /*前两个月加起来赋值给第三个月*/ 
　　　f2=f1+f2; /*前两个月加起来赋值给第三个月*/ 
　} 
} 
============================================================== 
【程序12】 
题目：判断101-200之间有多少个素数，并输出所有素数。 
1.程序分析：判断素数的方法：用一个数分别去除2到sqrt(这个数)，如果能被整除， 
　　　　　　则表明此数不是素数，反之是素数。 　　　　　　 
2.程序源代码： 
#include "math.h" 
main() 
{ 
　int m,i,k,h=0,leap=1; 
　printf("\n"); 
　for(m=101;m<=200;m++) 
　　{ k=sqrt(m+1); 
　　　for(i=2;i<=k;i++) 
　　　　　if(m%i==0) 
　　　　　　{leap=0;break;} 
　　　if(leap) {printf("%-4d",m);h++; 
　　　　　　　　if(h%10==0) 
　　　　　　　　printf("\n"); 
　　　　　　　 } 
　　　leap=1; 
　　} 
　printf("\nThe total is %d",h); 
} 
============================================================== 
【程序13】 
题目：打印出所有的“水仙花数”，所谓“水仙花数”是指一个三位数，其各位数字立方和等于该数 
　　　本身。例如：153是一个“水仙花数”，因为153=1的三次方＋5的三次方＋3的三次方。 
1.程序分析：利用for循环控制100-999个数，每个数分解出个位，十位，百位。 
2.程序源代码： 
main() 
{ 
int i,j,k,n; 
printf("'water flower'number is:"); 
　for(n=100;n<1000;n++) 
　{ 
　　i=n/100;/*分解出百位*/ 
　　j=n/10%10;/*分解出十位*/ 
　　k=n%10;/*分解出个位*/ 
　　if(i*100+j*10+k==i*i*i+j*j*j+k*k*k) 
　　　{ 
　　　printf("%-5d",n); 
　　　} 
　} 
printf("\n"); 
} 
============================================================== 
【程序14】 
题目：将一个正整数分解质因数。例如：输入90,打印出90=2*3*3*5。 

程序分析：对n进行分解质因数，应先找到一个最小的质数k，然后按下述步骤完成：  
(1)如果这个质数恰等于n，则说明分解质因数的过程已经结束，打印出即可。 
(2)如果n<>k，但n能被k整除，则应打印出k的值，并用n除以k的商,作为新的正整数你n, 
　重复执行第一步。 
(3)如果n不能被k整除，则用k+1作为k的值,重复执行第一步。 

2.程序源代码： 
/* zheng int is divided yinshu*/ 
main() 
{ 
int n,i; 
printf("\nplease input a number:\n"); 
scanf("%d",&n); 
printf("%d=",n); 
for(i=2;i<=n;i++) 
　{ 
　　while(n!=i) 
　　{ 
　　　if(n%i==0) 
　　　{ printf("%d*",i); 
　　　　n=n/i; 
　　　} 
　　　else 
　　　　break; 
　　} 
} 
printf("%d",n);} 
============================================================== 
【程序15】 
题目：利用条件运算符的嵌套来完成此题：学习成绩>=90分的同学用A表示，60-89分之间的用B表示， 
　　　60分以下的用C表示。 
1.程序分析：(a>b)?a:b这是条件运算符的基本例子。 
2.程序源代码： 
main() 
{ 
　int score; 
　char grade; 
　printf("please input a score\n"); 
　scanf("%d",&score); 
　grade=score>=90?'A':(score>=60?'B':'C'); 
　printf("%d belongs to %c",score,grade); 
} 
============================================================== 
【程序16】 
题目：输入两个正整数m和n，求其最大公约数和最小公倍数。 
 
 
  
 作者： zhlei81   2005-1-22 11:30 　 回复此发言    
 
--------------------------------------------------------------------------------
 
4 回复：经典Ｃ源程序100例  
 1.程序分析：利用辗除法。 

2.程序源代码： 
main() 
{ 
　int a,b,num1,num2,temp; 
　printf("please input two numbers:\n"); 
　scanf("%d,%d",&num1,&num2); 
　if(num1 　{ temp=num1; 
　　num1=num2;　 
　　num2=temp; 
　} 
a=num1;b=num2; 
while(b!=0)/*利用辗除法，直到b为0为止*/ 
　{ 
　　temp=a%b; 
　　a=b; 
　　b=temp; 
　} 
printf("gongyueshu:%d\n",a); 
printf("gongbeishu:%d\n",num1*num2/a); 
} 
============================================================== 
【程序17】 
题目：输入一行字符，分别统计出其中英文字母、空格、数字和其它字符的个数。 
1.程序分析：利用while语句,条件为输入的字符不为'\n'. 
　　　　　　 
2.程序源代码： 
#include "stdio.h" 
main() 
{char c; 
　int letters=0,space=0,digit=0,others=0; 
　printf("please input some characters\n"); 
　while((c=getchar())!='\n') 
　{ 
　if(c>='a'&&c<='z'||c>='A'&&c<='Z') 
　　letters++; 
　else if(c==' ') 
　　space++; 
　　　else if(c>='0'&&c<='9') 
　　　　　　　digit++; 
　　　　　else 
　　　　　　　others++; 
} 
printf("all in all:char=%d space=%d digit=%d others=%d\n",letters, 
space,digit,others); 
} 
============================================================== 
【程序18】 
题目：求s=a+aa+aaa+aaaa+aa...a的值，其中a是一个数字。例如2+22+222+2222+22222(此时 
　　　共有5个数相加)，几个数相加有键盘控制。 
1.程序分析：关键是计算出每一项的值。 
2.程序源代码： 
main() 
{ 
　int a,n,count=1; 
　long int sn=0,tn=0; 
　printf("please input a and n\n"); 
　scanf("%d,%d",&a,&n); 
　printf("a=%d,n=%d\n",a,n); 
　while(count<=n) 
　{ 
　　tn=tn+a; 
　　sn=sn+tn; 
　　a=a*10; 
　　++count; 
　} 
printf("a+aa+...=%ld\n",sn); 
} 
============================================================== 
【程序19】 
题目：一个数如果恰好等于它的因子之和，这个数就称为“完数”。例如6=1＋2＋3.编程 
　　　找出1000以内的所有完数。 
1. 程序分析：请参照程序<--上页程序14.  
2.程序源代码： 
main() 
{ 
static int k[10]; 
int i,j,n,s; 
for(j=2;j<1000;j++) 
　{ 
　n=-1; 
　s=j; 
　　for(i=1;i 　　{ 
　　　if((j%i)==0) 
　　　{　n++; 
　　　　s=s-i; 
　　　　k[n]=i; 
　　　} 
　　} 
　if(s==0) 
　{ 
　printf("%d is a wanshu",j); 
　for(i=0;i 　printf("%d,",k[i]); 
　printf("%d\n",k[n]); 
　} 
} 
} 
==============================================================  
【程序20】 
题目：一球从100米高度自由落下，每次落地后反跳回原高度的一半；再落下，求它在 
　　　第10次落地时，共经过多少米？第10次反弹多高？ 
1.程序分析：见下面注释 
2.程序源代码： 
main() 
{ 
float sn=100.0,hn=sn/2; 
int n; 
for(n=2;n<=10;n++) 
　{ 
　　sn=sn+2*hn;/*第n次落地时共经过的米数*/ 
　　hn=hn/2; /*第n次反跳高度*/ 
　} 
printf("the total of road is %f\n",sn); 
printf("the tenth is %f meter\n",hn); 
}  
 
  
 作者： zhlei81   2005-1-22 11:30 　 回复此发言    
 
--------------------------------------------------------------------------------
 
5 回复：经典Ｃ源程序100例  
 【程序21】 
题目：猴子吃桃问题：猴子第一天摘下若干个桃子，当即吃了一半，还不瘾，又多吃了一个 
　　　第二天早上又将剩下的桃子吃掉一半，又多吃了一个。以后每天早上都吃了前一天剩下 
　　　的一半零一个。到第10天早上想再吃时，见只剩下一个桃子了。求第一天共摘了多少。 
1.程序分析：采取逆向思维的方法，从后往前推断。 
2.程序源代码： 
main() 
{ 
int day,x1,x2; 
day=9; 
x2=1; 
while(day>0) 
　{x1=(x2+1)*2;/*第一天的桃子数是第2天桃子数加1后的2倍*/ 
　x2=x1; 
　day--; 
　} 
printf("the total is %d\n",x1); 
} 
============================================================== 
【程序22】 
题目：两个乒乓球队进行比赛，各出三人。甲队为a,b,c三人，乙队为x,y,z三人。已抽签决定 
　　　比赛名单。有人向队员打听比赛的名单。a说他不和x比，c说他不和x,z比，请编程序找出 
　　　三队赛手的名单。  
1.程序分析：判断素数的方法：用一个数分别去除2到sqrt(这个数)，如果能被整除， 
　　　　　　则表明此数不是素数，反之是素数。 　　　　　　 
2.程序源代码： 
main() 
{ 
char i,j,k;/*i是a的对手，j是b的对手，k是c的对手*/ 
for(i='x';i<='z';i++) 
　for(j='x';j<='z';j++) 
　{ 
　if(i!=j) 
　　for(k='x';k<='z';k++) 
　　{ if(i!=k&&j!=k) 
　　　{ if(i!='x'&&k!='x'&&k!='z') 
　　　printf("order is a--%c\tb--%c\tc--%c\n",i,j,k); 
　　　} 
　　} 
　} 
} 
============================================================== 
【程序23】  
题目：打印出如下图案（菱形） 

* 
*** 
****** 
******** 
****** 
*** 
* 
1.程序分析：先把图形分成两部分来看待，前四行一个规律，后三行一个规律，利用双重 
　　　　　　for循环，第一层控制行，第二层控制列。  
2.程序源代码： 
main() 
{ 
int i,j,k; 
for(i=0;i<=3;i++) 
　{ 
　for(j=0;j<=2-i;j++) 
　　printf(" "); 
　for(k=0;k<=2*i;k++) 
　　printf("*"); 
　printf("\n"); 
　} 
for(i=0;i<=2;i++) 
　{ 
　for(j=0;j<=i;j++) 
　　printf(" "); 
　for(k=0;k<=4-2*i;k++) 
　　printf("*"); 
　printf("\n"); 
　} 
} 
============================================================== 
【程序24】  
题目：有一分数序列：2/1，3/2，5/3，8/5，13/8，21/13...求出这个数列的前20项之和。 
1.程序分析：请抓住分子与分母的变化规律。  
2.程序源代码： 
main() 
{ 
int n,t,number=20; 
float a=2,b=1,s=0; 
for(n=1;n<=number;n++) 
　{ 
　s=s+a/b; 
　t=a;a=a+b;b=t;/*这部分是程序的关键，请读者猜猜t的作用*/ 
　} 
printf("sum is %9.6f\n",s); 
} 
============================================================== 
【程序25】  
题目：求1+2!+3!+...+20!的和 
1.程序分析：此程序只是把累加变成了累乘。  
2.程序源代码： 
main() 
{ 
float n,s=0,t=1; 
for(n=1;n<=20;n++) 
　{ 
　t*=n; 
　s+=t; 
　} 
printf("1+2!+3!...+20!=%e\n",s); 
} 
============================================================== 
【程序26】  
题目：利用递归方法求5!。 
1.程序分析：递归公式：fn=fn_1*4! 
2.程序源代码： 
#include "stdio.h" 
main() 
{ 
int i; 
int fact(); 
for(i=0;i<5;i++) 
　printf("\40:%d!=%d\n",i,fact(i)); 
} 
int fact(j) 
int j; 
{ 
int sum; 
if(j==0) 
　sum=1; 
else 
　sum=j*fact(j-1); 
return sum; 
} 
============================================================== 
【程序27】  
题目：利用递归函数调用方式，将所输入的5个字符，以相反顺序打印出来。 
1.程序分析： 
2.程序源代码： 
#include "stdio.h" 
main() 
{ 
int i=5; 
void palin(int n); 
 
 
  
 作者： zhlei81   2005-1-22 11:30 　 回复此发言    
 
--------------------------------------------------------------------------------
 
6 回复：经典Ｃ源程序100例  
 printf("\40:"); 
palin(i); 
printf("\n"); 
} 
void palin(n) 
int n; 
{ 
char next; 
if(n<=1) 
　{ 
　next=getchar(); 
　printf("\n\0:"); 
　putchar(next); 
　} 
else 
　{ 
　next=getchar(); 
　palin(n-1); 
　putchar(next); 
　} 
} 
============================================================== 
【程序28】  
题目：有5个人坐在一起，问第五个人多少岁？他说比第4个人大2岁。问第4个人岁数，他说比第 
　　　3个人大2岁。问第三个人，又说比第2人大两岁。问第2个人，说比第一个人大两岁。最后  
　　　问第一个人，他说是10岁。请问第五个人多大？ 
1.程序分析：利用递归的方法，递归分为回推和递推两个阶段。要想知道第五个人岁数，需知道 
　　　　　　第四人的岁数，依次类推，推到第一人（10岁），再往回推。 
2.程序源代码： 
age(n) 
int n; 
{ 
int c; 
if(n==1) c=10; 
else c=age(n-1)+2; 
return©; 
} 
main() 
{ printf("%d",age(5)); 
} 
============================================================== 
【程序29】  
题目：给一个不多于5位的正整数，要求：一、求它是几位数，二、逆序打印出各位数字。 
1. 程序分析：学会分解出每一位数，如下解释：(这里是一种简单的算法，师专数002班赵鑫提供)  
2.程序源代码： 
main( ) 
{ 
long a,b,c,d,e,x; 
scanf("%ld",&x); 
a=x/10000;/*分解出万位*/ 
b=x%10000/1000;/*分解出千位*/ 
c=x%1000/100;/*分解出百位*/ 
d=x%100/10;/*分解出十位*/ 
e=x%10;/*分解出个位*/ 
if (a!=0) printf("there are 5, %ld %ld %ld %ld %ld\n",e,d,c,b,a); 
else if (b!=0) printf("there are 4, %ld %ld %ld %ld\n",e,d,c,b); 
　　else if (c!=0) printf(" there are 3,%ld %ld %ld\n",e,d,c); 
　　　　else if (d!=0) printf("there are 2, %ld %ld\n",e,d); 
　　　　　　else if (e!=0) printf(" there are 1,%ld\n",e); 
} 
============================================================== 
【程序30】  
题目：一个5位数，判断它是不是回文数。即12321是回文数，个位与万位相同，十位与千位相同。　　　 
1.程序分析：同29例 
2.程序源代码： 
main( ) 
{ 
long ge,shi,qian,wan,x; 
scanf("%ld",&x); 
wan=x/10000; 
qian=x%10000/1000; 
shi=x%100/10; 
ge=x%10; 
if (ge==wan&&shi==qian)/*个位等于万位并且十位等于千位*/ 
　printf("this number is a huiwen\n"); 
else 
　printf("this number is not a huiwen\n"); 
}  
 
  
 作者： zhlei81   2005-1-22 11:30 　 回复此发言    
 
--------------------------------------------------------------------------------
 
7 回复：经典Ｃ源程序100例  
 程序31】 
题目：请输入星期几的第一个字母来判断一下是星期几，如果第一个字母一样，则继续 
　　　判断第二个字母。 
1.程序分析：用情况语句比较好，如果第一个字母一样，则判断用情况语句或if语句判断第二个字母。 
2.程序源代码： 
#include <stdio.h> 
void main() 
{ 
char letter; 
printf("please input the first letter of someday\n"); 
while ((letter=getch())!='Y')/*当所按字母为Y时才结束*/ 
{ switch (letter) 
{case 'S':printf("please input second letter\n"); 
　　　　　if((letter=getch())=='a') 
　　　　　　printf("saturday\n"); 
　　　　　else if ((letter=getch())=='u') 
　　　　　　　　　printf("sunday\n"); 
　　　　　　　else printf("data error\n"); 
　　　　　break; 
case 'F':printf("friday\n");break; 
case 'M':printf("monday\n");break; 
case 'T':printf("please input second letter\n"); 
　　　　　if((letter=getch())=='u') 
　　　　　　printf("tuesday\n"); 
　　　　　else if ((letter=getch())=='h') 
　　　　　　　　　printf("thursday\n"); 
　　　　　　　else printf("data error\n"); 
　　　　　break; 
case 'W':printf("wednesday\n");break; 
default: printf("data error\n"); 
　　} 
　} 
} 
============================================================== 
【程序32】 
题目：Press any key to change color, do you want to try it. Please hurry up! 
1.程序分析：　　　　　　　　　　　　 
2.程序源代码： 
#include <conio.h> 
void main(void) 
{ 
int color; 
for (color = 0; color < 8; color++) 
　{  
　textbackground(color);/*设置文本的背景颜色*/ 
　cprintf("This is color %d\r\n", color); 
　cprintf("Press any key to continue\r\n"); 
　getch();/*输入字符看不见*/ 
　} 
} 
============================================================== 
## gotoxy 与 clrscr 函数调用 

学习gotoxy()与clrscr()函数　　　 

```c
#include <conio.h> 
void main(void) 
{ 
clrscr();/*清屏函数*/ 
textbackground(2); 
gotoxy(1, 5);/*定位函数*/ 
cprintf("Output at row 5 column 1\n"); 
textbackground(3); 
gotoxy(20, 10); 
cprintf("Output at row 10 column 20\n"); 
}
```

【程序34】 
题目：练习函数调用 
1. 程序分析：  
2.程序源代码： 
#include <stdio.h> 
void hello_world(void) 
{ 
printf("Hello, world!\n"); 
} 
void three_hellos(void) 
{ 
int counter; 
for (counter = 1; counter <= 3; counter++) 
hello_world();/*调用此函数*/ 
} 
void main(void) 
{ 
three_hellos();/*调用此函数*/ 
} 
============================================================== 
【程序35】 
题目：文本颜色设置 
1.程序分析： 
2.程序源代码： 
#include <conio.h> 
void main(void) 
{ 
int color; 
for (color = 1; color < 16; color++) 
　{ 
　textcolor(color);/*设置文本颜色*/ 
　cprintf("This is color %d\r\n", color); 
　} 
textcolor(128 + 15); 
cprintf("This is blinking\r\n"); 
} 
============================================================== 
【程序36】 
题目：求100之内的素数　　　 
1.程序分析： 
2.程序源代码： 
#include <stdio.h> 
#include "math.h" 
#define N 101 
main() 
{ 
int i,j,line,a[N]; 
for(i=2;i<N;i++) a[i]=i; 
for(i=2;i<sqrt(N);i++) 
　for(j=i+1;j<N;j++) 
　{ 
　　if(a[i]!=0&&a[j]!=0) 
　　if(a[j]%a[i]==0) 
　　a[j]=0;} 
printf("\n"); 
for(i=2,line=0;i<N;i++) 
 
 
  
 作者： zhlei81   2005-1-22 11:30 　 回复此发言    
 
--------------------------------------------------------------------------------
 
8 回复：经典Ｃ源程序100例  
 { 
　if(a[i]!=0) 
　{printf("%5d",a[i]); 
　line++;} 
　if(line==10) 
　{printf("\n"); 
line=0;} 
} 
} 
============================================================== 
【程序37】 
题目：对10个数进行排序 
1.程序分析：可以利用选择法，即从后9个比较过程中，选择一个最小的与第一个元素交换， 
　　　　　　下次类推，即用第二个元素与后8个进行比较，并进行交换。 　　　　　　  
2.程序源代码： 
#define N 10 
main() 
{int i,j,min,tem,a[N]; 
/*input data*/ 
printf("please input ten num:\n"); 
for(i=0;i<N;i++) 
{ 
printf("a[%d]=",i); 
scanf("%d",&a[i]);} 
printf("\n"); 
for(i=0;i<N;i++) 
printf("%5d",a[i]); 
printf("\n"); 
/*sort ten num*/ 
for(i=0;i<N-1;i++) 
{min=i; 
for(j=i+1;j<N;j++) 
if(a[min]>a[j]) min=j; 
tem=a[i]; 
a[i]=a[min]; 
a[min]=tem; 
} 
/*output data*/ 
printf("After sorted \n"); 
for(i=0;i<N;i++) 
printf("%5d",a[i]); 
} 
============================================================== 
【程序38】 
题目：求一个3*3矩阵对角线元素之和  
1.程序分析：利用双重for循环控制输入二维数组，再将a[i][i]累加后输出。 
2.程序源代码： 
main() 
{ 
float a[3][3],sum=0; 
int i,j; 
printf("please input rectangle element:\n"); 
for(i=0;i<3;i++) 
　for(j=0;j<3;j++) 
　scanf("%f",&a[i][j]); 
for(i=0;i<3;i++) 
　sum=sum+a[i][i]; 
printf("duijiaoxian he is %6.2f",sum); 
} 
============================================================== 
【程序39】 
题目：有一个已经排好序的数组。现输入一个数，要求按原来的规律将它插入数组中。 
1. 程序分析：首先判断此数是否大于最后一个数，然后再考虑插入中间的数的情况，插入后 
　　　　　此元素之后的数，依次后移一个位置。  
2.程序源代码： 
main() 
{ 
int a[11]={1,4,6,9,13,16,19,28,40,100}; 
int temp1,temp2,number,end,i,j; 
printf("original array is:\n"); 
for(i=0;i<10;i++) 
　printf("%5d",a[i]); 
printf("\n"); 
printf("insert a new number:"); 
scanf("%d",&number); 
end=a[9]; 
if(number>end) 
　a[10]=number; 
else 
　{for(i=0;i<10;i++) 
　　{ if(a[i]>number) 
　　　{temp1=a[i]; 
　　　　a[i]=number; 
　　　for(j=i+1;j<11;j++) 
　　　{temp2=a[j]; 
　　　　a[j]=temp1; 
　　　　temp1=temp2; 
　　　} 
　　　break; 
　　　} 
　　} 
} 
for(i=0;i<11;i++) 
　printf("%6d",a[i]); 
} 
============================================================== 
【程序40】 
题目：将一个数组逆序输出。 
1.程序分析：用第一个与最后一个交换。 
2.程序源代码： 
#define N 5 
main() 
{ int a[N]={9,6,5,4,1},i,temp; 
　printf("\n original array:\n"); 
　for(i=0;i<N;i++) 
　printf("%4d",a[i]); 
　for(i=0;i<N/2;i++) 
　{temp=a[i]; 
　　a[i]=a[N-i-1]; 
　　a[N-i-1]=temp; 
　} 
printf("\n sorted array:\n"); 
for(i=0;i<N;i++) 
　printf("%4d",a[i]); 
}  
 
  
 作者： zhlei81   2005-1-22 11:30 　 回复此发言    
 
--------------------------------------------------------------------------------
 
## 使用按位与 

0&0=0; 0&1=0; 1&0=0; 1&1=1 

```
#include <stdio.h>
int main() 
{ 
    int a,b; 
    a=077; 
    b=a&3; 
    printf("\40: The a & b(decimal) is %d \n",b); 
    b&=7; 
    printf("\40: The a & b(decimal) is %d \n",b);

    return 0;
}
```
 
## 使用按位或

按位或的逻辑：

0 | 0 = 0;
0 | 1 = 1;
1 | 0 = 1;
1 | 1 = 1;　　　　　　　　　　 

```
#include <stdio.h>
int main() 
{ 
    int a;
    int b;

    a = 077; 

    b = a|3; 
    printf("\n: The a & b(decimal) is %d \n",b);

    b|=7; 
    printf("\n: The a & b(decimal) is %d \n",b);

    return 0;
}
```

============================================================== 
【程序53】 
题目：学习使用按位异或 ^ 。　　　 
1.程序分析：0^0=0; 0^1=1; 1^0=1; 1^1=0 
2.程序源代码： 
#include "stdio.h" 
main() 
{ 
int a,b; 
a=077; 
b=a^3; 
printf("\40: The a & b(decimal) is %d \n",b); 
b^=7; 
printf("\40: The a & b(decimal) is %d \n",b); 
} 
============================================================== 
【程序54】 
题目：取一个整数a从右端开始的4～7位。 
程序分析：可以这样考虑：  
(1)先使a右移4位。 
(2)设置一个低4位全为1,其余全为0的数。可用~(~0<<4) 
(3)将上面二者进行&运算。 
2.程序源代码： 
main() 
{ 
unsigned a,b,c,d; 
scanf("%o",&a); 
b=a>>4; 
c=~(~0<<4); 
d=b&c; 
printf("%o\n%o\n",a,d); 
} 
============================================================== 
【程序55】 
题目：学习使用按位取反~。　　　 
1.程序分析：~0=1; ~1=0; 
2.程序源代码： 
#include "stdio.h" 
main() 
{ 
int a,b; 
a=234; 
b=~a; 
printf("\40: The a's 1 complement(decimal) is %d \n",b); 
a=~a; 
printf("\40: The a's 1 complement(hexidecimal) is %x \n",a); 
}  
============================================================== 
【程序56】 
题目：画图，学用circle画圆形。　　　 
1.程序分析： 
2.程序源代码： 
/*circle*/ 
#include "graphics.h" 
main() 
{int driver,mode,i; 
float j=1,k=1; 
driver=VGA;mode=VGAHI; 
initgraph(&driver,&mode,""); 
setbkcolor(YELLOW); 
for(i=0;i<=25;i++) 
{ 
setcolor(8); 
circle(310,250,k); 
k=k+j; 
j=j+0.3; 
} 
}  
============================================================== 
【程序57】 
题目：画图，学用line画直线。 
1.程序分析：　　　　　　　　　　　 
2.程序源代码： 
#include "graphics.h" 
main() 
{int driver,mode,i; 
float x0,y0,y1,x1; 
float j=12,k; 
driver=VGA;mode=VGAHI; 
initgraph(&driver,&mode,""); 
setbkcolor(GREEN); 
x0=263;y0=263;y1=275;x1=275; 
for(i=0;i<=18;i++) 
{ 
setcolor(5); 
line(x0,y0,x0,y1); 
x0=x0-5; 
y0=y0-5; 
x1=x1+5; 
y1=y1+5; 
j=j+10; 
} 
x0=263;y1=275;y0=263; 
for(i=0;i<=20;i++) 
{ 
setcolor(5); 
line(x0,y0,x0,y1); 
x0=x0+5; 
y0=y0+5; 
y1=y1-5; 
} 
} 
============================================================== 
【程序58】 
题目：画图，学用rectangle画方形。　　　 
1.程序分析：利用for循环控制100-999个数，每个数分解出个位，十位，百位。 
2.程序源代码： 
#include "graphics.h" 
main() 
{int x0,y0,y1,x1,driver,mode,i; 
driver=VGA;mode=VGAHI; 
initgraph(&driver,&mode,""); 
setbkcolor(YELLOW); 
x0=263;y0=263;y1=275;x1=275; 
for(i=0;i<=18;i++) 
{ 
setcolor(1); 
rectangle(x0,y0,x1,y1); 
x0=x0-5; 
y0=y0-5; 
x1=x1+5; 
y1=y1+5; 
} 
settextstyle(DEFAULT_FONT,HORIZ_DIR,2); 
 
 
  
 作者： zhlei81   2005-1-22 11:31 　 回复此发言    
 
--------------------------------------------------------------------------------
 
11 回复：经典Ｃ源程序100例  
 outtextxy(150,40,"How beautiful it is!"); 
line(130,60,480,60); 
setcolor(2); 
circle(269,269,137); 
} 
============================================================== 
【程序59】 
题目：画图，综合例子。 
1.程序分析： 
2.程序源代码： 
# define PAI 3.1415926 
# define B 0.809 
# include "graphics.h" 
#include "math.h" 
main() 
{ 
int i,j,k,x0,y0,x,y,driver,mode; 
float a; 
driver=CGA;mode=CGAC0; 
initgraph(&driver,&mode,""); 
setcolor(3); 
setbkcolor(GREEN); 
x0=150;y0=100; 
circle(x0,y0,10); 
circle(x0,y0,20); 
circle(x0,y0,50); 
for(i=0;i<16;i++) 
{ 
　a=(2*PAI/16)*i; 
　x=ceil(x0+48*cos(a)); 
　y=ceil(y0+48*sin(a)*B); 
　setcolor(2); line(x0,y0,x,y);} 
setcolor(3);circle(x0,y0,60); 
/* Make 0 time normal size letters */ 
settextstyle(DEFAULT_FONT,HORIZ_DIR,0); 
outtextxy(10,170,"press a key"); 
getch(); 
setfillstyle(HATCH_FILL,YELLOW); 
floodfill(202,100,WHITE); 
getch(); 
for(k=0;k<=500;k++) 
{ 
　setcolor(3); 
　for(i=0;i<=16;i++) 
　{ 
　　a=(2*PAI/16)*i+(2*PAI/180)*k; 
　　x=ceil(x0+48*cos(a)); 
　　y=ceil(y0+48+sin(a)*B); 
　　setcolor(2); line(x0,y0,x,y); 
　} 
　for(j=1;j<=50;j++) 
　{ 
　　a=(2*PAI/16)*i+(2*PAI/180)*k-1; 
　　x=ceil(x0+48*cos(a)); 
　　y=ceil(y0+48*sin(a)*B); 
　　line(x0,y0,x,y); 
　} 
} 
restorecrtmode(); 
} 
============================================================== 
【程序60】 
题目：画图，综合例子。　　　 
1.程序分析： 
2.程序源代码： 
#include "graphics.h" 
#define LEFT 0 
#define TOP 0 
#define RIGHT 639 
#define BOTTOM 479 
#define LINES 400 
#define MAXCOLOR 15 
main() 
{ 
int driver,mode,error; 
int x1,y1; 
int x2,y2; 
int dx1,dy1,dx2,dy2,i=1; 
int count=0; 
int color=0; 
driver=VGA; 
mode=VGAHI; 
initgraph(&driver,&mode,""); 
x1=x2=y1=y2=10; 
dx1=dy1=2; 
dx2=dy2=3; 
while(!kbhit()) 
{ 
　line(x1,y1,x2,y2); 
　x1+=dx1;y1+=dy1; 
　x2+=dx2;y2+dy2; 
　if(x1<=LEFT||x1>=RIGHT) 
　dx1=-dx1; 
　if(y1<=TOP||y1>=BOTTOM) 
　　dy1=-dy1; 
　if(x2<=LEFT||x2>=RIGHT) 
　　dx2=-dx2; 
　if(y2<=TOP||y2>=BOTTOM) 
　　dy2=-dy2; 
　if(++count>LINES) 
　{ 
　　setcolor(color); 
　　color=(color>=MAXCOLOR)?0:++color; 
　} 
} 
closegraph(); 
}  
 
  
 作者： zhlei81   2005-1-22 11:31 　 回复此发言    
 
--------------------------------------------------------------------------------
 
12 回复：经典Ｃ源程序100例  
 【程序61】 
题目：打印出杨辉三角形（要求打印出10行如下图）　　　 
1.程序分析： 
　　　 　　 1 
　　　　　　1 　1 
　　　　　　1 　2 　1 
　　　　　　1　 3 　3　 1 
　　　　　　1　 4　 6 　4 　1 
　　　　　　1　 5　 10　10　5 　1　  
2.程序源代码： 
main() 
{int i,j; 
int a[10][10]; 
printf("\n"); 
for(i=0;i<10;i++) 
　{a[i][0]=1; 
　a[i][i]=1;} 
for(i=2;i<10;i++) 
　for(j=1;j<i;j++) 
　a[i][j]=a[i-1][j-1]+a[i-1][j]; 
for(i=0;i<10;i++) 
　{for(j=0;j<=i;j++) 
　printf("%5d",a[i][j]); 
　printf("\n"); 
　} 
} 
============================================================== 
【程序62】 
题目：学习putpixel画点。 
1.程序分析：　　　　　　　　　　　　 
2.程序源代码： 
#include "stdio.h" 
#include "graphics.h" 
main() 
{ 
int i,j,driver=VGA,mode=VGAHI; 
initgraph(&driver,&mode,""); 
setbkcolor(YELLOW); 
for(i=50;i<=230;i+=20) 
　for(j=50;j<=230;j++) 
　putpixel(i,j,1); 
for(j=50;j<=230;j+=20) 
　for(i=50;i<=230;i++) 
　putpixel(i,j,1); 
} 
============================================================== 
【程序63】 
题目：画椭圆ellipse　　　 
1.程序分析： 
2.程序源代码： 
#include "stdio.h" 
#include "graphics.h" 
#include "conio.h" 
main() 
{ 
int x=360,y=160,driver=VGA,mode=VGAHI; 
int num=20,i; 
int top,bottom; 
initgraph(&driver,&mode,""); 
top=y-30; 
bottom=y-30; 
for(i=0;i<num;i++) 
{ 
ellipse(250,250,0,360,top,bottom); 
top-=5; 
bottom+=5; 
} 
getch(); 
} 
============================================================== 
【程序64】 
题目：利用ellipse and rectangle 画图。 
1.程序分析： 
2.程序源代码： 
#include "stdio.h" 
#include "graphics.h" 
#include "conio.h" 
main() 
{ 
int driver=VGA,mode=VGAHI; 
int i,num=15,top=50; 
int left=20,right=50; 
initgraph(&driver,&mode,""); 
for(i=0;i<num;i++) 
{ 
ellipse(250,250,0,360,right,left); 
ellipse(250,250,0,360,20,top); 
rectangle(20-2*i,20-2*i,10*(i+2),10*(i+2)); 
right+=5; 
left+=5; 
top+=10; 
} 
getch(); 
} 
============================================================== 
【程序65】 
题目：一个最优美的图案。　　　 
1.程序分析： 
2.程序源代码： 
#include "graphics.h" 
#include "math.h" 
#include "dos.h" 
#include "conio.h" 
#include "stdlib.h" 
#include "stdio.h" 
#include "stdarg.h" 
#define MAXPTS 15 
#define PI 3.1415926 
struct PTS { 
int x,y; 
}; 
double AspectRatio=0.85; 
void LineToDemo(void) 
{ 
struct viewporttype vp; 
struct PTS points[MAXPTS]; 
int i, j, h, w, xcenter, ycenter; 
int radius, angle, step; 
double rads; 
printf(" MoveTo / LineTo Demonstration" ); 
getviewsettings( &vp ); 
h = vp.bottom - vp.top; 
w = vp.right - vp.left; 
xcenter = w / 2; /* Determine the center of circle */ 
ycenter = h / 2; 
radius = (h - 30) / (AspectRatio * 2); 
step = 360 / MAXPTS; /* Determine # of increments */ 
angle = 0; /* Begin at zero degrees */ 
for( i=0 ; i<MAXPTS ; ++i ){ /* Determine circle intercepts */ 
rads = (double)angle * PI / 180.0; /* Convert angle to radians */ 
points[i].x = xcenter + (int)( cos(rads) * radius ); 
points[i].y = ycenter - (int)( sin(rads) * radius * AspectRatio ); 
angle += step; /* Move to next increment */ 
} 
circle( xcenter, ycenter, radius ); /* Draw bounding circle */ 
 
 
  
 作者： zhlei81   2005-1-22 11:32 　 回复此发言    
 
--------------------------------------------------------------------------------
 
13 回复：经典Ｃ源程序100例  
 for( i=0 ; i<MAXPTS ; ++i ){ /* Draw the cords to the circle */ 
for( j=i ; j<MAXPTS ; ++j ){ /* For each remaining intersect */ 
moveto(points[i].x, points[i].y); /* Move to beginning of cord */ 
lineto(points[j].x, points[j].y); /* Draw the cord */ 
} } } 
main() 
{int driver,mode; 
driver=CGA;mode=CGAC0; 
initgraph(&driver,&mode,""); 
setcolor(3); 
setbkcolor(GREEN); 
LineToDemo();} 
============================================================== 
【程序66】 
题目：输入3个数a,b,c，按大小顺序输出。　　　 
1.程序分析：利用指针方法。 
2.程序源代码： 
/*pointer*/ 
main() 
{ 
int n1,n2,n3; 
int *pointer1,*pointer2,*pointer3; 
printf("please input 3 number:n1,n2,n3:"); 
scanf("%d,%d,%d",&n1,&n2,&n3); 
pointer1=&n1; 
pointer2=&n2; 
pointer3=&n3; 
if(n1>n2) swap(pointer1,pointer2); 
if(n1>n3) swap(pointer1,pointer3); 
if(n2>n3) swap(pointer2,pointer3); 
printf("the sorted numbers are:%d,%d,%d\n",n1,n2,n3); 
} 
swap(p1,p2) 
int *p1,*p2; 
{int p; 
p=*p1;*p1=*p2;*p2=p; 
} 
============================================================== 
【程序67】 
题目：输入数组，最大的与第一个元素交换，最小的与最后一个元素交换，输出数组。 
1.程序分析：谭浩强的书中答案有问题。　　　　　　 
2.程序源代码： 
main() 
{ 
int number[10]; 
input(number); 
max_min(number); 
output(number); 
} 
input(number) 
int number[10]; 
{int i; 
for(i=0;i<9;i++) 
　scanf("%d,",&number[i]); 
　scanf("%d",&number[9]); 
} 
max_min(array) 
int array[10]; 
{int *max,*min,k,l; 
int *p,*arr_end; 
arr_end=array+10; 
max=min=array; 
for(p=array+1;p<arr_end;p++) 
　if(*p>*max) max=p; 
　else if(*p<*min) min=p; 
　k=*max; 
　l=*min; 
　*p=array[0];array[0]=l;l=*p; 
　*p=array[9];array[9]=k;k=*p; 
　return; 
} 
output(array) 
int array[10]; 
{ int *p; 
for(p=array;p<array+9;p++) 
　printf("%d,",*p); 
printf("%d\n",array[9]); 
} 
============================================================== 
【程序68】 
题目：有n个整数，使其前面各数顺序向后移m个位置，最后m个数变成最前面的m个数 
1.程序分析： 
2.程序源代码： 
main() 
{ 
int number[20],n,m,i; 
printf("the total numbers is:"); 
scanf("%d",&n); 
printf("back m:"); 
scanf("%d",&m); 
for(i=0;i<n-1;i++) 
　scanf("%d,",&number[i]); 
scanf("%d",&number[n-1]); 
move(number,n,m); 
for(i=0;i<n-1;i++) 
　printf("%d,",number[i]); 
printf("%d",number[n-1]); 
} 
move(array,n,m) 
int n,m,array[20]; 
{ 
int *p,array_end; 
array_end=*(array+n-1); 
for(p=array+n-1;p>array;p--) 
　*p=*(p-1); 
　*array=array_end; 
　m--; 
　if(m>0) move(array,n,m); 
} 
============================================================== 
【程序69】 
题目：有n个人围成一圈，顺序排号。从第一个人开始报数（从1到3报数），凡报到3的人退出 
　　　圈子，问最后留下的是原来第几号的那位。 
1. 程序分析： 
2.程序源代码： 
#define nmax 50 
main() 
{ 
int i,k,m,n,num[nmax],*p; 
printf("please input the total of numbers:"); 
scanf("%d",&n); 
p=num; 
for(i=0;i<n;i++) 
　*(p+i)=i+1; 
　i=0; 
　k=0; 
　m=0; 
　while(m<n-1) 
　{ 
　if(*(p+i)!=0) k++; 
　if(k==3) 
　{ *(p+i)=0; 
　k=0; 
　m++; 
　} 
i++; 
if(i==n) i=0; 
} 
while(*p==0) p++; 
printf("%d is left\n",*p); 
} 
============================================================== 
【程序70】 
题目：写一个函数，求一个字符串的长度，在main函数中输入字符串，并输出其长度。　　　 
1.程序分析： 
2.程序源代码： 
main() 
{ 
int len; 
char *str[20]; 
printf("please input a string:\n"); 
scanf("%s",str); 
len=length(str); 
printf("the string has %d characters.",len); 
} 
length(p) 
char *p; 
{ 
int n; 
n=0; 
while(*p!='\0') 
{ 
　n++; 
　p++; 
} 
return n; 
}  
 
  
 作者： zhlei81   2005-1-22 11:32 　 回复此发言    
 
--------------------------------------------------------------------------------
 
14 回复：经典Ｃ源程序100例  
 【程序71】 
题目：编写input()和output()函数输入，输出5个学生的数据记录。 
1.程序分析： 
2.程序源代码： 
#define N 5 
struct student 
{ char num[6]; 
　char name[8]; 
　int score[4]; 
} stu[N]; 
input(stu) 
struct student stu[]; 
{ int i,j; 
　for(i=0;i<N;i++) 
　{ printf("\n please input %d of %d\n",i+1,N); 
　　printf("num: "); 
　　scanf("%s",stu[i].num); 
　　printf("name: "); 
　　scanf("%s",stu[i].name); 
　　　for(j=0;j<3;j++) 
　　　{ printf("score %d.",j+1); 
　　　　scanf("%d",&stu[i].score[j]); 
　　　} 
　　printf("\n"); 
　} 
} 
print(stu) 
struct student stu[]; 
{ int i,j; 
printf("\nNo. Name Sco1 Sco2 Sco3\n"); 
for(i=0;i<N;i++) 
{ printf("%-6s%-10s",stu[i].num,stu[i].name); 
　for(j=0;j<3;j++) 
　　printf("%-8d",stu[i].score[j]); 
　printf("\n"); 
} 
} 
main() 
{ 
　input(); 
　print(); 
} 
============================================================== 
【程序72】 
题目：创建一个链表。 
1.程序分析：　　　　　　　　　　　 
2.程序源代码： 
/*creat a list*/ 
#include "stdlib.h" 
#include "stdio.h" 
struct list 
{ int data; 
struct list *next; 
}; 
typedef struct list node; 
typedef node *link; 
void main() 
{ link ptr,head; 
int num,i; 
ptr=(link)malloc(sizeof(node)); 
ptr=head; 
printf("please input 5 numbers==>\n"); 
for(i=0;i<=4;i++) 
{ 
　scanf("%d",&num); 
　ptr->data=num; 
　ptr->next=(link)malloc(sizeof(node)); 
　if(i==4) ptr->next=NULL; 
　else ptr=ptr->next; 
} 
ptr=head; 
while(ptr!=NULL) 
{ printf("The value is ==>%d\n",ptr->data); 
　ptr=ptr->next; 
} 
} 
============================================================== 
【程序73】 
题目：反向输出一个链表。　　　 
1.程序分析： 
2.程序源代码： 
/*reverse output a list*/ 
#include "stdlib.h" 
#include "stdio.h" 
struct list 
{ int data; 
　struct list *next; 
}; 
typedef struct list node; 
typedef node *link; 
void main() 
{ link ptr,head,tail;　 
　int num,i; 
　tail=(link)malloc(sizeof(node)); 
　tail->next=NULL; 
　ptr=tail; 
　printf("\nplease input 5 data==>\n"); 
　for(i=0;i<=4;i++) 
　{ 
　　scanf("%d",&num); 
　　ptr->data=num; 
　　head=(link)malloc(sizeof(node)); 
　　head->next=ptr; 
　　ptr=head; 
　} 
ptr=ptr->next; 
while(ptr!=NULL) 
{ printf("The value is ==>%d\n",ptr->data); 
　ptr=ptr->next; 
}} 
============================================================== 
【程序74】 
题目：连接两个链表。 
1.程序分析： 
2.程序源代码： 
#include "stdlib.h" 
#include "stdio.h" 
struct list 
{ int data; 
struct list *next; 
}; 
typedef struct list node; 
typedef node *link; 
link delete_node(link pointer,link tmp) 
{if (tmp==NULL) /*delete first node*/ 
　return pointer->next; 
else 
{ if(tmp->next->next==NULL)/*delete last node*/ 
　　tmp->next=NULL; 
　else /*delete the other node*/ 
　　tmp->next=tmp->next->next; 
　return pointer; 
} 
} 
void selection_sort(link pointer,int num) 
{ link tmp,btmp; 
　int i,min; 
　for(i=0;i<num;i++) 
　{ 
　tmp=pointer; 
　min=tmp->data; 
　btmp=NULL; 
　while(tmp->next) 
　{ if(min>tmp->next->data) 
　{min=tmp->next->data; 
　　btmp=tmp; 
　} 
　tmp=tmp->next; 
　} 
printf("\40: %d\n",min); 
pointer=delete_node(pointer,btmp); 
 
 
  
 作者： zhlei81   2005-1-22 11:32 　 回复此发言    
 
--------------------------------------------------------------------------------
 
15 回复：经典Ｃ源程序100例  
 } 
} 
link create_list(int array[],int num) 
{ link tmp1,tmp2,pointer; 
int i; 
pointer=(link)malloc(sizeof(node)); 
pointer->data=array[0]; 
tmp1=pointer; 
for(i=1;i<num;i++) 
{ tmp2=(link)malloc(sizeof(node)); 
　tmp2->next=NULL; 
　tmp2->data=array[i]; 
　tmp1->next=tmp2; 
　tmp1=tmp1->next; 
} 
return pointer; 
} 
link concatenate(link pointer1,link pointer2) 
{ link tmp; 
tmp=pointer1; 
while(tmp->next) 
　tmp=tmp->next; 
tmp->next=pointer2; 
return pointer1; 
} 
void main(void) 
{ int arr1[]={3,12,8,9,11}; 
　link ptr; 
　ptr=create_list(arr1,5); 
　selection_sort(ptr,5); 
} 
============================================================== 
【程序75】 
题目：放松一下，算一道简单的题目。 
1.程序分析： 
2.程序源代码： 
main() 
{ 
int i,n; 
for(i=1;i<5;i++) 
{ n=0; 
　if(i!=1) 
　n=n+1; 
　if(i==3) 
　n=n+1; 
　if(i==4) 
　n=n+1; 
　if(i!=4) 
　n=n+1; 
　if(n==3) 
　　printf("zhu hao shi de shi:%c",64+i); 
　} 
} 
============================================================== 
【程序76】 
题目：编写一个函数，输入n为偶数时，调用函数求1/2+1/4+...+1/n,当输入n为奇数时，调用函数 
　　　1/1+1/3+...+1/n(利用指针函数) 
1.程序分析： 
2.程序源代码： 
main() 
#include "stdio.h" 
main() 
{ 
float peven(),podd(),dcall(); 
float sum; 
int n; 
while (1) 
{ 
　scanf("%d",&n); 
　if(n>1) 
　　break; 
} 
if(n%2==0) 
{ 
　printf("Even="); 
　sum=dcall(peven,n); 
} 
else 
{ 
　printf("Odd="); 
　sum=dcall(podd,n); 
} 
printf("%f",sum); 
} 
float peven(int n) 
{ 
float s; 
int i; 
s=1; 
for(i=2;i<=n;i+=2) 
　s+=1/(float)i; 
return(s); 
} 
float podd(n) 
int n; 
{ 
float s; 
int i; 
s=0; 
for(i=1;i<=n;i+=2) 
　s+=1/(float)i; 
return(s); 
} 
float dcall(fp,n) 
float (*fp)(); 
int n; 
{ 
float s; 
s=(*fp)(n); 
return(s); 
} 
============================================================== 
【程序77】 
题目：填空练习（指向指针的指针） 
1.程序分析：　　　　　 
2.程序源代码： 
main() 
{ char *s[]={"man","woman","girl","boy","sister"}; 
char **q; 
int k; 
for(k=0;k<5;k++) 
{　　　　　　　;/*这里填写什么语句*/ 
　printf("%s\n",*q); 
} 
} 
============================================================== 
【程序78】 
题目：找到年龄最大的人，并输出。请找出程序中有什么问题。 
1.程序分析： 
2.程序源代码： 
#define N 4 
#include "stdio.h" 
static struct man 
{ char name[20]; 
int age; 
} person[N]={"li",18,"wang",19,"zhang",20,"sun",22}; 
main() 
{struct man *q,*p; 
int i,m=0; 
p=person; 
for (i=0;i<N;i++) 
{if(m<p->age) 
　q=p++; 
　m=q->age;} 
printf("%s,%d",(*q).name,(*q).age); 
} 
============================================================== 
【程序79】 
题目：字符串排序。 
1.程序分析： 
2.程序源代码： 
main() 
{ 
char *str1[20],*str2[20],*str3[20]; 
char swap(); 
printf("please input three strings\n"); 
scanf("%s",str1); 
scanf("%s",str2); 
scanf("%s",str3); 
if(strcmp(str1,str2)>0) swap(str1,str2); 
if(strcmp(str1,str3)>0) swap(str1,str3); 
if(strcmp(str2,str3)>0) swap(str2,str3); 
printf("after being sorted\n"); 
printf("%s\n%s\n%s\n",str1,str2,str3); 
} 
char swap(p1,p2) 
char *p1,*p2; 
{ 
char *p[20]; 
strcpy(p,p1);strcpy(p1,p2);strcpy(p2,p); 
} 
============================================================== 
【程序80】 
题目：海滩上有一堆桃子，五只猴子来分。第一只猴子把这堆桃子凭据分为五份，多了一个，这只 
　　　猴子把多的一个扔入海中，拿走了一份。第二只猴子把剩下的桃子又平均分成五份，又多了 
　　　一个，它同样把多的一个扔入海中，拿走了一份，第三、第四、第五只猴子都是这样做的， 
　　　问海滩上原来最少有多少个桃子？ 
1.程序分析： 
2.程序源代码： 
main() 
{int i,m,j,k,count; 
for(i=4;i<10000;i+=4) 
{ count=0; 
m=i; 
for(k=0;k<5;k++) 
{ 
　j=i/4*5+1; 
　i=j; 
　if(j%4==0) 
　　count++; 
　else 
　　break; 
} 
　i=m; 
　if(count==4) 
　{printf("%d\n",count); 
　　break;} 
} 
}  
 
  
 作者： zhlei81   2005-1-22 11:32 　 回复此发言    
 
--------------------------------------------------------------------------------
 
16 回复：经典Ｃ源程序100例  
 【程序81】 
题目：809*??=800*??+9*??+1 其中??代表的两位数,8*??的结果为两位数，9*??的结果为3位数。求??代表的两位数，及809*??后的结果。 
1.程序分析： 
2.程序源代码： 
output(long b,long i) 
{ printf("\n%ld/%ld=809*%ld+%ld",b,i,i,b%i); 
} 
main() 
{long int a,b,i; 
a=809; 
for(i=10;i<100;i++) 
{b=i*a+1; 
if(b>=1000&&b<=10000&&8*i<100&&9*i>=100) 
output(b,i); } 
} 
============================================================== 

## 八进制转换为十进制 

```
main() 
{ char *p,s[6];int n; 
p=s; 
gets(p); 
n=0; 
while(*(p)!='\0') 
{n=n*8+*p-'0'; 
p++;} 
printf("%d",n); 
}
```

### 求0—7所能组成的奇数个数。 

```
main() 
{ 
long sum=4,s=4; 
int j; 
for(j=2;j<=8;j++)/*j is place of number*/ 
{ printf("\n%ld",sum); 
if(j<=2) 
s*=7; 
else 
s*=8; 
sum+=s;} 
printf("\nsum=%ld",sum); 
}
```

============================================================== 
【程序84】 
题目：一个偶数总能表示为两个素数之和。 
1.程序分析： 
2.程序源代码： 
#include "stdio.h" 
#include "math.h" 
main() 
{ int a,b,c,d; 
scanf("%d",&a); 
for(b=3;b<=a/2;b+=2) 
{ for(c=2;c<=sqrt(b);c++) 
if(b%c==0) break; 
if(c>sqrt(b)) 
d=a-b; 
else 
break; 
for(c=2;c<=sqrt(d);c++) 
if(d%c==0) break; 
if(c>sqrt(d)) 
printf("%d=%d+%d\n",a,b,d); 
} 
} 
============================================================== 
【程序85】 
题目：判断一个素数能被几个9整除 
1.程序分析： 
2.程序源代码： 
main() 
{ long int m9=9,sum=9; 
int zi,n1=1,c9=1; 
scanf("%d",&zi); 
while(n1!=0) 
{ if(!(sum%zi)) 
n1=0; 
else 
{m9=m9*10; 
sum=sum+m9; 
c9++; 
} 
} 
printf("%ld,can be divided by %d \"9\"",sum,c9); 
} 
============================================================== 
【程序86】 
题目：两个字符串连接程序 
1.程序分析： 
2.程序源代码： 
#include "stdio.h" 
main() 
{char a[]="acegikm"; 
char b[]="bdfhjlnpq"; 
char c[80],*p; 
int i=0,j=0,k=0; 
while(a[i]!='\0'&&b[j]!='\0') 
{if (a[i] { c[k]=a[i];i++;} 
else 
c[k]=b[j++]; 
k++; 
} 
c[k]='\0'; 
if(a[i]=='\0') 
p=b+j; 
else 
p=a+i; 
strcat(c,p); 
puts©; 
} 
============================================================== 
【程序87】 
题目：回答结果（结构体变量传递） 
1.程序分析：　　　　　 
2.程序源代码： 
#include "stdio.h" 
struct student 
{ int x; 
char c; 
} a; 
main() 
{a.x=3; 
a.c='a'; 
f(a); 
printf("%d,%c",a.x,a.c); 
} 
f(struct student b) 
{ 
b.x=20; 
b.c='y'; 
} 
============================================================== 
【程序88】 
题目：读取7个数（1—50）的整数值，每读取一个值，程序打印出该值个数的＊。 
1.程序分析： 
2.程序源代码： 
main() 
{int i,a,n=1; 
while(n<=7) 
{ do { 
　　　scanf("%d",&a); 
　　　}while(a<1||a>50); 
for(i=1;i<=a;i++) 
　printf("*"); 
printf("\n"); 
n++;} 
getch(); 
} 
============================================================== 
【程序89】 
题目：某个公司采用公用电话传递数据，数据是四位的整数，在传递过程中是加密的，加密规则如下： 
　　　每位数字都加上5,然后用和除以10的余数代替该数字，再将第一位和第四位交换，第二位和第三位交换。 
1.程序分析： 
2.程序源代码： 
main() 
{int a,i,aa[4],t; 
scanf("%d",&a); 
aa[0]=a%10; 
aa[1]=a%100/10; 
aa[2]=a%1000/100; 
aa[3]=a/1000; 
for(i=0;i<=3;i++) 
　{aa[i]+=5; 
　aa[i]%=10; 
　} 
for(i=0;i<=3/2;i++) 
　{t=aa[i]; 
　aa[i]=aa[3-i]; 
　aa[3-i]=t; 
　} 
for(i=3;i>=0;i--) 
printf("%d",aa[i]); 
} 
============================================================== 
【程序90】 
题目：专升本一题，读结果。 
1.程序分析： 
2.程序源代码： 
#include "stdio.h" 
#define M 5 
main() 
{int a[M]={1,2,3,4,5}; 
int i,j,t; 
i=0;j=M-1; 
while(i {t=*(a+i); 
*(a+i)=*(a+j); 
*(a+j)=t; 
i++;j--; 
} 
for(i=0;i printf("%d",*(a+i)); 
}  
 
  
 作者： zhlei81   2005-1-22 11:33 　 回复此发言    
 
--------------------------------------------------------------------------------
 
17 回复：经典Ｃ源程序100例  
 【程序91】 
题目：时间函数举例1 
1.程序分析： 
2.程序源代码： 
#include "stdio.h" 
#include "time.h" 
void main() 
{ time_t lt; /*define a longint time varible*/ 
lt=time(NULL);/*system time and date*/ 
printf(ctime(<)); /*english format output*/ 
printf(asctime(localtime(<)));/*tranfer to tm*/ 
printf(asctime(gmtime(<))); /*tranfer to Greenwich time*/ 
} 
============================================================== 
【程序92】 
题目：时间函数举例2 
1.程序分析：　　　　　　　　　　　 
2.程序源代码： 
/*calculate time*/ 
#include "time.h" 
#include "stdio.h" 
main() 
{ time_t start,end; 
int i; 
start=time(NULL); 
for(i=0;i<3000;i++) 
{ printf("\1\1\1\1\1\1\1\1\1\1\n");} 
end=time(NULL); 
printf("\1: The different is %6.3f\n",difftime(end,start)); 
} 
============================================================== 
【程序93】 
题目：时间函数举例3 
1.程序分析： 
2.程序源代码： 
/*calculate time*/ 
#include "time.h" 
#include "stdio.h" 
main() 
{ clock_t start,end; 
int i; 
double var; 
start=clock(); 
for(i=0;i<10000;i++) 
{ printf("\1\1\1\1\1\1\1\1\1\1\n");} 
end=clock(); 
printf("\1: The different is %6.3f\n",(double)(end-start)); 
} 
============================================================== 
【程序94】 
题目：时间函数举例4,一个猜数游戏，判断一个人反应快慢。（版主初学时编的） 
1.程序分析： 
2.程序源代码： 
#include "time.h" 
#include "stdlib.h" 
#include "stdio.h" 
main() 
{char c; 
clock_t start,end; 
time_t a,b; 
double var; 
int i,guess; 
srand(time(NULL)); 
printf("do you want to play it.('y' or 'n') \n"); 
loop: 
while((c=getchar())=='y') 
{ 
i=rand()%100; 
printf("\nplease input number you guess:\n"); 
start=clock(); 
a=time(NULL); 
scanf("%d",&guess); 
while(guess!=i) 
{if(guess>i) 
{printf("please input a little smaller.\n"); 
scanf("%d",&guess);} 
else 
{printf("please input a little bigger.\n"); 
scanf("%d",&guess);} 
} 
end=clock(); 
b=time(NULL); 
printf("\1: It took you %6.3f seconds\n",var=(double)(end-start)/18.2); 
printf("\1: it took you %6.3f seconds\n\n",difftime(b,a)); 
if(var<15) 
printf("\1\1 You are very clever! \1\1\n\n"); 
else if(var<25) 
printf("\1\1 you are normal! \1\1\n\n"); 
else 
printf("\1\1 you are stupid! \1\1\n\n"); 
printf("\1\1 Congradulations \1\1\n\n"); 
printf("The number you guess is %d",i); 
} 
printf("\ndo you want to try it again?(\"yy\".or.\"n\")\n"); 
if((c=getch())=='y') 
goto loop; 
} 
============================================================== 
【程序95】 
题目：家庭财务管理小程序 
1.程序分析： 
2.程序源代码： 
/*money management system*/ 
#include "stdio.h" 
#include "dos.h" 
main() 
{ 
FILE *fp; 
struct date d; 
float sum,chm=0.0; 
int len,i,j=0; 
int c; 
char ch[4]="",ch1[16]="",chtime[12]="",chshop[16],chmoney[8]; 
pp: clrscr(); 
sum=0.0; 
gotoxy(1,1);printf("|---------------------------------------------------------------------------|"); 
gotoxy(1,2);printf("| money management system(C1.0) 2000.03 |"); 
gotoxy(1,3);printf("|---------------------------------------------------------------------------|"); 
gotoxy(1,4);printf("| -- money records -- | -- today cost list -- |"); 
 
 
  
 作者： zhlei81   2005-1-22 11:33 　 回复此发言    
 
--------------------------------------------------------------------------------
 
18 回复：经典Ｃ源程序100例  
 gotoxy(1,5);printf("| ------------------------ |-------------------------------------|"); 
gotoxy(1,6);printf("| date: -------------- | |"); 
gotoxy(1,7);printf("| | | | |"); 
gotoxy(1,8);printf("| -------------- | |"); 
gotoxy(1,9);printf("| thgs: ------------------ | |"); 
gotoxy(1,10);printf("| | | | |"); 
gotoxy(1,11);printf("| ------------------ | |"); 
gotoxy(1,12);printf("| cost: ---------- | |"); 
gotoxy(1,13);printf("| | | | |"); 
gotoxy(1,14);printf("| ---------- | |"); 
gotoxy(1,15);printf("| | |"); 
gotoxy(1,16);printf("| | |"); 
gotoxy(1,17);printf("| | |"); 
gotoxy(1,18);printf("| | |"); 
gotoxy(1,19);printf("| | |"); 
gotoxy(1,20);printf("| | |"); 
gotoxy(1,21);printf("| | |"); 
gotoxy(1,22);printf("| | |"); 
gotoxy(1,23);printf("|---------------------------------------------------------------------------|"); 
i=0; 
getdate(&d); 
sprintf(chtime,"%4d.%02d.%02d",d.da_year,d.da_mon,d.da_day); 
for(;;) 
{ 
gotoxy(3,24);printf(" Tab __browse cost list Esc __quit"); 
gotoxy(13,10);printf(" "); 
gotoxy(13,13);printf(" "); 
gotoxy(13,7);printf("%s",chtime); 
j=18; 
ch[0]=getch(); 
if(ch[0]==27) 
break; 
strcpy(chshop,""); 
strcpy(chmoney,""); 
if(ch[0]==9) 
{ 
mm:i=0; 
fp=fopen("home.dat","r+"); 
gotoxy(3,24);printf(" "); 
gotoxy(6,4);printf(" list records "); 
gotoxy(1,5);printf("|-------------------------------------|"); 
gotoxy(41,4);printf(" "); 
gotoxy(41,5);printf(" |"); 
while(fscanf(fp,"%10s%14s%f\n",chtime,chshop,&chm)!=EOF) 
{ if(i==36) 
{ getch(); 
i=0;} 
if ((i%36)<17) 
{ gotoxy(4,6+i); 
printf(" "); 
gotoxy(4,6+i);} 
else 
if((i%36)>16) 
{ gotoxy(41,4+i-17); 
printf(" "); 
gotoxy(42,4+i-17);} 
i++; 
sum=sum+chm; 
printf("%10s %-14s %6.1f\n",chtime,chshop,chm);} 
gotoxy(1,23);printf("|---------------------------------------------------------------------------|"); 
gotoxy(1,24);printf("| |"); 
gotoxy(1,25);printf("|---------------------------------------------------------------------------|"); 
gotoxy(10,24);printf("total is %8.1f$",sum); 
fclose(fp); 
gotoxy(49,24);printf("press any key to.....");getch();goto pp; 
} 
else 
{ 
while(ch[0]!='\r') 
{ if(j<10) 
{ strncat(chtime,ch,1); 
j++;} 
if(ch[0]==8) 
{ 
len=strlen(chtime)-1; 
if(j>15) 
{ len=len+1; j=11;} 
strcpy(ch1,""); 
j=j-2; 
strncat(ch1,chtime,len); 
strcpy(chtime,""); 
strncat(chtime,ch1,len-1); 
gotoxy(13,7);printf(" ");} 
gotoxy(13,7);printf("%s",chtime);ch[0]=getch(); 
if(ch[0]==9) 
goto mm; 
if(ch[0]==27) 
exit(1); 
} 
gotoxy(3,24);printf(" "); 
gotoxy(13,10); 
j=0; 
ch[0]=getch(); 
while(ch[0]!='\r') 
{ if (j<14) 
{ strncat(chshop,ch,1); 
j++;} 
if(ch[0]==8) 
{ len=strlen(chshop)-1; 
strcpy(ch1,""); 
j=j-2; 
strncat(ch1,chshop,len); 
strcpy(chshop,""); 
strncat(chshop,ch1,len-1); 
gotoxy(13,10);printf(" ");} 
gotoxy(13,10);printf("%s",chshop);ch[0]=getch();} 
gotoxy(13,13); 
j=0; 
ch[0]=getch(); 
while(ch[0]!='\r') 
{ if (j<6) 
{ strncat(chmoney,ch,1); 
j++;} 
if(ch[0]==8) 
{ len=strlen(chmoney)-1; 
strcpy(ch1,""); 
j=j-2; 
strncat(ch1,chmoney,len); 
strcpy(chmoney,""); 
strncat(chmoney,ch1,len-1); 
gotoxy(13,13);printf(" ");} 
gotoxy(13,13);printf("%s",chmoney);ch[0]=getch();} 
if((strlen(chshop)==0)||(strlen(chmoney)==0)) 
continue; 
if((fp=fopen("home.dat","a+"))!=NULL); 
fprintf(fp,"%10s%14s%6s",chtime,chshop,chmoney); 
fputc('\n',fp); 
fclose(fp); 
i++; 
gotoxy(41,5+i); 
printf("%10s %-14s %-6s",chtime,chshop,chmoney); 
}}}  
============================================================== 
【程序96】 
题目：计算字符串中子串出现的次数 
1.程序分析： 
2.程序源代码： 
#include "string.h" 
#include "stdio.h" 
main() 
{ char str1[20],str2[20],*p1,*p2; 
int sum=0; 
printf("please input two strings\n"); 
scanf("%s%s",str1,str2); 
p1=str1;p2=str2; 
while(*p1!='\0') 
{ 
if(*p1==*p2) 
{while(*p1==*p2&&*p2!='\0') 
{p1++; 
p2++;} 
} 
else 
p1++; 
if(*p2=='\0') 
sum++; 
p2=str2; 
} 
printf("%d",sum); 
getch();}  
============================================================== 
【程序97】 
题目：从键盘输入一些字符，逐个把它们送到磁盘上去，直到输入一个#为止。 
1.程序分析：　　　　　 
2.程序源代码： 
#include "stdio.h" 
main() 
{ FILE *fp; 
char ch,filename[10]; 
scanf("%s",filename); 
if((fp=fopen(filename,"w"))==NULL) 
{printf("cannot open file\n"); 
exit(0);} 
ch=getchar(); 
ch=getchar(); 
while(ch!='#') 
{fputc(ch,fp);putchar(ch); 
ch=getchar(); 
} 
fclose(fp); 
} 
============================================================== 
【程序98】 
题目：从键盘输入一个字符串，将小写字母全部转换成大写字母，然后输出到一个磁盘文件“test”中保存。 
　　　输入的字符串以！结束。  
1.程序分析： 
2.程序源代码： 
```
#include "stdio.h" 
main() 
{FILE *fp; 
char str[100],filename[10]; 
int i=0; 
if((fp=fopen("test","w"))==NULL) 
{ printf("cannot open the file\n"); 
exit(0);} 
printf("please input a string:\n"); 
gets(str); 
while(str!='!') 
{ if(str>='a'&&str<='z') 
str=str-32; 
fputc(str,fp); 
i++;} 
fclose(fp); 
fp=fopen("test","r"); 
fgets(str,strlen(str)+1,fp); 
printf("%s\n",str); 
fclose(fp); 
}  
```

19 回复：经典Ｃ源程序100例  
 【程序99】 
题目:有两个磁盘文件A和B,各存放一行字母,要求把这两个文件中的信息合并(按字母顺序排列),  
 输出到一个新文件C中. 
1.程序分析: 
2.程序源代码: 

```
#include "stdio.h" 
main() 
{ FILE *fp; 
int i,j,n,ni; 
char c[160],t,ch; 
if((fp=fopen("A","r"))==NULL) 
{printf("file A cannot be opened\n"); 
exit(0);} 
printf("\n A contents are :\n"); 
for(i=0;(ch=fgetc(fp))!=EOF;i++) 
{c[i]=ch; 
putchar(c[i]); 
} 
fclose(fp); 
ni=i; 
if((fp=fopen("B","r"))==NULL) 
{printf("file B cannot be opened\n"); 
exit(0);} 
printf("\n B contents are :\n"); 
for(i=0;(ch=fgetc(fp))!=EOF;i++) 
{c[i]=ch; 
putchar(c[i]); 
} 
fclose(fp); 
n=i; 
for(i=0;i<n;i++) 
for(j=i+1;j<n;j++) 
if(c[i]>c[j]) 
{t=c[i];c[i]=c[j];c[j]=t;} 
printf("\n C file is:\n"); 
fp=fopen("C","w"); 
for(i=0;i<n;i++) 
{ putc(c[i],fp); 
putchar(c[i]); 
} 
fclose(fp); 
} 
```

## 计算平均成绩并存入文件

有五个学生,每个学生有3门课的成绩,从键盘输入以上数据(包括学生号,姓名,三门课成绩),计算出平均成绩,况原有的数据和计算出的平均分数存放在磁盘文件"stud"中. 

```c
#include "stdio.h" 
struct student 
{ char num[6]; 
char name[8]; 
int score[3]; 
float avr; 
} stu[5]; 
main() 
{int i,j,sum; 
FILE *fp; 
/*input*/ 
for(i=0;i<5;i++) 
{ printf("\n please input No. %d score:\n",i); 
printf("stuNo:"); 
scanf("%s",stu[i].num); 
printf("name:"); 
scanf("%s",stu[i].name); 
sum=0; 
for(j=0;j<3;j++) 
{ printf("score %d.",j+1); 
scanf("%d",&stu[i].score[j]); 
sum+=stu[i].score[j]; 
} 
stu[i].avr=sum/3.0; 
} 
fp=fopen("stud","w"); 
for(i=0;i<5;i++) 
if(fwrite(&stu[i],sizeof(struct student),1,fp)!=1) 
printf("file write error\n"); 
fclose(fp); 
}  
```