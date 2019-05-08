## 题目描述：

Given two binary strings, return their sum (also a binary string).
The input strings are both non-empty and contains only characters 1 or 0.

Example 1:
Input: a = "11", b = "1"
Output: "100"

Example 2:
Input: a = "1010", b = "1011"
Output: "10101"


## 思路

获取两个字符串的长度下标范围（0~length-1），初始化进位为0,从两个字符串的最后一位开始，判断是否移动到两个字符串当前位置都没有数字，没有则停止，若a或b还有数字，则初始sum为carrry,若某字符串当前位有数字，则加到sum，指针位置向前移动,sum除以2余数附加到StringBuilder对象sb（因为sum=0时候附加0进位0，结果为1附加1进位0，结果为2附加0进位1）,可以发现进位为sum/2,两个字符串指针位置都移到最前面没有数字的时候，判断是否有进位不为0，若不为0则附加到StringBuilder对象sb，真实数字是从左到右，附加到StringBuilder对象sb的时候是从右往左，所以需要反转然后转化为字符串并返回

## 代码

      public class Solution {
        public String addBinary(String a, String b) {
            StringBuilder sb=new StringBuilder();
            int i=a.length()-1;//获取字符串a的长度下标范围（0~length-1）
            int j=b.length()-1;//获取字符串b的长度下标范围（0~length-1）
            int carry=0;//初始化进位为0
            while(i>=0||j>=0){//i，j相当于指针，i或j大于0说明a或b当前指针位置有数字，当a和b都没有数组时停止循环
                int sum=carry;//初始化sum为carry
                if(i>=0) sum+=a.charAt(i)-'0';//获取i指针位置字符串a的数字
                i--;//i指针位置前移一位
                if(j>=0) sum+=b.charAt(j)-'0';//获取i指针位置字符串b的数字
                j--;//j指针位置前移一位
                sb.append(sum%2);//附加到StringBuilder对象sb
                carry=sum/2;//求进位
        }
        if(carry!=0)
            sb.append(carry);//若进位不为0附加到StringBuilder对象sb
        return sb.reverse().toString();//反转并转化为字符串返回
        }
    }
