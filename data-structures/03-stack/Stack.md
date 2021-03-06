# 栈

### 一，什么是栈

栗子：叠盘子

- 从上往下一个一个一次取
- 后进后出
- 先进先出
- 不能从中间任意抽取

### 二，特点

- 只允许在一端插入和删除数据
- 后进先出
- 先进后出

### 三，如何实现栈

- 顺序栈——数组实现

  ```c++
  typeof int Position;
  typeof struct SNode* PtrToSNode;

  struct SNode{
      // 存储元素的数组
      ElementType* Data;
      // 栈顶指针
      Position Top;
      // 最大容量
      int MaxSize;
  }

  typeof PtrToSNode Stack;

  Stack CreateStack(int MaxSize){
      Stack S=(Stack)malloc(sizeof(stuct SNode));
      S->Data=(ElementType*)malloc(MaxSize*sizeof(ElementType));
      S->Top=-1;
      S->MaxSize=MaxSize;
      return S;
  }

  //isFull
  bool IsFull(Stack S){
      return(S->Top==S->MaxSize)
  }

  //push
  bool Push(Stack S,ElementType X){
      if(IsFull(S)){
          printf("is Full");
          return false;
      }
      else{
          S->Data[++(S->Top)]=X;
          return true;
      }
  }

  //isEmpty
  bool IsEmpty(Stack S){
      return(S->Top==-1);
  }

  //pop
  ElementType Pop(Stack S){
      if(IsEmpty(S)){
          printf("is empty");
          return ERROR;
      }
      else{
          return(S->Data[(S->top])--];
      }
  }
  ```

一个数组实现两个链表

TODO

- 链式栈——链表实现

  ```c++
  typedef struct SNode*PtrToNode;
  struct SNode{
      ElementType Data;
      PtrToSNode Next;
  }
  typedef PtrToSNode Stack;

  //带头结点的链栈操作

  Stack CreateStack(){
      Stack S;//头结点

      S=malloc(sizeof(stuct SNode));
      S->Next=NULL;
      return S;
  }

  bool IsEmpty(Stack S){
      return (S->Next==NULL)
  }

  bool Push(Stack S,Element X){
      PtrToSNode TempCell;
      TempCell=(PtrToSNode)malloc(sizeof(struct SNode));
      TempCell->Data=X;
      TempCell->next=S->next;
      S->next=TempCell;
      return true;
  }

  ElementType Pop(Stack S){
      PrrToSNode FisrtCell;
      ElementType TopElem;

      if(IsEmpty(S)){
          printf("is empty");
          return ERROR;
      }else{
          FirstCell=S->next;
          TopElem=FistCell->Data;
          S->next=FistCell->Next;
          free(FirstCell);
          return TopElem;
      }
  }
  ```

#### 四，支持动态扩容的顺序栈

- 底层依赖一个支持动态扩容的数组
- 出栈操作——时间复杂度 O(1)
- 入栈操作——
  - 栈中有空闲空间——最好时间复杂度 O(1)
  - 栈中空间不够时，重新申请空间数据搬移——最坏时间复杂度 O(n)
  - 平均时间复杂度（均摊时间复杂度）
- 假设
  - 栈空间不够时，申请原来大小两倍的数组
  - 只有入栈操作
  - 不涉及内存搬移的入栈操作为 simple-push 操作，时间复杂度为 O(1)
  - 如果当前栈大小为 K，并且已满，当再有新的数据要入栈时，就需要重新申请 2 倍大小的内存，并且做 K 个数据的搬移操作，然后再入栈
  - 接下来的 K-1 次入栈操作，都不需要再重新申请内存和搬移数据，所以这 K-1 次入栈操作都只需要一个 simple-push 操作就可以完成
  - 这 K 次入栈操作，总共涉及了 K 个数据的搬移，以及 K 次 simple-push 操作
  - 将 K 个数据搬移均摊到 K 次入栈操作，那每个入栈操作只需要一个数据搬移和一个 simple-push 操作
  - 以此类推，入栈操作的均摊时间复杂度就为 O(1)
  - 所以把耗时多的入栈操作的时间均摊到其他入栈操作上，平均情况下的耗时就接近 O(1)

### 五，栈在函数调用中的应用——**函数调用栈**

操作系统给每个线程分配了一块独立的内存空间，这块内存被组织成“栈”这种结构, 用来存储函数调用时的临时变量

### 六，栈在表达式求值中的应用——**后缀表达式**

加减乘除四则运算，**两个栈**来实现的

- 保存操作数的栈
- 保存运算符的栈

实现：

- 从左向右遍历表达式，当遇到数字，直接压入操作数栈；
- 当遇到运算符，就与运算符栈的栈顶元素进行比较
- 如果比运算符栈顶元素的优先级高，就将当前运算符压入栈；
- 如果比运算符栈顶元素的优先级低或者相同，从运算符栈中取栈顶运算符，从操作数栈的栈顶取 2 个操作数，然后进行计算，再把计算完的结果压入操作数栈，继续比较

### 七，栈在括号匹配中的应用

实现：

- 用栈来保存未匹配的左括号，从左到右依次扫描字符串。
- 当扫描到左括号时，则将其压入栈中；
- 当扫描到右括号时，从栈顶取出一个左括号。
- 如果能够匹配，比如“(”跟“)”匹配，“[”跟“]”匹配，“{”跟“}”匹配，则继续扫描剩下的字符串。
- 如果扫描的过程中，遇到不能配对的右括号，或者栈中没有数据，
- 则说明为非法格式
- 当所有的括号都扫描完成之后，如果栈为空，则说明字符串为合法格式；否则，说明有未匹配的左括号，为非法格式

### 八，如何实现浏览器的前进后退功能？

实现：

- 使用两个栈，X 和 Y，
- 把首次浏览的页面依次压入栈 X，
- 当点击后退按钮时，再依次从栈 X 中出栈，并将出栈的数据依次放入栈 Y
- 当我们点击前进按钮时，我们依次从栈 Y 中取出数据，放入栈 X 中
- 当栈 X 中没有数据时，那就说明没有页面可以继续后退浏览了
- 当栈 Y 中没有数据，那就说明没有页面可以点击前进按钮浏览了
