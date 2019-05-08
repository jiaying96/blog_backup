## 背景知识：牛顿迭代法

快速寻找平方根可以很有效地求出根号n的近似值：首先随便猜一个近似值x，然后不断令x等于x和n/x的平均数，迭代个六七次后x的值就已经相当精确了。
    例如，我想求根号2等于多少。假如我猜测的结果为4，虽然错的离谱，但你可以看到使用牛顿迭代法后这个值很快就趋近于根号2了：

(       4  + 2/   4     ) / 2 = 2.25
(    2.25  + 2/   2.25  ) / 2 = 1.56944..
( 1.56944..+ 2/1.56944..) / 2 = 1.42189..
( 1.42189..+ 2/1.42189..) / 2 = 1.41423..


![在这里插入图片描述](https://img-blog.csdnimg.cn/20190120124127670.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDEzNTI4Mg==,size_16,color_FFFFFF,t_70)
经过(xi, f(xi))这个点的切线方程为：  f(x) = f(xi) + f’(xi)(x - xi)
其中f'(x)为f(x)的导数。
令切线方程等于0，即可求出xi+1=xi - f(xi) / f'(xi)。
f(x)=x&sup2;-n,f'(x)=2x代入后继续化简
	xi+1
=xi - f(xi) / f'(xi)  
=xi - (xi&sup2;- n) / (2xi) 
= xi - xi / 2 + n / (2xi) 
= xi / 2 + n / 2xi 
= (xi + n/xi) / 2
故迭代公式为：<font face="times new roman" color=red size=4>xi+1= (xi + n/xi) / 2</font>

参考文章：
http://www.matrix67.com/blog/archives/361
https://www.cnblogs.com/qlky/p/7735145.html

## 题目描述：
Implement int sqrt(int x).
Compute and return the square root of x, where x is guaranteed to be a non-negative integer.

Since the return type is an integer, the decimal digits are truncated and only the integer part of the result is returned.

Example 1:

Input: 4
Output: 2
Example 2:

Input: 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since 
             the decimal part is truncated, 2 is returned.
## 代码

    class Solution {
        public int mySqrt(int x) {
           long r = x ;//初始化数字为x
           while (r*r > x)   //当r*r=x时候r为所求  因为初始化r=x 所以r*r>x  当r*r<=x时候退出循环
              r = (r + x/r) / 2; //利用牛顿迭代法的迭代公式进行迭代
           return (int) r;//返回结果
        }
    }
