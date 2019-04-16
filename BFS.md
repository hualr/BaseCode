# 宽度优先遍历

[TOC]

## 宽度优先遍历的原理

1. 宽度优先遍历实际上是进行波澜状遍历
2. 按照先进去的先出来,我们需要一个队列
3. 按照一个数字只会对他进行深度遍历一次,我们需要一个set记录这个值是否已经参与遍历



### 具体想法

1. 一个节点下面存在着很多个子节点,因此首先我们将根节点放入队列
2. 其次,我们将根节点从队列中移出,然后将该节点下的子节点放入
3. 继续这种操作



### 具体代码

```java
public void bfs(Node node){
    if(node==null)
        return;
    Queue<Node> queue=new LinkedList<>();
    //记录节点是否存在
    HashSet<Node> set=new HashSet<>();
    queue.add(node);
    set.add(node);
    while(!queue.isEmpty()){
    	Node cur=queue.poll();
        //do cur
        for(Node next:cur.nexts){
            if(!set.contains(next)){
				set.add(next);
                  3queue.add(next);
            }
        }
    }
}
```

