# Manacher算法

[TOC]

## 学习所需要的基础

### 快速判断一个数的奇偶

```java
i&1  
//任意一个偶数的二进制最后一位数为0
//因此,i&1为基数时,为1  偶数时,为0
```

### 快速扩充为#字符串

```java
public char[] manacherString(String str){
    char [] charArr=str.toCharArray();
    // [0]→[#0#]  2*1+1
    // [10]->[#1#0#]  2*2+1
    char [] res=new char[str.length()*2+1];
    int index=0;
    for(int i=0;i!=res.length;i++){
        res[i]=(i&1)==0?'#':charArr[index++];
    }
    return res;
}
```



## manacher算法的原理解释

1. 首先,我们需要声明两个定义
   * 最右边界:回文序列中的最右值,如果一个str中存在多个str,那么最右边界就是回文串的最右的东西
   * 中心  一个字符串中有很多个回文序列 这就意味着他有多个中心
2. 四种情况
   * 如果 R在一个中心序列的左边,那么直接暴力移动
   * 如果R在一个中心序列的右边
     * 如果,我们此时的i对应的i'在最大会问序列中的内部,那么此时该i的最右序列就是i'最右的序列 最右R不变
     * 如果,此时i对应的i'的会问在内部,那么i的最右就是R
     * 如果R'和i'对应,那么继续推广.
3. 反正我觉得 我完全没解释清楚我刚刚到底在说啥.



## manacher算法代码理解

```java
public int maxLcpsLength(String str){
    if(str==null||str.length()==0)
        return 0;
    char [] charArr=manacherString(str);
    //初始化中心点 R 和最大半径
    int C=-1;
    int R=-1;
    int max=Integer.MIN_VALUE;
    //记录每个字符的回文半径  这里可以先不初始化为1
    int [] pArr=new int[charArr.length];
    for(int i=0;i!=charArr.length;i++){
        //当我们发现,R最右在i前面的时候,我们需要进行四种情况中的2/3的判断,否则初始化为1,就是我之前说的初始化
        //注意到,这是在比较R-i和i'半径之间的大小,那个小,他的回文半径就是那个,如果相等,那么还得重新算
        pArr[i]=R>i?Math.min(pArr[2*C-1],R-i):1;
        //在知晓当前i和之前的回文半径的时候,开始进行穷举
        while(i+pArr[i]<charArr.length&&i-pArr[i]>-1){
            //防止下面的等式失败越界
            //由于情况2 情况3这两种情况下,是不可能相等的,这就是天然屏障 
            //之所以不直接让R>i 是因为,还可能是情况四  情况四的结果是,R可能原地踏步,也可能不动
            if(charArr[i+pArr[i]]==charArr[i-Arr[i]])
               //i的半径+1
                pArr[i]++;
        	else
                break;      
        }
        if(i+pArr[i]>R){
            R=i+pArr[i];
            C=i;
        }
        max=Math.max(max,pArr[i]);   
    }
    //这也是个知识点!!!
    return max-1;
}
```

















