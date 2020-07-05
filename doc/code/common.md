
# 各种类型初始化
```java
    String[] strarr = new String[]{"cat", "bat", "hat", "tree"};
    int[] intarr = new int[]{1,0,0,1,0,0,1,0};
    char[] chararr=new char[]{'A','A','B','B','C','C','D','D'};
    int[][] board = new int[][]{
            {8,0},{4,4},{8,1},{5,0},{6,1},{5,2}
    };
    String str = "this problem is an easy problem";
    List<String> list=new ArrayList();
    list.add("xxx");
    list.add("yyy");
    list.add("zzz");
    List<String> name = Arrays.asList("xxx","yyy","zzz");
    List<String> list = new ArrayList<String>() {{
        add("a");
        add("b");
    }};
    Map<String, Object> map = new HashMap<>() {{
        put("key", "value");
        put("key2", "value2");
    }};
```

# 排序
## 冒泡排序
算法描述
- 比较相邻的元素。如果第一个比第二个大，就交换它们两个；
- 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对，这样在最后的元素应该会是最大的数；
- 针对所有的元素重复以上的步骤，除了最后一个；
- 重复步骤1~3，直到排序完成。
最佳情况：T(n) = O(n) 最差情况：T(n) = O(n2) 平均情况：T(n) = O(n2)
```java
    public static void select(int[] arr){
        for (int i=0; i<arr.length-1; i++)
        {
            for (int j=0; j<arr.length-1-i;j++)
            {
                if (arr[j]>arr[j+1])
                {
                    int tmp=arr[j];
                    arr[j]=arr[j+1];
                    arr[j+1]=tmp;
                }
            }
        }
    }
```
## 插入排序
```java
    public static void insertSort(int[] a) {
            int i, j, insertNote;// 要插入的数据
            for (i = 1; i < a.length; i++) {// 从数组的第二个元素开始循环将数组中的元素插入
                insertNote = a[i];// 设置数组中的第2个元素为第一次循环要插入的数据
                j = i - 1;
                while (j >= 0 && insertNote < a[j]) {
                    a[j + 1] = a[j];// 如果要插入的元素小于第j个元素,就将第j个元素向后移动
                    j--;
                }
                a[j + 1] = insertNote;// 直到要插入的元素不小于第j个元素,将insertNote插入到数组中
            }
        }
```
## 选择排序
表现最稳定的排序算法之一，因为无论什么数据进去都是O(n2)的时间复杂度，所以用到它的时候，数据规模越小越好。唯一的好处可能就是不占用额外的内存空间了吧。理论上讲，选择排序可能也是平时排序一般人想到的最多的排序方法了吧。

选择排序(Selection-sort)是一种简单直观的排序算法。它的工作原理：首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕。

```java
    public static void selectSort(int[] arr){
            for (int i = 0; i < arr.length; i++) {
                int tmp = arr[i];
                int index=i;
                for (int j = i+1; j < arr.length; j++) {
                    if (arr[j]<tmp) {
                        tmp=arr[j];
                        index=j;
                    }
                }
                if (index!=i) {
                    //交换
                    arr[i]=arr[index]; 
                    arr[index]=tmp;
                }
            }
        }
```

## 快速排序
通过一趟排序将待排记录分隔成独立的两部分，其中一部分记录的关键字均比另一部分的关键字小，则可分别对这两部分记录继续进行排序，以达到整个序列有序。
最佳情况：T(n) = O(nlogn) 最差情况：T(n) = O(n2) 平均情况：T(n) = O(nlogn)
```java
public static void quickSort(int[] array) {
	quickSort(array, 0, array.length - 1);
}


private static void quickSort(int[] array, int left, int right) {
	if (array == null || left >= right || array.length <= 1) {
		return;
	}
	int mid = partition(array, left, right);
	quickSort(array, left, mid);
	quickSort(array, mid + 1, right);
}


private static int partition(int[] array, int left, int right) {
	int temp = array[left];
	while (right > left) {
		// 先判断基准数和后面的数依次比较
		while (temp <= array[right] && left < right) {
			--right;
		}
		// 当基准数大于了 arr[left]，则填坑
		if (left < right) {
			array[left] = array[right];
			++left;
		}
		// 现在是 arr[right] 需要填坑了
		while (temp >= array[left] && left < right) {
			++left;
		}
		if (left < right) {
			array[right] = array[left];
			--right;
		}
	}
	array[left] = temp;
	return left;
}
```

# 查找
## 二分法查找
二分查找也称折半查找（Binary Search），它是一种效率较高的查找方法，前提是数据结构必须先排好序，时间复杂度可以表示O(h)=O(log2n)，以2为底，n的对数。其缺点是要求待查表为有序表，且插入删除困难。
```java
public static int search(int[] arr, int key){
        int start=0;
        int end=arr.length-1;
        while (start<=end)
        {
            int mid = (end+start)/2;
            if (key<arr[mid])
            {
                end=mid-1;
            }else if (key>arr[mid]){
                start=mid+1;
            }else {
                return mid;
            }
        }
        return -1;
    }
```

## 深度优先
```java
public void depthOrderTraversal(TreeNode root) {
        if (root == null) {
            System.out.println("empty tree");
            return;
        }
        ArrayDeque<TreeNode> stack = new ArrayDeque<TreeNode>();
        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
            System.out.print(node.val + "    ");
            if (node.right != null) {
                stack.push(node.right);
            }
            if (node.left != null) {
                stack.push(node.left);
            }
        }
        System.out.print("\n");
    }
```

## 广度优先
```java
/**
     * 广度优先遍历
     * 采用非递归实现
     * 需要辅助数据结构：队列
     */
    public void levelOrderTraversal(TreeNode root) {
        if (root == null) {
            System.out.println("empty tree");
            return;
        }
        ArrayDeque<TreeNode> queue = new ArrayDeque<TreeNode>();
        queue.add(root);
        while (!queue.isEmpty()) {
            TreeNode node = queue.remove();
            System.out.print(node.val + "    ");
            if (node.left != null) {
                queue.add(node.left);
            }
            if (node.right != null) {
                queue.add(node.right);
            }
        }
        System.out.print("\n");
    }
```

# 去重
   ```java
   public static void searchRepeatIndex(int[] arr){
           for (int i = 0; i < arr.length-1; i++) {
               for (int j = i+1; j < arr.length; j++) {
                   if (arr[i]==arr[j]) {
                       System.out.println("重复元素下标:"+i);
                       break;//去掉这句可以查找重复次数
                   }
               }
           }
       }
   ```

# 二叉树的遍历
## 先序遍历
```java
    /**
     * 递归先序遍历
     * */
    public void preOrderRecursion(TreeNode node){
        if(node==null) //如果结点为空则返回
            return;
        visit(node);//访问根节点
        preOrderRecursion(node.left);//访问左孩子
        preOrderRecursion(node.right);//访问右孩子
    }
    /**
     * 非递归先序遍历二叉树
     * */
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> resultList=new ArrayList<>();
        Stack<TreeNode> treeStack=new Stack<>();
        if(root==null) //如果为空树则返回
            return resultList;
        treeStack.push(root);
        while(!treeStack.isEmpty()){
            TreeNode tempNode=treeStack.pop(); 
            if(tempNode!=null){
                resultList.add(tempNode.val);//访问根节点
                treeStack.push(tempNode.right); //入栈右孩子
                treeStack.push(tempNode.left);//入栈左孩子
            }
        }
        return resultList;
    }
```

## 中序遍历
```java
    /**
    * 递归中序遍历
    * */
    public void preOrderRecursion(TreeNode node){
      if(node==null) //如果结点为空则返回
          return;
      preOrderRecursion(node.left);//访问左孩子
      visit(node);//访问根节点
      preOrderRecursion(node.right);//访问右孩子
    }
    /**
    * 非递归中序遍历
    * */
    public List<Integer> inorderTraversalNonCur(TreeNode root) {
      List<Integer> visitedList=new ArrayList<>();
      Map<TreeNode,Integer> visitedNodeMap=new HashMap<>();//保存已访问的节点
      Stack<TreeNode> toBeVisitedNodes=new Stack<>();//待访问的节点
      if(root==null)
          return visitedList;
      toBeVisitedNodes.push(root);
      while(!toBeVisitedNodes.isEmpty()){
          TreeNode tempNode=toBeVisitedNodes.peek(); //注意这里是peek而不是pop
          while(tempNode.left!=null){ //如果该节点的左节点还未被访问，则需先访问其左节点
              if(visitedNodeMap.get(tempNode.left)!=null) //该节点已经被访问（不存在某个节点已被访问但其左节点还未被访问的情况）
                  break;
              toBeVisitedNodes.push(tempNode.left);
              tempNode=tempNode.left;
          }
          tempNode=toBeVisitedNodes.pop();//访问节点
          visitedList.add(tempNode.val);
          visitedNodeMap.put(tempNode, 1);//将节点加入已访问map
          if(tempNode.right!=null) //将右结点入栈
              toBeVisitedNodes.push(tempNode.right);
      }
      return visitedList;
    }
```


## 后序遍历
```java
    /**
    * 递归中序遍历
    * */
    public void preOrderRecursion(TreeNode node){
        if(node==null) //如果结点为空则返回
          return;
        preOrderRecursion(node.left);//访问左孩子
        preOrderRecursion(node.right);//访问右孩子
        visit(node);//访问根节点
    }
    /**
    * 非递归后序遍历
    * */
    public List<Integer> postOrderNonCur(TreeNode root){
        List<Integer> resultList=new ArrayList<>();
        if(root==null)
           return resultList;
        Map<TreeNode,Integer> visitedMap=new HashMap<>();
        Stack<TreeNode> toBeVisitedStack=new Stack<>();
        toBeVisitedStack.push(root);
        while(!toBeVisitedStack.isEmpty()){
           TreeNode tempNode=toBeVisitedStack.peek(); //注意这里是peek而不是pop
           if(tempNode.left==null && tempNode.right==null){ //如果没有左右孩子则访问
               resultList.add(tempNode.val);
               visitedMap.put(tempNode, 1);
               toBeVisitedStack.pop();
               continue;
           }else if(!((tempNode.left!=null&&visitedMap.get(tempNode.left)==null )|| (tempNode.right!=null && visitedMap.get(tempNode.right)==null))){
               //如果节点的左右孩子均已被访问            
               resultList.add(tempNode.val);
               toBeVisitedStack.pop();
               visitedMap.put(tempNode, 1);
               continue;
           }
           if(tempNode.left!=null){
               while(tempNode.left!=null && visitedMap.get(tempNode.left)==null){//左孩子没有被访问
                   toBeVisitedStack.push(tempNode.left);
                   tempNode=tempNode.left;
               }
           }
           if(tempNode.right!=null){
               if(visitedMap.get(tempNode.right)==null){//右孩子没有被访问
                   toBeVisitedStack.push(tempNode.right);
               }               
           }
        }
        return resultList;
    }
```

一致性Hash算法
概述
一致性Hash是一种特殊的Hash算法，由于其均衡性、持久性的映射特点，被广泛的应用于负载均衡领域和分布式存储，如nginx和memcached都采用了一致性Hash来作为集群负载均衡的方案。

普通的Hash函数最大的作用是散列，或者说是将一系列在形式上具有相似性质的数据，打散成随机的、均匀分布的数据。不难发现，这样的Hash只要集群的数量N发生变化，之前的所有Hash映射就会全部失效。如果集群中的每个机器提供的服务没有差别，倒不会产生什么影响，但对于分布式缓存这样的系统而言，映射全部失效就意味着之前的缓存全部失效，后果将会是灾难性的。一致性Hash通过构建环状的Hash空间代替线性Hash空间的方法解决了这个问题。

良好的分布式cahce系统中的一致性hash算法应该满足以下几个方面：
- 平衡性(Balance)
平衡性是指哈希的结果能够尽可能分布到所有的缓冲中去，这样可以使得所有的缓冲空间都得到利用。很多哈希算法都能够满足这一条件。
- 单调性(Monotonicity)
单调性是指如果已经有一些内容通过哈希分派到了相应的缓冲中，又有新的缓冲区加入到系统中，那么哈希的结果应能够保证原有已分配的内容可以被映射到新的缓冲区中去，而不会被映射到旧的缓冲集合中的其他缓冲区。
- 分散性(Spread)
在分布式环境中，终端有可能看不到所有的缓冲，而是只能看到其中的一部分。当终端希望通过哈希过程将内容映射到缓冲上时，由于不同终端所见的缓冲范围有可能不同，从而导致哈希的结果不一致，最终的结果是相同的内容被不同的终端映射到不同的缓冲区中。这种情况显然是应该避免的，因为它导致相同内容被存储到不同缓冲中去，降低了系统存储的效率。分散性的定义就是上述情况发生的严重程度。好的哈希算法应能够尽量避免不一致的情况发生，也就是尽量降低分散性。
- 负载(Load)
负载问题实际上是从另一个角度看待分散性问题。既然不同的终端可能将相同的内容映射到不同的缓冲区中，那么对于一个特定的缓冲区而言，也可能被不同的用户映射为不同的内容。与分散性一样，这种情况也是应当避免的，因此好的哈希算法应能够尽量降低缓冲的负荷。
- 平滑性(Smoothness)
平滑性是指缓存服务器的数目平滑改变和缓存对象的平滑改变是一致的。

一致性Hash算法原理

简单来说，一致性哈希将整个哈希值空间组织成一个虚拟的圆环，如假设某哈希函数H的值空间为0-232-1（即哈希值是一个32位无符号整形），整个哈希空间环如下：整个空间按顺时针方向组织。0和232-1在零点中方向重合。


下一步将各个服务器使用Hash进行一次哈希，具体可以选择服务器的ip或主机名作为关键字进行哈希，这样每台机器就能确定其在哈希环上的位置，这里假设将上文中四台服务器使用ip地址哈希后在环空间的位置如下：

接下来使用如下算法定位数据访问到相应服务器：将数据key使用相同的函数Hash计算出哈希值，并确定此数据在环上的位置，从此位置沿环顺时针“行走”，第一台遇到的服务器就是其应该定位到的服务器。

例如我们有Object A、Object B、Object C、Object D四个数据对象，经过哈希计算后，在环空间上的位置如下：

下面分析一致性哈希算法的容错性和可扩展性。现假设Node C不幸宕机，可以看到此时对象A、B、D不会受到影响，只有C对象被重定位到Node D。一般的，在一致性哈希算法中，如果一台服务器不可用，则受影响的数据仅仅是此服务器到其环空间中前一台服务器（即沿着逆时针方向行走遇到的第一台服务器）之间数据，其它不会受到影响。


一致性哈希算法对于节点的增减都只需重定位环空间中的一小部分数据，具有较好的容错性和可扩展性。
```java
public class ConsistentHash<T> {

    /**
     * 节点的复制因子,实际节点个数 * numberOfReplicas = 虚拟节点个数
     */
    private final int numberOfReplicas;
    /**
     * 存储虚拟节点的hash值到真实节点的映射
     */
    private final SortedMap<Integer, T> circle = new TreeMap<Integer, T>();

    public ConsistentHash(int numberOfReplicas, Collection<T> nodes) {
        this.numberOfReplicas = numberOfReplicas;
        for (T node : nodes) {
            add(node);
        }
    }

    public void add(T node) {
        for (int i = 0; i < numberOfReplicas; i++) {
            // 对于一个实际机器节点 node, 对应 numberOfReplicas 个虚拟节点
            /*
             * 不同的虚拟节点(i不同)有不同的hash值,但都对应同一个实际机器node
             * 虚拟node一般是均衡分布在环上的,数据存储在顺时针方向的虚拟node上
             */
            String nodestr = node.toString() + i;
            int hashcode = nodestr.hashCode();
            System.out.println("hashcode:" + hashcode);
            circle.put(hashcode, node);

        }
    }

    public void remove(T node) {
        for (int i = 0; i < numberOfReplicas; i++) {
            circle.remove((node.toString() + i).hashCode());
        }
    }


    /**
     * 获得一个最近的顺时针节点,根据给定的key 取Hash
     * 然后再取得顺时针方向上最近的一个虚拟节点对应的实际节点
     * 再从实际节点中取得 数据
     *
     * @param key
     * @return
     */
    public T get(Object key) {
        if (circle.isEmpty()) {
            return null;
        }
        // node 用String来表示,获得node在哈希环中的hashCode
        int hash = key.hashCode();
        System.out.println("hashcode----->:" + hash);
        //数据映射在两台虚拟机器所在环之间,就需要按顺时针方向寻找机器
        if (!circle.containsKey(hash)) {
            SortedMap<Integer, T> tailMap = circle.tailMap(hash);
            hash = tailMap.isEmpty() ? circle.firstKey() : tailMap.firstKey();
        }
        return circle.get(hash);
    }

    public long getSize() {
        return circle.size();
    }

    /**
     * 查看表示整个哈希环中各个虚拟节点位置
     */
    public void testBalance() {
        //获得TreeMap中所有的Key
        Set<Integer> sets = circle.keySet();
        //将获得的Key集合排序
        SortedSet<Integer> sortedSets = new TreeSet<Integer>(sets);
        for (Integer hashCode : sortedSets) {
            System.out.println(hashCode);
        }

        System.out.println("----each location 's distance are follows: ----");
        /*
         * 查看相邻两个hashCode的差值
         */
        Iterator<Integer> it = sortedSets.iterator();
        Iterator<Integer> it2 = sortedSets.iterator();
        if (it2.hasNext()) {
            it2.next();
        }
        long keyPre, keyAfter;
        while (it.hasNext() && it2.hasNext()) {
            keyPre = it.next();
            keyAfter = it2.next();
            System.out.println(keyAfter - keyPre);
        }
    }

    public static void main(String[] args) {
        Set<String> nodes = new HashSet<String>();
        nodes.add("A");
        nodes.add("B");
        nodes.add("C");

        ConsistentHash<String> consistentHash = new ConsistentHash<String>(2, nodes);
        consistentHash.add("D");

        System.out.println("hash circle size: " + consistentHash.getSize());
        System.out.println("location of each node are follows: ");
        consistentHash.testBalance();

        String node = consistentHash.get("apple");
        System.out.println("node----------->:" + node);
    }

}
```