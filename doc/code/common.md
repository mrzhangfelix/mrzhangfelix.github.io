
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

# 查找
## 二分法查找
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

