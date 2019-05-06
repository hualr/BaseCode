# Manacher算法

[TOC]

## 基础

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
        //这一步处理的非常高明
        res[i]=(i&1)==0?'#':charArr[index++];
    }
    return res;
}
```



## 原理解释

为什么需要该算法

单纯的对数组进行#扩展,也是完全可以解决该问题的.但是manacher算法本身不复杂,因此可以用来加速

1. 首先,我们需要声明两个定义
   * 最右回文边界:回文序列中的最右值,如果一个str中存在多个str,那么最右边界就是回文串的最右的东西
   * 最右回文边界的中心 
2. 四种情况
   * 如果 R在一个中心序列C的左边,那么直接暴力移动
   * 如果R在一个中心序列C的右边
     * 如果,我们此时的i对应的i'在最大会问序列中的内部,那么此时该i的最右序列就是i'最右的序列 最右R不变
     * 当i对应的i'的值的回文半径在以C为中心 R为半径的范围内,那么,i的回文半径就是i'的回文半径
     * 当i对应的i'的值的回文半径在以C为中心 R为半径的范围外,那么,i的回文半径就是R-i
     * 当刚好从i的的时候,那就得测算了
3. 该算法的优势
   1. 普通算法是每次进行扩充
   2. 而该算法的2 3两种情况的回文半径是直接可以被测算的
   3. 而情况四我们至少知道半径最少的值 因此减少了扩充  





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
    //该数组用来记录每个字符的回文半径
    int [] pArr=new int[charArr.length];
    for(int i=0;i!=charArr.length;i++){
        //当我们发现,R最右在i前面的时候,我们需要进行四种情况中的2/3的判断,
        //注意到,这是在比较R-i和i'半径之间的大小,那个小,他的回文半径就是那个,如果相等,那么还得重新算
        pArr[i]=R>i?Math.min(pArr[2*C-i],R-i):1;
        //在知晓当前i和之前的回文半径的时候,开始进行穷举
        //2 两种情况是直接在while中的if中被算了一遍 但是完全没问题
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
        //找到新的更右回文边界
        if(i+pArr[i]>R){
            R=i+pArr[i];
            C=i;
        }
        max=Math.max(max,pArr[i]);   
    }
    //parr是个好东西
    //这也是个知识点!!! 因为之前我是扩充了的
    return max-1;
}
```

















