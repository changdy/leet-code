# 146. LRU缓存机制
## 初版-- 大意失荆州

这道题在掘金上面看过 , 也知道大概是使用 HashMap和链表实现 , 所以刚开始不以为意  .匆匆忙忙使用HashMap以及LinkedList实现了, 初始代码如下:

```java
class LRUCache {
    int count;
    HashMap<Integer, Integer> map;
    LinkedList<Integer> linkedList = new LinkedList<>();
    public LRUCache(int capacity) {
        count = capacity;
        map = new HashMap<>(capacity);
    }
    public int get(int key) {
        Integer integer = map.get(key);
        if (integer == null) {
            return -1;
        }
        Integer keyInteger = key;
        linkedList.remove(keyInteger);
        linkedList.add(keyInteger);
        return integer;
    }
    public void put(int key, int value) {
        Integer preValue = map.put(key, value);
        if (preValue == null) {
            if (count > 0) {
                count--;
            } else {
                map.remove(linkedList.removeFirst());
            }
        } else {
            linkedList.remove(Integer.valueOf(key));
        }
        linkedList.add(key);
    }
}
```

代码比较简单 , key和value使用 HashMap进行存储 ,然后使用 LinkedList 存一下 key的顺序 , 提交之后 , 显示超过 5%  . 意识到哪里肯定有问题 .

## get方法优化

想了想 , 每次get操作LinkedList都要诺一次 , 不如 get的时候增加这个值的权重 , 然后put的时候删除最小权重 , 此时引入了一个内部类

```java
class LRUCache {
    static class Pair {
        int value;
        int index;

        public Pair(int value, int index) {
            this.value = value;
            this.index = index;
        }
    }

    int count;
    int index = 0;
    HashMap<Integer, Pair> map;
    LinkedList<Integer> linkedList = new LinkedList<>();
    public LRUCache(int capacity) {
        count = capacity;
        map = new HashMap<>(capacity);
    }
    public int get(int key) {
        Pair pair = map.get(key);
        if (pair == null) {
            return -1;
        }
        pair.index = index++;
        return pair.value;
    }

    public void put(int key, int value) {
        Pair put = map.put(key, new Pair(value, index++));
        if (put == null) {
            if (count > 0) {
                count--;
            } else {
                Set<Map.Entry<Integer, Pair>> entries = map.entrySet();
                int minValue = Integer.MAX_VALUE, minIndex = 0;
                for (Map.Entry<Integer, Pair> entry : entries) {
                    if (entry.getValue().index < minValue) {
                        minValue = entry.getValue().index;
                        minIndex = entry.getKey();
                    }
                }
                map.remove(minIndex);
            }
        }
    }
}
```

这样的话 get操作能够实现O(1) 但是, 对于put 复杂度还是比较高 , 最终是超过了 11%

## 看答案后秒懂

这个时候有点沮丧 , 看了下答案 , 原来双链表要自己实现  , 而我一直都是使用JDK自带的 , 所以无法实现put O(1)  , 最终代码如下:

```java
class LRUCache {
    static class DLinkedNode {
        int value;
        int key;
        DLinkedNode pre;
        DLinkedNode next;

        public DLinkedNode(int value, int key) {
            this.value = value;
            this.key = key;
        }
    }

    int capacity;
    HashMap<Integer, DLinkedNode> map;
    DLinkedNode first = new DLinkedNode(0, 0);
    DLinkedNode last = new DLinkedNode(-1, -1);
    public LRUCache(int capacity) {
        this.capacity = capacity;
        map = new HashMap<>(capacity);
        first.next = last;
        last.pre = first;
    }

    public int get(int key) {
        DLinkedNode dLinkedNode = map.get(key);
        if (dLinkedNode == null) {
            return -1;
        }
        removeItem(dLinkedNode);
        addToLast(dLinkedNode);
        //System.out.println("get:" + key + "\t" + first);
        return dLinkedNode.value;
    }

    public void put(int key, int value) {
        DLinkedNode dLinkedNode = new DLinkedNode(value, key);
        addToLast(dLinkedNode);
        DLinkedNode put = map.put(key, dLinkedNode);
        if (put != null) {
            removeItem(put);
        } else if (capacity > 0) {
            capacity--;
        } else {
            removeFirst();
        }
        //System.out.println("添加:key" + key + "\t" + first);
    }

    public void addToLast(DLinkedNode dLinkedNode) {
        dLinkedNode.pre = last.pre;
        dLinkedNode.next = last;
        last.pre = dLinkedNode;
        dLinkedNode.pre.next = dLinkedNode;
    }

    public void removeItem(DLinkedNode item) {
        item.pre.next = item.next;
        item.next.pre = item.pre;
    }

    public void removeFirst() {
        DLinkedNode next = first.next;
        removeItem(next);
        map.remove(next.key);
    }
}
```

这样的话 就实现了get和put都是O(1) , 同时注意一下几个坑:

* 队列最好先定义好头节点和尾节点 , 看到评论有人头尾节点没有固定 导致判断非常繁琐(和俄罗斯方块外面再嵌套一层差不多)
* 淘汰节点的时候一定要注意同步删除map , 避免map中仍然遗留数据
* 系统自带的链表功能比较少 , 有额外需求的话, 可以自己定制