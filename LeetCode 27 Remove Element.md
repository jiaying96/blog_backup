Given an array nums and a value val, remove all instances of that value in-place and return the new length.
Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.
The order of elements can be changed. It doesn't matter what you leave beyond the new length.
Example 1:
Given nums = [3,2,2,3], val = 3,

Your function should return length = 2, with the first two elements of nums being 2.

It doesn't matter what you leave beyond the returned length.

Example 2:
Given nums = [0,1,2,2,3,0,4,2], val = 2,

Your function should return length = 5, with the first five elements of nums containing 0, 1, 3, 0, and 4.

Note that the order of those five elements can be arbitrary.

### 分析题目要求：

 - 在适当位置删除该值的所有实例并返回新长度。
 -  不要为另一个数组分配额外的空间，必须通过使用O（1）额外内存修改输入数组来实现。
 - 元素的顺序可以改变

### 解决思路
从i数组下标i=0开始，若下标i对应的元素不是要val(要删除的值)，将其放到数组的第count个位置(初始count=0)，计数器加1，依次方式遍历数组即可得到结果。

### 代码

    class Solution {
        public int removeElement(int[] nums, int val) {
            int count=0;
            //遍历数组
            for(int i=0;i<nums.length;i++){
            //若下标i对应的元素不是要val(要删除的值)
                if(nums[i]!=val){
                    nums[count]=nums[i]; //将其放到数组的第count个位置
                    count++;//计数器加1
                }
            }
            return count;
        }
    }

