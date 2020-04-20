---
title: leetcode寻找两个有序数组的中位数
date: 2020-04-20
tags:
 - leetcode
categories:
 - 技术分享
---

# leetcode寻找两个有序数组的中位数

## 题目

> 给定两个大小为 m 和 n 的有序数组 nums1 和 nums2。
> 
> 请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。
> 
> 你可以假设 nums1 和 nums2 不会同时为空。
> 
> 示例 1:
> 
> nums1 = [1, 3]
> nums2 = [2]
> 
> 则中位数是 2.0
> 
> 
> 示例 2:
> 
> nums1 = [1, 2]
> nums2 = [3, 4]
> 
> 则中位数是 (2 + 3)/2 = 2.5
> 
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/median-of-two-sorted-arrays

## 解决过程
### 概念
中位数：按顺序排列的一组数据中居于中间位置的数

### 要求
数据：数组2个
时间复杂:O(log(m + n))

#### 方法一 (合并 排列 选中数)

##### 分析

两个数组合并成一个数组，并且排列，选择中间位置的数
问题 奇数组和偶数组问题 （分开分析 或者 通用分析）

##### 实现

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;  
        //特殊判定
        if(n == 0){
            return findMedianSortedArrays(nums1);
        }
        else if(m == 0){
            return findMedianSortedArrays(nums2);
        }
        int[] nums;
        {
            nums = new int[n + m];
            //合并
            {
                for(int count = 0 ;count < m + n ; count++) 
                {
                    if (count < m) 
                    {
                        nums[count] = nums1[count];
                        continue;
                    }
                    nums[count] = nums2[count - m];
                }    
            }
            //排序(冒泡排序)
            {
                for(int i = 0; i < nums.length - 1; i++)
                {
                    for(int j = 0; j < nums.length - 1 - i; j++)
                    {
                        if(nums[j] > nums[j+1])
                        {
                            int temp = nums[j];
                            nums[j] = nums[j+1];
                            nums[j+1] = temp;
                        }       
                    }
                } 
            }
        }
        //返回结果
        return findMedianSortedArrays(nums);
    }   
	//一个数组找中位数
    public double findMedianSortedArrays(int[] nums) {
        if (nums.length % 2 == 0) {
            return (nums[nums.length  / 2 - 1] + nums[nums.length  / 2]) / 2.0;
        } 
        return nums[nums.length / 2];
    }

}
```

这种方法比较通用，但速度上会稍微损耗了一些，优化点有很多例如：采取更优排序，合并和排序同时进行（耦合度高）。



#### 方法二(切割 移动）

##### 概念

待整理

##### 分析

待整理

##### 实现

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int n = nums1.length;
        int m = nums2.length;  

      //特殊判定
        if(m == 0){
            return findMedianSortedArrays(nums1);
        }
        else if(n == 0){
            return findMedianSortedArrays(nums2);
        }

		if (n > m)  //保证数组1一定最短
		{
			return findMedianSortedArrays(nums2, nums1);
		}
	
		// Ci 为第i个数组的割,比如C1为2时表示第1个数组只有2个元素。
        // LMaxi为第i个数组割后的左元素。RMini为第i个数组割后的右元素。
		int LMax1 = Integer.MIN_VALUE , LMax2 =Integer.MIN_VALUE, \
            RMin1 =  Integer.MAX_VALUE , RMin2 =  Integer.MAX_VALUE \
            , c1, c2, lo = 0, hi = 2 * n;  //我们目前是虚拟加了'#'所以数组1是2*n长度
	
		while (lo <= hi)   //二分
		{
			c1 = (lo + hi) / 2;  //c1是二分的结果
			c2 = m + n - c1;
	
			LMax1 = (c1 == 0) ? Integer.MIN_VALUE : nums1[(c1 - 1) / 2];
			RMin1 = (c1 == 2 * n) ? Integer.MAX_VALUE : nums1[c1 / 2];
			LMax2 = (c2 == 0) ? Integer.MIN_VALUE : nums2[(c2 - 1) / 2];
			RMin2 = (c2 == 2 * m) ? Integer.MAX_VALUE : nums2[c2 / 2];
	
			if (LMax1 > RMin2)
				hi = c1 - 1;
			else if (LMax2 > RMin1)
				lo = c1 + 1;
			else
				break;
		}
		return ((LMax1 > LMax2 ? LMax1 : LMax2) + (RMin1 < RMin2 ? RMin1 : RMin2)) / 2.0;
	}
    //一个数组找中位数
    public double findMedianSortedArrays(int[] nums) {
        if (nums.length % 2 == 0) {
            return (nums[nums.length  / 2 - 1] + nums[nums.length  / 2]) / 2.0;
        } 
        return nums[nums.length / 2];
    }
}

```

## 总结

第一种方法不限制数组数量和序列的排序，每个步骤单独解决一个问题。第二种方法针对性强，速度快，两种方法比较，方法一的通用性会比第二种高，但速度和空间是需消耗资源。