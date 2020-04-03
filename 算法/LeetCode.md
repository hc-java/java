记录我在LeetCode敲过的代码

# 简单题

## 两数之和

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例:

给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]



~~~java
class Solution {
    public int[] twoSum(int[] nums, int target) {
    //    int[] nums = {2, 7, 11, 15};
    //    int target = 9;
  
      for(int i=0;i<nums.length;i++){
            int temp=nums[i];

            for(int j=i+1;j<nums.length;j++){
                if((temp +nums[j])==target){
                    //System.out.println(Arrays.toString(new int[]{i,j}));
                   return new int[]{i,j};

                }
            }
        }
    

    }
}
~~~



题解：有点类似于冒泡排序，时间复杂度O(n^2)

还有一种hash的解法

## 回文数


判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

**示例 1:**

```
输入: 121
输出: true
```

**示例 2:**

```
输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
```

**示例 3:**

```
输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。
```



~~~java
class Solution {
    public boolean isPalindrome(int x) {
        String s=x+"";
       char[] ch1=new char[s.length()];

       for(int i=s.length()-1,j=0;i>=0;i--,j++){
           ch1[j]=s.charAt(i);
       }

       String ch2=String.valueOf(ch1);
       if(s.equals(ch2)){
           return true;
       }else{
           return false;
       }

    }
}
~~~



题解：先int整数 转成String，然后倒叙；最后对比前后两个字符串是否一样



## 整数反转

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

示例 1:

输入: 123
输出: 321
 示例 2:

输入: -123
输出: -321
示例 3:

输入: 120
输出: 21
注意:

假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。



~~~java
class Solution {
    public int reverse(int x) {
        int num=0;
        double math=Math.pow(2,32);
        int math1=new Double(math).intValue();

         if(x>math1 || x<-math1){
                return num;
        }
        
       
        
        String s=String.valueOf(x);

        char[] c=s.toCharArray();
        
        String index=String.valueOf(s.charAt(0));
        String last=String.valueOf(s.charAt(s.length() -1));

        if(index.equals("-")&& last.equals("0")){
            x=(-x)/10;
            s=String.valueOf(x);
           
            //倒叙
            nixu(s);
             num=nixu(s);
            num=(-num);
          
        }else if(index.equals("-") && !last.equals("0")){
            x=(-x);
            s=String.valueOf(x);
           
             num=nixu(s);
            num=(-num);
            


        }else if(!index.equals("-") && last.equals("0")){
            x=x/10;
            s=String.valueOf(x);
        
             num=nixu(s);
            

        }else if(!index.equals("-") && !last.equals("0")){
            
             num=nixu(s);
         

        }
        return num;

    }
     public  int  nixu(String s){

        char[] ch=new char[s.length()];
        //System.out.println(s+","+s.length());
        for(int i=s.length()-1,j=0;i>=0;i--,j++){
            ch[j]=s.charAt(i);
        }
        String result=String.valueOf(ch);
        
        int num=Integer.parseInt(result);
        return num;
    }
}
~~~



执行出错信息：**Line 70: java.lang.NumberFormatException: For input string: "9646324351"**

最后执行的输入：**1534236469**



输入超出了int整型的范围，所以类型转换的时候出错。