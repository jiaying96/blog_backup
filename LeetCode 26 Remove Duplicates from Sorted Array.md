Given a sorted array nums, remove the duplicates in-place such that each element appear only once and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

Example 1:

Given nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.

It doesn't matter what you leave beyond the returned length.
Example 2:

Given nums = [0,0,1,1,1,2,2,3,3,4],

Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively.

It doesn't matter what values are set beyond the returned length.

## 思路
**虽然函数只返回不同元素的个数，但是该题目从nums数组和不相同元素个数两方面看结果是否正确。**

从第一个元素（此时i=1）开始比较，若它与nuns[count]（初始时候count=0,nuns[count]即第0个元素)不相同，计数器（count）+1同时将该新值（与之前的值不同）给nums[count],原因有
- if条件判断是用nuns[count]与数组遍历中下一个元素比较，将nums[i]赋值给nums[count]，保证了下一次比较
- 将nums[i]赋值给nums[count]，保证了最后nums数组是正确的，即nums中存放的是与之前不同的值，即没有重复元素的值

i++，重复上述过程，可以得到正确结果。


```
class Solution {
    public int removeDuplicates(int[] nums) {
        if(nums.length==0)
            return 0;
        int count=0;
        for(int i=1;i<nums.length;i++){
            if(nums[i]!=nums[count]){
                //如果不相等，计数器加1
                count++;
                //把这个新值给nums[count],因为if条件判断是用nuns[count]与数组遍历中下一个元素比较
                nums[count]=nums[i];
            }
        }
        return count+1;       
    }
}
```

