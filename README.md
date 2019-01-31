# offer2


<h3>37.数组中只出现一次的数字：一个整型数组里除了两个数字之外，
  其他的数字都出现了偶数次。请写程序找出这两个只出现一次的数字。</h3>
  
```

　问题一：在一个整数数组中，除了一个数之外，其他的数出现的次数都是两次，
 求出现一次的数，要求时间复杂度尽可能的小。例如数组{1,2,2,3,3,6,6},出现一次的数是1.
 
从题目的描述可以看出，数组中只有一个数字出现了一次，其他的数字都出现两次
  ，联想到异或运算的特点：任何一个数字和自己做异或运算的结果都是0，
  任何数字和0运算的结果都是本身。根据上述特点，可以考虑从数组的第一个元素开始，
  逐个和后面的元素做异或操作，最后的计算结果就是要找的只出现一次的数。
  
  　根据上面问题一的思路，我们依然从头到位对数组做异或运算，得到的最终结果应该是两个不同数字做异或运算后的值。
   因为两个数字不相同，最终的结果也肯定不是0，切结果对应的二进制位中至少有一个为1，
   我们找到二进制位中第一个是1的位置，即为n，然后根据地n为是1还是0把数组分为两个子数组
   ，第一个子数组中的每个数的二进制位的第n位都是1，另外一个是0，这样分完后，
   相同的数字肯定会被分到同一个子数组中，并且每个子数组中只包含一个只出现一次的数，
   根据问题一，我们可以很方便的求出两个只出现一次的数，实现如下：
   
 public static int getNumAppearsOnce(int[] nums){

        if(nums == null || nums.length <= 2){
            throw new IllegalArgumentException("nums size must bigger than 2");
        }

        int result = 0;
        for(int i=0;i<nums.length;i++){
            result ^= nums[i];
        }

        return result;
    }

    public static List<Integer> getNumsAppearsOnce(int[] nums){

        if(nums == null || nums.length <= 2){
            throw new IllegalArgumentException("nums size must bigger than 2");
        }

        List<Integer> result = new ArrayList<Integer>(2);
        int temp = getNumAppearsOnce(nums);

        int index = findFirstBitIdOne(temp);
        int num1 = 0,num2 = 0;

        for(int i=0;i<nums.length;i++){
            if(isBitOne(nums[i],index)){
                num1 ^= nums[i];
            }else{
                num2 ^= nums[i];
            }
        }

        result.add(num1);
        result.add(num2);
        return result;
    }

    private static boolean isBitOne(int num, int index) {
        num = num >> index;
        return (num & 1) == 1;
    }

    private static int findFirstBitIdOne(int temp) {
        int index = 0;
        while((temp & 1) == 0){
            temp =  temp >> 1;
            index++;
        }
        return index;
    }
```






<h3>38.和为S的连续正数序列
小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,他马上就写出了正确答案是100。
  但是他并不满足于此,他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。
  没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。现在把问题交给你,
  你能不能也很快的找出所有和为S的连续正数序列? Good Luck!

```

在答案区找到一个答案，说的很好，叫做双指针技术，就是相当于有一个窗口，窗口的左右两边就是两个指针，我们根据窗口内值之和来确定窗口的位置和宽度。非常牛逼的思路，虽然双指针或者所谓的滑动窗口技巧还是蛮常见的，但是这一题还真想不到这个思路。

import java.util.ArrayList;
public class Solution {
    public ArrayList<ArrayList<Integer> > FindContinuousSequence(int sum) {
        //存放结果
        ArrayList<ArrayList<Integer> > result = new ArrayList<>();
        //两个起点，相当于动态窗口的两边，根据其窗口内的值的和来确定窗口的位置和大小
        int plow = 1,phigh = 2;
        while(phigh > plow){
            //由于是连续的，差为1的一个序列，那么求和公式是(a0+an)*n/2
            int cur = (phigh + plow) * (phigh - plow + 1) / 2;
            //相等，那么就将窗口范围的所有数添加进结果集
            if(cur == sum){
                ArrayList<Integer> list = new ArrayList<>();
                for(int i=plow;i<=phigh;i++){
                    list.add(i);
                }
                result.add(list);
                plow++;
            //如果当前窗口内的值之和小于sum，那么右边窗口右移一下
            }else if(cur < sum){
                phigh++;
            }else{
            //如果当前窗口内的值之和大于sum，那么左边窗口右移一下
                plow++;
            }
        }
        return result;
    }
}



```















<h3>38.和为S的两个数字：输入一个递增排序的数组和一个数字S，在数组中查找两个数，
  使得他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。</h3>
```
public class Solution {
   public ArrayList<Integer> FindNumbersWithSum(int [] array,int sum) {
        if(array.length==0) return null;
        ArrayList<ArrayList<Integer>> list=new ArrayList<ArrayList<Integer>>();
        int l=0,r=1,min=Integer.MAX_VALUE,temp=0,flag=0;
        while(r<=array.length-1) {
        	if(array[l]+array[r]==sum) {
        		ArrayList<Integer> li=new ArrayList<Integer>();
        		li.add(array[l]);
        		li.add(array[r]);
        		l++;
        		r++;
        		list.add(li);
        	}
        	else {
        		l++;
        		r++;
        	}
        }
        if(list.size()!=1) {
         for(int i =0;i<list.size();i++) {
        	 
        		temp= list.get(i).get(0)*list.get(i).get(1);
        		if(temp<min) {
               	 min=temp;
               	 flag=i;
                }
        		
         }
         return list.get(flag);
         
        	
        }
        
        return list.get(0);
    }
}
```
