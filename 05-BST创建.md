# 二分搜索数(BinarySearchTree)
## 定义及特点
- 二分搜索树也是二叉树
- 二分搜索树每个节点的左子树都小于节点,右子树都大于其节点,且每个节点的字数都满足,即左孩子 < 当前节点 < 右孩子
## 创建及实现
- 以Node作为链表的基础存储结构
### 基础结构的实现
    通过上面的分析，我们可知，如果我们要实现一个二分搜索树，我们需要我们的节点有左右两个孩子节点。
>   from pprint import pformat  # (美化输出)                                                                                                                                                                                                            
>   class Node:   
>
>       def __init__(self, value, parent):                                                                                                                     
>           self.value = value        # 相当于self.data =data                                                                                    
>           self.parent = parent      # 定义父节点在哪里                                                                                          
>           self.left = None          #左节点定义为空                                                                              
>           self.right = None         #右节点定义为空                                                                        
>   
>       def __repr__(self):       #定义方法                                                                                                                                         
>           if self.left is None and self.right is None:                                                
>               return str(self.value)                                                                                  
>           return pformat({"%s" % (self.value): (                                                                                                  
>               self.left, self.right)}, indent=1)                    
>           # 整体框架:节点:(节点的左孩子,节点的右孩子)                                                                                       

>   class BinarySearchTree:                     #创建二分搜索树类                                                                                               
>
>       def __init__(self, root=None):          #定义根节点                                                                                                                               
>           self.root = root                                                                                  
>                                                                                                                                                 
>       def __str__(self):                                                            
>           return str(self.root)               #以字符串形式返回                                                               
>                                                                                                                                                   
>       def is_empty(self):                    #判断二叉树是否为空                                                                                 
>           if self.root is None:                                                                                 
>               return True                                                                                           
>           else:                                                                                   
>               return False                                                                        

### 插入节点
-   首先找到要添加的元素需要放到什么位置
-   如果小于节点,那么继续往左叶子走,如果为空,那么插入,停止,否则再重复上述比较步骤.
-   如果大于节点,那么继续往右叶子走,如果为空,那么插入,停止,否则再重复上述比较步骤.
>     def __insert(self, value):                                                                                                                                                                                                                                                                           
>        new_node = Node(value, None)                                                                         
>        if self.is_empty():                                                                              
>            self.root = new_node                                                                                                                                     
>        else:                                                                                                                                   
>            parent_node = self.root         # 从根节点开始查找位置                                                       
>            while True:                     # 循环查找                                     
>                if value < parent_node.value:         # 如果插入值小于父结点的值                                                                                                               
>                    if parent_node.left is None:      # 如果父节点的左结点是空                                                             
>                        parent_node.left = new_node   # 把插入值设为左节点                                                   
>                        break                                                      
>                    else:                                                                  
>                        parent_node = parent_node.left             # 如果父节点左侧不空,那么判断结点下移                                                
>                elif value >= parent_node.value:                   # 若插入值大于父结点的值                                                  
>                    if parent_node.right is None:                  # 且父结点右节点是空                                                      
>                        parent_node.right = new_node               # 把插入值设为右节点                                          
>                        break                                                                                
>                    else:                                                                                          
>                        parent_node = parent_node.right            # 如果父节点右侧不空,那么判断结点下移                                                    
>            new_node.parent = parent_node                          # 指定一下新结点的父结点                                                            
-    调用__insert(self, value)函数
>     def insert(self, *args):                                      # *args:不定长参数                                                                                                                                                               
>        for value in args:                                                         
>            self.__insert(value)                                                       
>        return  # self.root                                                                                                                       

### 查找节点          
-   首先判断要找的元素是否为根节点
-   如果不是根节点且根节点不为空,则依次判断是否小于父节点的值,如果不是则向左下移,反之则向右下移动.
>     def search(self, value):                                                                   
>         if self.is_empty():                                             
>           raise IndexError('空树')                                                                                                              
>         else:                                                                                                                                                              
>           node = self.root                                     #定义根节点                                                                          
>           while node and node.value != value:                  #定义条件                                                   
>               if value < node.value:                                                                                                          
>                   node = node.left                                                                                                                                                          
>               elif value >= node.value:                               
>                   node = node.right                                                                                                    
>           #   node =node.left if value<node.value else node.right                                                                                     
>           # print('查找树为:%s' % node.value)                                                                                             
>           print(node)                                                                                                                       
>           return node                                                                                                                                


### *移除某个节点*       
####  定义移除函数(需要调用其他函数)
-   首先通过调用上面的查找函数找到需要移除的节点
-   其次分别列出四种情况:
-   1.不存在子节点;2.只存在左叶子;3.只存在右叶子;4.左右叶子都存在.调用函数__reassign_nodes()和get_max()操作.
>      def remove(self, value: int):                                                                                                        
>        search_node = self.search(value)                                                                       
>        if search_node is not None:                                                                            
>            if search_node.left is None and search_node.right is None:                                                               
>                self.__reassign_nodes(search_node, None)                                                                               
>            elif search_node.left is None:                                                                                     
>                self.__reassign_nodes(search_node, search_node.right)                                                              
>            elif search_node.right is None:                                                                                              
>                self.__reassign_nodes(search_node, search_node.left)                                                                                             
>            else:                                                                                                  
>                temp = self.get_max(search_node.left)                                                                                        
>                self.remove(temp.value)                                                                                        
>                search_node.value = temp.value                                                                                             

-   定义函数,进行替换操作,new_children是当前节点的孩子,要删除节点的定义为node,要补进的是new_children
-   首先将new_children的父节点指向节点node的父节点
-   其次将节点的父节点的孩子指向new_children 
>     def __reassign_nodes(self, node, new_children):                                                                                      
>        if new_children is not None:               #先给新的孩子指定父节点                                                                               
>            new_children.parent=node.parent        #将孩子节点的父节点指向节点的父节点                                                                                   
>        if node.parent is not None:                #指定当前节点父节点的孩子为new_children                                                     
>                node.parent.right=new_children                                                                               
>            else:                                                                                                                  
>                node.parent.left=new_children                                                                    
>        else:                                                                                                            
>            self.root=new_children                                                                   

-   定义父节点是否存在右孩子函数                                                                                    
>     def is_right(self,node):                                                                           
>        return node == node.parent.right                                                         

-   定义最大值函数
>     def get_max(self, node):                                                                                                                                                          
>        if node is None:                                                                                       
>            node=self.root                                                                   
>        if not self.is_empty():   #如果不是空BST,就找给定子树的最大节点                                                                                                
>            while node.right is not None:                                                                          
>                node=node.right                                                                                              
>        return node                                                                                      

##  递归Recursion(使用深度优先排序)的三种排序方法
-    必要条件:1.结束条件,2.关系式
-   核心思想:每一次递归，整体问题都要比原来减小，并且递归到一定层次时，要能直接给出结果。
-   在递归的调用过程中，系统会为每一层的返回值或局部变量开辟新的栈进行存储。
-   递归的调用过程可以看做压栈和弹栈的过程
###  前序   
-   前序遍历是按照：父节点，左孩子，右孩子的顺序对节点进行遍历
-   self, left, right     eg:  1, 2, 4, 5, 3, 6, 7                                           
>     def pre_order(self,node):                                                                                    
>        if node is None:                                                                                   
>            return                                                                               
>        print(node.value, end=' ')                                                                               
>        self.pre_order(node.left)                                                                                                  
>        self.pre_order(node.right)                                                           
>        return node                                                                                          

### 中序
-   left,self,right  eg:  4, 2, 5, 1, 6, 3, 7                                                                    
>     def in_order(self, node):                                                                
>        if node is None:                                                                               
>            return                                                                                       
>        self.in_order(node.left)                                                               
>        print(node.value, end=' ')                                                                         
>        self.in_order(node.right)                                                                                    
>        return node                                                                                                    

### 后序
-   left, right, self  eg:  4, 5, 2, 6, 7, 3, 1                                                     
>     def pro_order(self, node):                                                                                                 
>        if node is None:                                                                         
>            return                                                           
>        self.pro_order(node.left)                                                              
>        self.pro_order(node.right)                                                                         
>        print(node.value, end=' ')                                                         
>        return node                                                                                      









