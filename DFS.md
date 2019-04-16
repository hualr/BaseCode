# 深度优先遍历

[TOC]

## 深度遍历基础

### 原理分析

1. 深度优先遍历由于是先深度到底,然后返回,因此需要用栈这样的结构
2. 我们依然需要一个set来说明这个节点是否有过遍历
3. 实际上堆栈中总是一条完成的从根节点走来的路

### 具体方法

1.  首先将根路径压入堆栈和set,然后打印出根路径的值
2. 然后将堆栈中的元素pop出来
3. 将这个值的后台遍历一遍,当发现后台的值存在且之前没有遍历,那么放入当前pop值和后代
4. 此时堆栈中是一条完整的从根节点到后代的路
5. 这个后代先pop自己,不存在则不进行其他操作

```java
public void dfs(Node node){
    if(node==null||node.next==null){
        syso(node);
        return;
    }
    Stack <Node> stack=new Stack<>();
    stack.add(node);
    set.add(node);
    syso(node.val);
    while(!stack.isEmpty()){
        Node cur=stack.pop();
        for(Node next:cur.nexts){
            if(!set.contains(next)){
                stack.push(cur);
                stack.push(next);
                set.push(next);
                syso(next);
                break;
            }
        }
    }
}
```



