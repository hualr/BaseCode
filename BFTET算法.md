# TOP-K问题

[TOC]



## 问题要求

求解一个数组中,第k大的数字,一般运用很多方面



## 使用堆解决

### 具体步骤

1. 首先,将数组中前K个数建立小根堆
2. 然后将剩下的数字和小根堆的堆顶(目前最大的数比较),发现更大的压入,吐出一个最小的,发现更小忽视
3. 最终的堆应该是数组中最大的K个数,最上面就是这k个数字中最小的一个,也就是第K大的数字

### 具体代码

```java
public class TopHeapSelect{
    public int findKthLarget(int []num,int k){
        if(num.length<k) return 0;
        //容量首先是k+1 因为要先加进去一个,然后再拿出一个,维持k 个
        Queue<Integer>minQueue=new PriorityQueue<>(k+1);
        for(int n:num){
            //如果小根堆没有装到k,那么加进去,这个过程只会在开始经历
            if(minQueue.size()<k)
                minQueue.offer(n);
            //下面的过程只会在堆的大小为k的时候出现
            else{
                //如果加进去的数字比堆顶部大  那么加进去一个,然后拖出来一个
                if(n>minQueue.peek()){
                    minQueue.offer(n);
                    minQueue.poll();
            	}
        }
    }
}
```



## 使用快速排序解决

### 荷兰国旗问题

1. 问题定义 给定一个数组arr和数字num  将小于num的所有数字放在左边 等于num的所有数字放在中间 大于num的所有数字放在右边

2. 解决方法

   1. 首先定义两个位置,
      1. 也就是小于num的数字群0-x, 此时为start-1 也就是-1,即0- -1
      2. 大于num的数字群y-end  也就是end+1-end 都不存在
   2. 定义当前的指针为cur
   3. 发现我们当前的数字就是k  那么就向前移动cur
   4. 发现当前的数字小于k 
      1. 首先,找到小数群,确定小数群最后一个位置的下一位A---也就是等数群
      2. 然后将当前的数字和A交换,小数群++ 
      3. 也就是说,当先小于N的数字进入了小数群,而等数群最后一个数被换到等数群最前面了 等数群内部进行交换
      4. 指针cur自然继续向前走--等数群头部减小了 ,尾部增加了
   5. 发现当前的数大于k
      1. 首先找到大数群最前面一个位置的上一位B---也就是未分类数群
      2. 然后将当前的数字和B交换,大数群自然增加了一员 大数群++
      3. 判断数字B的情况

   ```java
   private int[] partition(int[] nums,int start,int end){
       //一般start为0 end为arr.length-1 cur为0
       int less=start-1;
       int more=end + 1;
       int cur=start;
       
       //这个时候,我们的基准数k就是nums[end],只要等数群和大数群不接壤,就走
       while(cur<more){
           if(nums[cur]<nums[end])
               //小数群的最后一位 less+1和cur交换,然后小数群++ 当前数也向前移动一个格子
               swap(nums,++less,cur++);
           	//大数群的前面一位和cur交换,然后大数群-- (范围扩大)
           else if(nums[cur]>nums[end])
               swap(nums,--more,cur);
           else
               //等于的话cur++
               cur++;
       }
       //返回等数区域
       return new int[]{less+1,more-1};
   }
   
   ```

**非常值得注意的一点是,千万不要将数组的角标和数组的值搞混,我就是因为这一点思考了非常久**

### 经典快速排序

```java
public void quickSort(int[] arr,int L,int R){
 	//保证L<R
    if(L<R){
        int []p=partition(arr,L,R);
        quickSort(arr,L,p[0]-1);
        quickSort(arr,p[1]+1,R)
    }
}
```



### 快速排序解决TOPK问题

1. 我们知道，根据荷兰国旗方法的划分，可以划分出三个区间
2. 第k大的意思就是在数组中找到索引为k-1的位置是否是确定值（等数群中）
3. 假设等数群的中间一个区间的第一个索引大于k-1,那么直接在小数区间内找索引为k-1的数字，继续排序即可
4. 假设等数群的中间一个区间索引包含k-1,直接返回等数群的任意一个数即可
5. 假设等数群的中间一个区间值的最后一个索引大于k-1，那么在数组中找到第ｋ大的数则意味着在大数群中找索引为k-1的数

```java
public int getKthNum(int[] arr,k){
    if(arr.length<1||arr.length<k)
        return -1;
    //也就是找到索引为k-1的数是否为确定值
    return getIsKNum(int []arr,k-1,0,arr.length-1);
}

private int getKthNum(int[] arr,int index,int start,int end){
   	int p[]=partition(arr,start,end);
    if(p[0]>index)
        return getKthNum(arr,index,start,p[0]-1);
    else if(p[1]<index)
        return getKthNum(arr,index,p[1]+1,end);
    else
        //返回的是值 而非索引
        return arr[p[1]];
}
```





```java

```

