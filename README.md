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
