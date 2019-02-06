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
  </h3>

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




















<h3>39.和为S的两个数字：输入一个递增排序的数组和一个数字S，在数组中查找两个数，
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


<h3>40:翻转单词顺序列
  例如，“student. a am I”。后来才意识到，这家伙原来把句子单词的顺序翻转了，
  正确的句子应该是“I am a student.”。Cat对一一的翻转这些单词顺序可不在行，你能帮助他么？</h3>
  
```
方法一：

 public String ReverseSentence(String str) {
        if(str.trim().equals("")){
            return str;
        }
        String[] a = str.split(" ");
        StringBuffer o = new StringBuffer();
        int i;
        for (i = a.length; i >0;i--){
            o.append(a[i-1]);
            if(i > 1){
                o.append(" ");
            }
        }
        return o.toString();
    }
    
方法二：

public String ReverseSentence(String str) {
    if (str.trim().equals("") && str.length() > 0) {
        return str; 
    }
    Stack reverse = new Stack();
    String string = str.trim();
    String[] strings = string.split(" ");
    for (int i = 0; i length; i++) {
        reverse.push(strings[i]);
    }
    string = reverse.pop();
    while (!reverse.isEmpty()) {
        string = string + " " + reverse.pop();
    }
    return string;
}
```


<h3>41:扑克牌顺子
  “红心A,黑桃3,小王,大王,方片5”,“Oh My God!”不是顺子.....LL不高兴了,他想了想,决定大\小 
  王可以看成任何数字,并且A看作1,J为11,Q为12,K为13。上面的5张牌就可以
  变成“1,2,3,4,5”(大小王分别看作2和4),“So Lucky!”。LL决定去买体育彩票啦。 
  现在,要求你使用这幅牌模拟上面的过程,然后告诉我们LL的运气如何， 如果牌能组成顺子就输出true，
  否则就输出false。为了方便起见,你可以认为大小王是0。？</h3>


```
public static boolean isContinuous(int [] numbers) {

		 int count =0;
		 int max=0;
		
		Arrays.sort(numbers);
          if(numbers.length == 0){
           return false;
        }
		for(int i = 0;i<numbers.length-1;i++) {
            
			if(numbers[i]==0) {
				count++;
				continue;
			}
			 if (numbers[i] == numbers[i + 1]) {
                return false;
            }
			max+=numbers[i+1]-numbers[i]-1;
		}
		return max>count?false:true;
		
		
	    }

```













<h3>42:孩子们的游戏(圆圈中最后剩下的数)
  其中,有个游戏是这样的:首先,让小朋友们围成一个大圈。然后,他随机指定一个数m,让编号为0的小朋友开始报数。
	每次喊到m-1的那个小朋友要出列唱首歌,然后可以在礼品箱中任意的挑选礼物,并且不再回到圈中,
	从他的下一个小朋友开始,继续0...m-1报数....这样下去....直到剩下最后一个小朋友,可以不用表演,
	并且拿到牛客名贵的“名侦探柯南”典藏版(名额有限哦!!^_^)。请你试着想下,哪个小朋友会得到这份礼品呢？
	(注：小朋友的编号是从0到n-1)


```

用Java实现的话，可以使用LinkedList，考虑删除节点的效率。模拟游戏过程即可：
public class Solution {
    public int LastRemaining_Solution(int n, int m) {
        LinkedList<Integer> list = new LinkedList<Integer>();
        for (int i = 0; i < n; i ++) {
            list.add(i);
        }
         
        int bt = 0;
        while (list.size() > 1) {
            bt = (bt + m - 1) % list.size();
            list.remove(bt);
        }
         
        return list.size() == 1 ? list.get(0) : -1;
    }
}
```
















<h3>43.求1+2+3+...+n: 要求不能使用乘除法、for、while、if、else、switch、case
	等关键字及条件判断语句（A?B:C）。</h3>
  
```

解题思路：
1.需利用逻辑与的短路特性实现递归终止。 2.当n==0时，(n>0)&&((sum+=Sum_Solution(n-1))>0)只执行前面的判断，为false，然后直接返回0；
3.当n>0时，执行sum+=Sum_Solution(n-1)，实现递归计算Sum_Solution(n)。
    public int Sum_Solution(int n) {
        int sum = n;
        boolean ans = (n>0)&&((sum+=Sum_Solution(n-1))>0);
        return sum;
    }
```





<h3>44.不用加减乘除做加法: 写一个函数，求两个整数之和，
	要求在函数体内不得使用+、-、*、/四则运算符号。
</h3>

```
1.两个数异或：相当于每一位相加，而不考虑进位；
2.两个数相与，并左移一位：相当于求得进位；
3.将上述两步的结果相加

public int Add(int num1,int num2) {
    while( num2!=0 ){
        int sum = num1 ^ num2;
        int carray = (num1 & num2) << 1; //用于进位，当为0时表示没有进位了
        num1 = sum;
        num2 = carray;
    }
    return num1;
}
```




<h3>46.将字符串转换成整数
	将一个字符串转换成一个整数(实现Integer.valueOf(string)的功能，
	但是string不符合数字要求时返回0)，要求不能使用字符串转换整数的库函数。
	数值为0或者字符串不是一个合法的数值则返回0。
</h3>

```
public static int StrToInt(String str) {
	        int res=0;
	        int flag=1;
	        
		 if(str.length()==0) return 0;
		 char[] c = str.toCharArray();
		 if(c[0]=='-') {
			 flag=-1;
			 c[0]='0';
		 }
		 if(c[0]=='+') {
			 flag=1;
			 c[0]='0';
		 }
		 for(int i = 0;i<c.length;i++) 
		 {
			 if(c[i]<'0'||c[i]>'9') {
				 return 0;
			 }
			 res=res*10+c[i]-'0';
		 }
		 return res*flag;



}
```





















<h3>47.数组中重复的数字
在一个长度为n的数组里的所有数字都在0到n-1的范围内。 数组中某些数字是重复的，
	但不知道有几个数字是重复的。也不知道每个数字重复几次。请找出数组中任意一个重复的数字。
	例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2。

</h3>

```
//boolean只占一位，所以还是比较省的

public boolean duplicate(int numbers[], int length, int[] duplication) {
        boolean[] k = new boolean[length];
        for (int i = 0; i < k.length; i++) {
            if (k[numbers[i]] == true) {
                duplication[0] = numbers[i];
                return true;
            }
            k[numbers[i]] = true;
        }
        return false;
    }
    
```













<h3>48.构建乘积数组
给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],
	其中B中的元素B[i]=A[0]*A[1]*...*A[i-1]*A[i+1]*...*A[n-1]。不能使用除法。

</h3>

```

剑指的思路：
B[i]的值可以看作下图的矩阵中每行的乘积。
下三角用连乘可以很容求得，上三角，从下向上也是连乘。
因此我们的思路就很清晰了，先算下三角中的连乘，即我们先算出B[i]中的一部分，然后倒过来按上三角中的分布规律，把另一部分也乘进去。
  public int[] multiply(int[] A) {
        int length = A.length;
        int[] B = new int[length];
        if(length != 0 ){
            B[0] = 1;
            //计算下三角连乘
            for(int i = 1; i < length; i++){
                B[i] = B[i-1] * A[i-1];
            }
            int temp = 1;
            //计算上三角
            for(int j = length-2; j >= 0; j--){
                temp *= A[j+1];
                B[j] *= temp;
            }
        }
        return B;
    }
```































<h3>49.正则表达式匹配
请实现一个函数用来匹配包括'.'和'*'的正则表达式。模式中的字符'.'表示任意一个字符，
	而'*'表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的
	所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但是与"aa.a"和"ab*a"均不匹配

</h3>

```

当模式中的第二个字符不是“*”时：
1、如果字符串第一个字符和模式中的第一个字符相匹配，那么字符串和模式都后移一个字符，然后匹配剩余的。
2、如果 字符串第一个字符和模式中的第一个字符相不匹配，直接返回false。

而当模式中的第二个字符是“*”时：
如果字符串第一个字符跟模式第一个字符不匹配，则模式后移2个字符，继续匹配。如果字符串第一个字符跟模式第一个字符匹配，可以有3种匹配方式：
1、模式后移2字符，相当于x*被忽略；
2、字符串后移1字符，模式后移2字符；
3、字符串后移1字符，模式不变，即继续匹配字符下一位，因为*可以匹配多位；

这里需要注意的是：Java里，要时刻检验数组是否越界。
public class Solution {
    public boolean match(char[] str, char[] pattern) {
    if (str == null || pattern == null) {
        return false;
    }
    int strIndex = 0;
    int patternIndex = 0;
    return matchCore(str, strIndex, pattern, patternIndex);
}
  
public boolean matchCore(char[] str, int strIndex, char[] pattern, int patternIndex) {
    //有效性检验：str到尾，pattern到尾，匹配成功
    if (strIndex == str.length && patternIndex == pattern.length) {
        return true;
    }
    //pattern先到尾，匹配失败
    if (strIndex != str.length && patternIndex == pattern.length) {
        return false;
    }
    //模式第2个是*，且字符串第1个跟模式第1个匹配,分3种匹配模式；如不匹配，模式后移2位
    if (patternIndex + 1 < pattern.length && pattern[patternIndex + 1] == '*') {
        if ((strIndex != str.length && pattern[patternIndex] == str[strIndex]) || (pattern[patternIndex] == '.' && strIndex != str.length)) {
            return matchCore(str, strIndex, pattern, patternIndex + 2)//模式后移2，视为x*匹配0个字符
                    || matchCore(str, strIndex + 1, pattern, patternIndex + 2)//视为模式匹配1个字符
                    || matchCore(str, strIndex + 1, pattern, patternIndex);//*匹配1个，再匹配str中的下一个
        } else {
            return matchCore(str, strIndex, pattern, patternIndex + 2);
        }
    }
    //模式第2个不是*，且字符串第1个跟模式第1个匹配，则都后移1位，否则直接返回false
    if ((strIndex != str.length && pattern[patternIndex] == str[strIndex]) || (pattern[patternIndex] == '.' && strIndex != str.length)) {
        return matchCore(str, strIndex + 1, pattern, patternIndex + 1);
    }
    return false;
    }
}
```


































<h3>50.删除链表中重复的结点:在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点
，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5
</h3>

```
public ListNode deleteDuplication(ListNode pHead)
    {	
        if(pHead ==null||pHead.next==null) 
        {return pHead;}
        else{
	//新建一个节点，防止头结点要被删除
		ListNode n= new ListNode(0);
		n.next=pHead;
		ListNode cur =pHead;
		ListNode pre =n;
		ListNode next=null;
		while(cur !=null&&cur.next!=null) {
			next=cur.next;
			if(cur.val==next.val) {//如果当前节点的值和下一个节点的值相等
				while(next!=null&&cur.val==next.val) {//向后重复查找
					next=next.next;
				}
				pre.next=next;//指针赋值，就相当于删除
				cur=next;//cur依次和之前一样向前移动
			}	
			else {//如果当前节点和下一个节点值不等，则向后移动一位
				pre=cur;
				cur=cur.next;
			}
		}
		return n.next;
        }
    }
```



























<h3>51.二叉树的下一个结点:给定一个二叉树和其中的一个结点，请找出中序遍历顺序的
	下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。
</h3>

```

我们可发现分成两大类：1、有右子树的，那么下个结点就是右子树最左边的点；（eg：D，B，E，A，C，G） 
2、没有右子树的，也可以分成两类，a)是父节点左孩子（eg：N，I，L） ，那么父节点就是下一个节点 ； 
b)是父节点的右孩子（eg：H，J，K，M）找他的父节点的父节点的父节点...直到当前结点是其父节点的左孩子位置。
如果没有eg：M，那么他就是尾节点。

public class Solution {
    TreeLinkNode GetNext(TreeLinkNode node)
    {
        if(node==null) return null;
        if(node.right!=null){    //如果有右子树，则找右子树的最左节点
            node = node.right;
            while(node.left!=null) node = node.left;
            return node;
        }
        while(node.next!=null){ //没右子树，则找第一个当前节点是父节点左孩子的节点
            if(node.next.left==node) return node.next;
            node = node.next;
        }
        return null;   //退到了根节点仍没找到，则返回null
    }
}
```




<h3>52.对称的二叉树：请实现一个函数，用来判断一颗二叉树是不是对称的。
	注意，如果一个二叉树同此二叉树的镜像是同样的，定义其为对称的。
</h3>

```
/*思路：首先根节点以及其左右子树，左子树的左子树和右子树的右子树相同
* 左子树的右子树和右子树的左子树相同即可，采用递归
* 非递归也可，采用栈或队列存取各级子树根节点
*/
public class Solution {
    boolean isSymmetrical(TreeNode pRoot)
    {
        if(pRoot == null){
            return true;
        }
        return comRoot(pRoot.left, pRoot.right);
    }
    private boolean comRoot(TreeNode left, TreeNode right) {
        // TODO Auto-generated method stub
        if(left == null) return right==null;
        if(right == null) return false;
        if(left.val != right.val) return false;
        return comRoot(left.right, right.left) && comRoot(left.left, right.right);
    }
}
```


<h3>53.按之字形顺序打印二叉树:请实现一个函数按照之字形打印二叉树，即第一行按照从左到右的顺序打印，
	第二层按照从右至左的顺序打印，第三行按照从左到右的顺序打印，其他行以此类推。
</h3>

```

public class Solution {
    public ArrayList<ArrayList<Integer> > Print(TreeNode pRoot) {
        ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>>();
        Stack<TreeNode> stack1 = new Stack<TreeNode>();//存奇数行节点
        Stack<TreeNode> stack2 = new Stack<TreeNode>();//存偶数行节点
        if(pRoot==null) return result;
        stack1.push(pRoot);
        boolean flag=true;
        TreeNode node;
        while(!stack1.isEmpty()||!stack2.isEmpty()){
            ArrayList<Integer> list = new ArrayList<Integer>();
            if(!stack1.isEmpty()){
                while(!stack1.isEmpty()){
                    node = stack1.pop();
                    list.add(node.val);
                    if(node.left!=null) stack2.push(node.left);
                    if(node.right!=null) stack2.push(node.right);
                }
            }else{
                while(!stack2.isEmpty()){
                    node = stack2.pop();
                    list.add(node.val);
                    if(node.right!=null) stack1.push(node.right);
                    if(node.left!=null) stack1.push(node.left);
                }
            }
            result.add(list);
        }
        return result;
    }
}
```











<h3>53.把二叉树打印成多行：
	从上到下按层打印二叉树，同一层结点从左至右输出。每一层输出一行。

</h3>

```
/*
* 队列LinkedList完成层序遍历，用end记录每层结点数目
*/
public class Solution {
    ArrayList<ArrayList<Integer> > Print(TreeNode pRoot) {
        ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>>();
        if(pRoot == null){
            return result;
        }
        Queue<TreeNode> layer = new LinkedList<TreeNode>();
        ArrayList<Integer> layerList = new ArrayList<Integer>();
        layer.add(pRoot);
        int start = 0, end = 1;
        while(!layer.isEmpty()){
            TreeNode cur = layer.remove();
            layerList.add(cur.val);
            start++;
            if(cur.left!=null){
                layer.add(cur.left);           
            }
            if(cur.right!=null){
                layer.add(cur.right);
            }
            if(start == end){
                end = layer.size();
                start = 0;
                result.add(layerList);
                layerList = new ArrayList<Integer>();
            }
        }
        return result;
    }
}
```




<h3>54.序列化二叉树：
	请实现两个函数，分别用来序列化和反序列化二叉树

</h3>

```
/*
    算法思想：根据前序遍历规则完成序列化与反序列化。所谓序列化指的是遍历二叉树为字符串；所谓反序列化指的是依据字符串重新构造成二叉树。
    依据前序遍历序列来序列化二叉树，因为前序遍历序列是从根结点开始的。当在遍历二叉树时碰到Null指针时，这些Null指针被序列化为一个特殊的字符“#”。
    另外，结点之间的数值用逗号隔开。
*/
public class Solution {
    int index = -1;   //计数变量
  
    String Serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        if(root == null){
            sb.append("#,");
            return sb.toString();
        }
        sb.append(root.val + ",");
        sb.append(Serialize(root.left));
        sb.append(Serialize(root.right));
        return sb.toString();
  }
    TreeNode Deserialize(String str) {
        index++;
        //int len = str.length();
        //if(index >= len){
        //    return null;
       // }
        String[] strr = str.split(",");
        TreeNode node = null;
        if(!strr[index].equals("#")){
            node = new TreeNode(Integer.valueOf(strr[index]));
            node.left = Deserialize(str);
            node.right = Deserialize(str);
        }
        return node;
  }
}
```
