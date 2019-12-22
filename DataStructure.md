[TOC]

# 数据结构

## 线性表

### 顺序表

```c++
#define MAX_LEN 100
template <typename T>
class SeqList {
    T *seqlist;
    int length;
public:
    /*
    	顺序存储结构，随机存取结构
    	通过一组连续的存储单元 seqList 进行数据存储，length 存储顺序表长度
    	所有传入的下标默认从 1 开始
    */
    SeqList(): length(0) {
        seqlist = new T[MAX_LEN];
    }
    ~SeqList() {
        delete[] seqlist;
    }
    int size() {
        return length;
    }
    bool empty() {
        return length == 0;
    }
    /*
        将 [index-1,size-1] 移动到 [index,size]
        随后用新数据覆盖 [index-1]
        最后将长度 +1
    */
    bool insertItem(int index, T data) {
        if (index < 1 || index > length + 1)
            return false;
        for (int i = length - 1; i >= index - 1; i--)
            seqlist[i + 1] = seqlist[i];
        seqlist[index - 1] = data;
        length++;
        return true;
    }
    /*
        将 [index, size-1] 移动到 [index-1, size-2]
        从而覆盖[index-1]
        最后将长度 -1
    */
    bool deleteItem(int index) {
        if (index < 1 || index > length)
            return false;
        for (int i = index; i < length; i++)
            seqlist[i - 1] = seqlist[i];
        length--;
        return true;
    }
    /*
        将 [index-1, size-1] 移动到 [index-1+num, size-1+num]
        随后用新数据覆盖 [index-1,index-1+num-1]
        最后将长度 +num
    */
    bool insertItems(int index, int num, T *data) {
        if (index < 1 || index > length + 1)
            return false;
        for (int i = length - 1; i >= index - 1; i--)
            seqlist[i + num] = seqlist[i];
        for (int i = 0; i < num; i++)
            seqlist[index - 1 + i] = data[i];
        length += num;
        return true;
    }
    /*
        将 [index-1+num,size-1] 移动到 [index-1,size-1-num]
        从而覆盖 [index-1,index-1+num-1]
        最后将长度 -num
    */
    bool deleteItems(int index, int num) {
        if (index < 1 || index - 1 + num > length || num < 1)
            return false;
        for (int i = index - 1 + num; i < length; i++)
            seqlist[i - num] = seqlist[i];
        length -= num;
        return true;
    }
    void show() {
        for (int i = 0; i < length; i++)
            cout << seqlist[i] << ' ';
        cout << endl;
    }
};
```

### 线性链表

```c++
template <typename T>
struct Node {
    T data;
    Node *next;
    /* data 域存数据，next 域存下一个结点的地址 */
    Node(): next(NULL) {}
};
template <typename T>
class LinkedList {
    Node<T> *head;
    int length;
public:
    /*
    	随机存储结构，顺序存取结构
    	head 不存储数据，length 存储链表长度
    	所有传入的下标默认从 1 开始
    */
    LinkedList(): length(0) {
        head = new Node<T>;
    }
    ~LinkedList() {
        Node<T> *p;
        while (head) {
            p = head->next;
            delete head;
            head = p;
        }
    }
    int size() {
        return length;
    }
    bool empty() {
        return length == 0;
    }
    /*
    	先找到 [index-1] 的上一个结点 p
    	然后将创建的新结点 node 的 next 指向 p 的 next
    	再将 p 的 next 指向 node，并将长度 +1
    */
    bool insertItem(int index, T data) {
        if (index < 1 || index > length + 1)
            return false;
        Node<T> *p = head;
        while (--index > 0)
            p = p->next;
        Node<T> *node = new Node<T>;
        node->data = data;
        node->next = p->next;
        p->next = node;
        length++;
        return true;
    }
    /*
    	先找到 [index-1] 的上一个结点 p
    	然后将 [index-1] 结点存到 trash
    	将 p 的 next 指向 trash 的 next
    	最后删除 trash，并将长度 -1
    */
    bool deleteItem(int index) {
        if (index < 1 || index > length)
            return false;
        Node<T> *p = head;
        while (--index > 0)
            p = p->next;
        Node<T> *trash = p->next;
        p->next = trash->next;
        delete trash;
        length--;
        return true;
    }
    /*
    	先找到 [index-1] 的上一个结点 p，开始循环
    	将创建的新结点 node 的 next 指向 p 的 next
    	再将 p 的 next 指向 node
    	然后更新 p，使其指向 node，进入新循环
    	最后将长度 +num
    */
    bool insertItems(int index, int num, T *data) {
        if (index < 1 || index > length + 1)
            return false;
        Node<T> *node, *p = head;
        while (--index > 0)
            p = p->next;
        for (int i = 0; i < num; i++) {
            node = new Node<T>;
            node->data = data[i];
            node->next = p->next;
            p->next = node;
            p = p->next;
        }
        length += num;
        return true;
    }
    /*
  		找到 [index-1] 的上一个结点 p，开始循环
    	将 p 的下一个结点存到 trash
    	将 p 的 next 指向 trash 的 next
    	删除 trash，进入新循环
    	最后将长度 -num
    */
    bool deleteItems(int index, int num)  {
        if (index < 1 || index - 1 + num > length || num < 1)
            return false;
        Node<T> *trash, *p = head;
        while (--index > 0)
            p = p->next;
        for (int i = 0; i < num; i++) {
            trash = p->next;
            p->next = trash->next;
            delete trash;
        }
        length -= num;
        return true;
    }
    void show() {
        Node<T> *p = head->next;
        while (p != NULL) {
            cout << p->data << ' ';
            p = p->next;
        }
        cout << endl;
    }
};
```

### 循环链表

```c++
template <typename T>
struct Node {
    T data;
    Node *next;
    /* data 域存数据，next 域存下一个结点的地址 */
    Node(): next(NULL) {}
};
template <typename T>
class CircularLinkedList {
    Node<T> *head, *now;
    int length;
public:
    /*
    	随机存储结构，顺序存取结构
    	head 不存储数据，length 存储链表长度
    	所有传入的下标默认从 1 开始
    	通过将最后一个结点的 next 指向 head 能够构成一个环
    	通过setNow(), getNow(), deleteNow() 进行特殊结点的设置、读取和删除
    */
    CircularLinkedList(): length(0) {
        head = new Node<T>;
        head->next = head;
        now = head;
    }
    ~CircularLinkedList() {
        Node<T> *p, *trash = head->next;
        while (trash != head) {
            p = trash->next;
            delete trash;
            trash = p;
        }
    }
    int size() {
        return length;
    }
    bool empty() {
        return length == 0;
    }
    /*
    	先找到 [index-1] 的上一个结点 p
    	然后将创建的新结点 node 的 next 指向 p 的 next
    	再将 p 的 next 指向 node，并将长度 +num
    */
    bool insertItem(int index, T data) {
        if (index < 1 || index > length + 1)
            return false;
        Node<T> *p = head;
        while (--index > 0)
            p = p->next;
        Node<T> *node = new Node<T>;
        node->data = data;
        node->next = p->next;
        p->next = node;
        length++;
        return true;
    }
    /*
    	先找到 [index-1] 的上一个结点 p
    	然后将 [index-1] 结点存到 trash
    	将 p 的 next 指向 trash 的 next
    	最后删除 trash，并将长度 -1
    */
    bool deleteItem(int index) {
        if (index < 1 || index > length)
            return false;
        Node<T> *p = head;
        while (--index > 0)
            p = p->next;
        Node<T> *trash = p->next;
        p->next = trash->next;
        delete trash;
        length--;
        return true;
    }
    /*
    	先找到 [index-1] 的上一个结点 p，开始循环
    	将创建的新结点 node 的 next 指向 p 的 next
    	再将 p 的 next 指向 node
    	然后更新 p，使其指向 node，进入新循环
    	最后将长度 +num
    */
    bool insertItems(int index, int num, T *data) {
        if (index < 1 || index > length + 1)
            return false;
        Node<T> *p = head;
        while (--index > 0)
            p = p->next;
        for (int i = 0; i < num; i++) {
            Node<T> *node = new Node<T>;
            node->data = data[i];
            node->next = p->next;
            p->next = node;
            p = p->next;
        }
        length += num;
        return true;
    }
    /*
  		找到 [index-1] 的上一个结点 p，开始循环
    	将 p 的下一个结点存到 trash
    	将 p 的 next 指向 trash 的 next
    	删除 trash，进入新循环（每次循环 p 并不后移，故不用更新 p）
    	最后将长度 -num
    */
    bool deleteItems(int index, int num) {
        if (index < 1 || index - 1 + num > length || num < 1)
            return false;
        Node<T> *trash, *p = head;
        while (--index > 0)
            p = p->next;
        for (int i = 0; i < num; i++) {
            trash = p->next;
            p->next = trash->next;
            delete trash;
        }
        length -= num;
        return true;
    }
    /* 通过传入下标以设置特殊结点的位置 */
    bool setNow(int index) {
        if (index < 1 || index > length)
            return false;
        Node<T> *p = head;
        while (index --> 0)
            p = p->next;
        now = p;
        return true;
    }
    /* 获取特殊结点的信息 */
    T getNow() {
        return now->data;
    }
    /*
    	删除特殊结点：需要特殊处理最后一个结点，其删除过程不需要特殊处理，但 delete 前需重置 head 和 now
    	删除结点方式：将 now 的后一个结点 trash 的 data 与 next 赋值给 now，随后 delete trash
    	最后将长度 -1
    */
    bool deleteNow()
    {
        if (now == head || length == 0)
            return false;
        Node<T> *trash = now->next;
        now->data = trash->data;
        now->next = trash->next;
        if (trash == head) {
            head = now;
            now = now->next;
        }
        delete trash;
        length--;
        return true;
    }
    /*
    	通过控制次数的循环进行特殊结点的更新
    	若循环到 head 则跳过，若链表无元素则不循环
    	通过预处理 times 进行化简
    */
    bool loop(int times) {
        if (times < 1 || length == 0)
            return false;
        times %= length;
        while (times --> 0) {
            now = now->next;
            if (now == head)
                now = now->next;
        }
        return true;
    }
    void show() {
        Node<T> *p = head->next;
        while (p != head) {
            cout << p->data << ' ';
            p = p->next;
        }
        cout << endl;
    }
};
```

### 双向链表

```c++
template <typename T>
struct Node {
    T data;
    Node *prev, *next;
    /* data 域存数据，next 域存下一个结点的地址，prev 域存上一个结点的地址 */
    Node(): prev(NULL), next(NULL) {}
};
template <typename T>
class DoubleLinkedList {
    Node<T> *head, *tail;
    int length;
public:
    /*
    	随机存储结构，顺序存取结构
    	head 和 tail 不存储数据，length 存储链表长度，tail 作为链表尾的标志
    	head 的 next 指向 tail，tail 的 prev 指向 head
    	所有传入的下标默认从 1 开始
    */
    DoubleLinkedList(): length(0) {
        head = new Node<T>;
        tail = new Node<T>;
        head->next = tail;
        tail->prev = head;
    }
    ~DoubleLinkedList() {
        Node<T> *p;
        while (head) {
            p = head->next;
            delete head;
            head = p;
        }
    }
    int size() {
        return length;
    }
    bool empty() {
        return length == 0;
    }
    /*
    	先找到 [index-1] 的上一个结点 p，针对 index 的大小选择性移动 p
    	然后将创建的新结点 node 的 next 指向 p 的 next
    	将 node 的 next 的 prev 指向 node
    	将 node 的 prev 指向 p
    	最后将 p 的 next 指向 node，并将长度 +1
    */
    bool insertItem(int index, T data) {
        if (index < 1 || index > length + 1)
            return false;
        Node<T> *p;
        if (index <= (length + 1) / 2) {
            p = head;
            while (--index > 0)
                p = p->next;
        } else {
            p = tail;
            index = length - index + 2;
            while (index --> 0)
                p = p->prev;
        }
        Node<T> *node = new Node<T>;
        node->data = data;
        node->next = p->next;
        node->next->prev = node;
        node->prev = p;
        p->next = node;
        length++;
        return true;
    }
    /*
    	先找到 [index-1] 的上一个结点 p，针对 index 的大小选择性移动 p
    	将 [index-1] 结点存到 trash
    	将 p 的 next 指向 trash 的 next
    	将 p 的 next 的 prev 指向 p
    	最后删除 trash，并将长度 -1
    */
    bool deleteItem(int index) {
        if (index < 1 || index > length)
            return false;
        Node<T> *p;
        if (index <= (length + 1) / 2) {
            p = head;
            while (--index > 0)
                p = p->next;
        } else {
            p = tail;
            index = length - index + 2;
            while (index --> 0)
                p = p->prev;
        }
        Node<T> *trash = p->next;
        p->next = trash->next;
        p->next->prev = p;
        delete trash;
        length--;
        return true;
    }
    /*
    	先找到 [index-1] 的上一个结点 p，针对 index 的大小选择性移动 p，开始循环
    	将创建的新结点 node 的 next 指向 p 的 next
    	将 node 的 next 的 prev 指向 node
    	将 node 的 prev 指向 p
    	最后将 p 的 next 指向 node
    	然后更新 p，使其指向 node，进入新循环
    	最后将长度 +num
    */
    bool insertItems(int index, int num, T *data) {
        if (index < 1 || index > length + 1)
            return false;
        Node<T> *p;
        if (index <= (length + 1) / 2) {
            p = head;
            while (--index > 0)
                p = p->next;
        } else {
            p = tail;
            index = length - index + 2;
            while (index --> 0)
                p = p->prev;
        }
        for (int i = 0; i < num; i++) {
            Node<T> *node = new Node<T>;
            node->data = data[i];
            node->next = p->next;
            node->next->prev = node;
            node->prev = p;
            p->next = node;
            p = p->next;
        }
        length += num;
        return true;
    }
    /*
  		找到 [index-1] 的上一个结点 p，针对 index 的大小选择性移动 p，开始循环
    	将 p 的下一个结点存到 trash
    	将 p 的 next 指向 trash 的 next
    	将 p 的 next 的 prev 指向 p
    	删除 trash，进入新循环（每次循环 p 并不后移，故不用更新 p）
    	最后将长度 -num
    */
    bool deleteItems(int index, int num)  {
        if (index < 1 || index - 1 + num > length || num < 1)
            return false;
        Node<T> *trash, *p;
        if (index <= (length + 1) / 2) {
            p = head;
            while (--index > 0)
                p = p->next;
        } else {
            p = tail;
            index = length - index + 2;
            while (index --> 0)
                p = p->prev;
        }
        for (int i = 0; i < num; i++) {
            trash = p->next;
            p->next = trash->next;
            p->next->prev = p;
            delete trash;
        }
        length -= num;
        return true;
    }
    void show() {
        Node<T> *p = head->next;
        while (p != tail) {
            cout << p->data << ' ';
            p = p->next;
        }
        cout << endl;
    }
};
```

## 栈和队列

### 栈

#### 顺序栈

```c++
#define MAX_LEN 100
template <typename T>
class SeqStack {
    T *base, *top;
    int length;
public:
    /*
    	顺序存储结构，随机存取结构
    	通过一组连续的存储单元 base 进行数据存储
    	通过 base 和 top 指针指向连续单元的特定地址
    	top 所指向单元为待插入位置，base 为栈底元素位置
    */
    SeqStack() {
        base = new T[MAX_LEN];
        top = base;
        length = 0;
    }
    ~SeqStack() {
        delete[] base;
    }
    bool empty() {
        return top == base;
    }
    int size() {
        return length;
    }
    /* 由于 top 指向的单元的前一个存储单元才是栈顶，故需要取 [top-1] */
    T getTop() {
        if (top == base)
            return 0;
        return *(top - 1);
    }
    /* 每次将新数据放入 top 指向的空单元，然后 top 后移一个单位，并将长度 +1  */
    bool push(T data) {
        *top++ = data;
        length++;
        return true;
    }
    /* 直接将 top 回退，并将长度 -1，效果类似于栈顶元素弹出，pop() 常常会返回被弹出的元素 */
    T pop() {
        if (top == base)
            return 0;
        top--;
        length--;
        return *top;
    }
    /* 默认从栈顶开始输出，直到栈底 */
    void show() {
        T *p = top;
        for (p--; p != base; p--)
            cout << *p << ' ';
        cout << *p << ' ' << endl;
    }
};
```

#### 链栈

```c++
template <typename T>
struct Node {
    T data;
    Node *next;
    /* data 域存数据，next 域存下一个结点的地址 */
    Node():next(NULL) {}
};
template <typename T>
class ChainStack {
    Node<T> *top;
    int length;
public:
    /*
    	随机存储结构，顺序存取结构
    	top 不存储数据，top->data 存储栈当前的长度
    */
    ChainStack(): length(0) {
        top = new Node<T>;
    }
    ~ChainStack() {
        Node<T> *p;
        while (top) {
            p = top->next;
            delete top;
            top = p;
        }
    }
    int size() {
        return length;
    }
    bool empty() {
        return length == 0;
    }
    /*
    	先将创建的新结点 node 的 next 指向 top 的 next
    	再将 top 的 next 指向 node
        最后将长度 +1
    */
    bool push(T data) {
        Node<T> *node = new Node<T>;
        node->data = data;
        node->next = top->next;
        top->next = node;
        length++;
        return true;
    }
    /*
    	先将 top 的下一个结点（即第一个元素结点）存到 trash
    	再将 top 的 next 指向 trash 的 next
    	最后删除 trash，并将长度 -1
        此外，pop() 常常会返回被弹出的元素
    */
    T pop() {
        if (length == 0)
            return 0;
        Node<T> *trash = top->next;
        top->next = trash->next;
        T res = trash->data;
        delete trash;
        length--;
        return res;
    }
    /* 获取栈顶元素 */
    T getTop() {
        if (length == 0)
            return 0;
        return top->next->data;
    }
    /* 默认从栈顶开始输出，直到栈底 */
    void show() {
        Node<T> *p = top->next;
        while (p != NULL) {
            cout << p->data << ' ';
            p = p->next;
        }
        cout << endl;
    }
};
```

### 队列

#### 链队列

```c++
template <typename T>
struct Node {
    T data;
    Node *next;
    /* data 域存数据，next 域存下一个结点的地址 */
    Node(): next(NULL) {}
};
template <typename T>
class ChainQueue {
    Node<T> *front, *rear;
    int length;
public:
    /*
    	随机存储结构，顺序存取结构
    	front 指向队头，不存储数据，rear 指向队尾，队尾存储最后一个入队元素，length 存储队列当前长度
    */
    ChainQueue(): length(0) {
        front = new Node<T>;
        rear = front;
    }
    ~ChainQueue() {
        Node<T> *p;
        while (front != rear) {
            p = front->next;
            delete front;
            front = p;
        }
        delete rear;
    }
    int size() {
        return length;
    }
    bool empty() {
        return length == 0;
    }
    /* 直接通过 front 获取第一个元素并返回 */
    T getFront() {
        if (length == 0)
            return 0;
        return front->next->data;
    }
    /*
    	将 rear 指向创建的新结点 node
    	再将 rear 更新为 node
        最后将长度 +1
    */
    bool enqueue(T data) {
        Node<T> *node = new Node<T>;
        node->data = data;
        rear->next = node;
        rear = node;
        length++;
        return true;
    }
    /*
    	先将 front 的下一个结点（即第一个元素结点）存到 trash
    	再将 front 的 next 指向 trash 的 next
        预先保存即将出队元素的信息，以便最后返回该元素
        先检查 trash 是否是 rear （即队列只剩下一个元素的情况）
        若是，则重置该即将被 delete 的 rear 为 front
    	最后删除 trash，并将长度 -1，同时返回出队的元素
    */
    T dequeue() {
        if (length == 0)
            return 0;
        Node<T> *trash = front->next;
        front->next = trash->next;
        T res = trash->data;
        if (trash == rear)
            rear = front;
        delete trash;
        length--;
        return res;
    }
    /* 默认从队头开始输出，直到队尾，因队尾的 rear 也有数据，故循环至 NULL 停止 */
    void show() {
        Node<T> *p = front->next;
        while (p != NULL) {
            cout << p->data << ' ';
            p = p->next;
        }
        cout << endl;
    }
};
```

#### 循环队列

```c++
#define MAX_LEN 100
template <typename T>
class CircularQueue {
    T *base;
    int front, rear, length;
public:
    /*
    	顺序存储结构，随机存取结构
    	通过一组连续的存储单元 base 进行数据存储，length 存储顺序表长度
    	front 为队头索引，存储第一个入队元素，rear 为队尾索引，不存储数据
    	front 和 rear 在一个定长数组内循环右移，当 front 的上一个索引为 rear 时，队列满
    */
    CircularQueue(): front(0), rear(0), length(0) {
        base = new T[MAX_LEN];
    }
    ~CircularQueue() {
        delete[] base;
    }
    bool empty() {
        return length == 0;
    }
    int size() {
        return length;
    }
    /* 直接通过 front 索引以获取并返回队头元素 */
    T getFront() {
        if (length == 0)
            return 0;
		return base[front];
    }
    /* 将数据写入 rear 所在的存储单元，再将 rear 循环右移 1，最后将长度 +1 */
    bool enqueue(T data) {
        if (length >= MAX_LEN - 1)
            return false;
        base[rear] = data;
        rear = (rear + 1) % MAX_LEN;
        length++;
        return true;
    }
    /* 先将队头元素存起来，再将 front 循环右移 1，随后将长度 -1，最后返回队头元素 */
    T dequeue() {
        if (length == 0)
            return 0;
        T res = base[front];
        front = (front + 1) % MAX_LEN;
        length--;
        return res;
    }
    /* 默认从队头开始输出，通过循环右移 i 直到队尾，因队尾的 rear 无数据，故到 rear 之前就停止 */
    void show() {
        for (int i = front; i != rear; i = (i + 1) % MAX_LEN)
            cout << base[i] << ' ';
        cout << endl;
    }
};
```

## 串

### KMP算法

```c++
int KMP(string mainstr, string substr) {
    /*
        index 存储需要返回的匹配下标，若为 -1 说明匹配失败
        next[] 存储 KMP 的匹配失败移动对应新下标，nextval[] 为 next[] 的改良版
    */
    int index = -1;
    int *next = new int[substr.size()];
    int *nextval = new int[substr.size()];
    /*
        计算 next[] 的时候，substr[i] 匹配失败移动的对应的新下标 只看前 [0, i-1] 的字符 
        next[]和 nextval[] 的算法步骤：
        1) 初始化 next[0] = nextval[0] = -1;
        2) 从 substr[1] 开始外循环以计算对应移动下标，直到最后一个字符
        3) 开始内循环
        4) 初始化 j 为 i-1
        5) 只要 next[j] >= 0 且 substr[i-1] != substr[j]，就令 j = next[j]
        6) 退出内循环
        7) 令 next[i] = next[j] + 1
        8) 若 substr[i] == substr[next[i]]，令 nextval[i] = nextval[next[i]]
           否则直接令 nextval[i] = next[i]
        9) 重新开始外循环进行下一个字符的移动下标计算，直到计算完所有字符
    */
    next[0] = nextval[0] = -1;
    for (int j, i = 1; i < substr.size(); i++) {
        j = i - 1;
        while (next[j] >= 0 && substr[i - 1] != substr[next[j]])
            j = next[j];
        next[i] = next[j] + 1;
        if (substr[i] == substr[next[i]])
            nextval[i] = nextval[next[i]];
        else
            nextval[i] = next[i];
    }
    /* 开始利用 nextval[] 进行匹配，初始化主串和子串的当前索引值 i = 0 和 j = 0 */
    for (int i = 0, j = 0; i < mainstr.size(); i++) {
        /* 当在允许范围内，主串和子串匹配时，匹配继续，i 和 j 同时右移 */
        while (i < mainstr.size() && j < substr.size() && mainstr[i] == substr[j])
            i++, j++;
        if (j >= substr.size()) {
            /* 无论如何，子串一旦匹配完了，必定匹配成功，匹配成功的位置是 i - j */
            index = i - j;
            break;
        } else if (i >= mainstr.size()) {
            /* 还没匹配完子串，但主串就结束了，故匹配失败 */
            break;
        } else {
            /* 主串没匹配完，子串也是，所以更新 j，继续匹配 */
            j = nextval[j];
            /*
                更新完后，若 nextval[j] == -1，说明 mainstr[i] 已没必要匹配，直接在外层循环 i++
                但如果不为 -1，需要 i-- 来抵消外层循环的 i++
            */
            if (j == -1)
                j = 0;
            else
                i--;
        }
    }
    delete[] next;
    delete[] nextval;
    
    return index;
}
```

## 树和二叉树

### 二叉树

+ 二叉树类型：
  + 满二叉树：深度为 $k$ 且有 $2^k-1$ 个结点的二叉树
  + 完全二叉树：$n$ 个结点都与存在 $1-n$ 编号的满二叉树的结点一一对应的二叉树

+ 二叉树性质：
  1. 在二叉树的第 $k$ 层上至多有 $2^{k-1}$ 个结点 $(k>=1)$
  2. 设度为 $k$ 记为 $n_k$ ，则有 $n=n_0+n_1+n_2$ 和 $n_0=n_2+1$
  3. 具有 $n$ 个结点的完全二叉树的深度为 $\lfloor log_2{n}\rfloor+1$
  4. 对任意结点 $[i](1<=i<=n)$ ，其左孩子为结点 $[2i]$ ，右孩子为结点 $[2i+1]$ ，双亲为结点$[i/2]$

#### 顺序二叉树

```c++
#define MAX_LEN 127
template <typename T>
class SeqBinaryTree {
    T *root, ends, null_element;
    int length;
public:
    /* PRE: 先序遍历，MID: 中序遍历，AFT: 后序遍历 */
    const static char PRE = 'P', MID = 'M', AFT = 'A';
    /*
        顺序存储结构，随机存取结构
        通过一组连续的存储单元 root 进行数据存储，length 存储顺序二叉树长度
        通过二叉树的广度优先遍历顺序进行输入以创建顺序二叉树
        传入的 _null_element 和 _ends 分别代表空结点标志和输入结束标志，这两个参数不是必需的
    */
    SeqBinaryTree(T _null_element, T _end) {
        length = 0;
        null_element = _null_element;
        ends = _end;
        root = new T[MAX_LEN];
        T data;
        while (true) {
            cin >> data;
            if (data == ends)
                break;
            root[length++] = data;
        }
    }
    ~SeqBinaryTree() {
        delete[] root;
    }
    bool empty() {
        return length == 0;
    }
    int size() {
        return length;
    }
    void traverse(char mode) {
        _traverse(mode, 0);
        cout << endl;
    }
private:
    /* 通过传入 mode 以确定遍历顺序，该私有函数将被 show() 调用 */
    void _traverse(char mode, int index) {
        int lchild = 2 * index + 1;
        int rchild = lchild + 1;
        switch (mode) {
            case PRE:
                cout << root[index] << ' ';
                if (lchild < length && root[lchild] != null_element)
                    _traverse(mode, lchild);
                if (rchild < length && root[rchild] != null_element)
                    _traverse(mode, rchild);
                break;
            case MID:
                if (lchild < length && root[lchild] != null_element)
                    _traverse(mode, lchild);
                cout << root[index] << ' ';
                if (rchild < length && root[rchild] != null_element)
                    _traverse(mode, rchild);
                break;
            case AFT:
                if (lchild < length && root[lchild] != null_element)
                    _traverse(mode, lchild);
                if (rchild < length && root[rchild] != null_element)
                    _traverse(mode, rchild);
                cout << root[index] << ' ';
                break;
            default:
                break;
        }
    }
};
```

#### 链式二叉树

```c++
template <typename T>
struct Node {
    T data;
    Node *lchild, *rchild;
    /* data 域存数据，lchild 和 rchild 域存左右子结点的地址 */
    Node() {
        lchild = rchild = NULL;
    }
};
template <typename T>
class BinaryTree {
    Node<T> *root;
    T null_element;
    int length;
public:
    /* PRE: 先序遍历，MID: 中序遍历，AFT: 后序遍历 */
    const static char PRE = 'P', MID = 'M', AFT = 'A';
    /*
        随机存储结构，顺序存取结构
        root 代表二叉树的根结点，null_element 代表空结点标志，length 存储二叉树长度
        通过二叉树的先序遍历顺序进行输入以创建链式二叉树
        链式二叉树的创建将会调用私有函数 _init() 进行递归创建
        传入的 _null_element 代表空结点标志，这个参数不是必需的
    */
    BinaryTree(T __ull_element) {
        length = 0;
        null_element = _null_element;
        T data;
        cin >> data;
        if (data != null_element) {
            root = new Node<T>;
            root->data = data;
            length++;
            _init(root);
        }
    }
    ~BinaryTree() {
        delete[] root;
    }
    bool empty() {
        return length == 0;
    }
    int size() {
        return length;
    }
    void traverse(char mode) {
        _traverse(mode, root);
        cout << endl;
    }
private:
    /*
        通过先序遍历方式进行二叉树的递归创建
        只要输入的不是空结点标志，就创建新结点
        为新结点设置好数据后，递归创建该结点
    */
    void _init(Node<T> *node) {
        T data;
        cin >> data;
        if (data != null_element) {
            node->lchild = new Node<T>;
            node->lchild->data = data;
            length++;
            _init(node->lchild);
        }
        cin >> data;
        if (data != null_element) {
            node->rchild = new Node<T>;
            node->rchild->data = data;
            length++;
            _init(node->rchild);
        }
    }
    /* 通过传入 mode 以确定遍历顺序，该私有函数将被 traverse() 调用 */
    void _traverse(char mode, Node<T> *node) {
        switch (mode) {
            case PRE:
                cout << node->data << ' ';
                if (node->lchild != null_element)
                    _traverse(mode, node->lchild);
                if (node->rchild != null_element)
                    _traverse(mode, node->rchild);
                break;
            case MID:
                if (node->lchild != null_element)
                    _traverse(mode, node->lchild);
                cout << node->data << ' ';
                if (node->rchild != null_element)
                    _traverse(mode, node->rchild);
                break;
            case AFT:
                if (node->lchild != null_element)
                    _traverse(mode, node->lchild);
                if (node->rchild != null_element)
                    _traverse(mode, node->rchild);
                cout << node->data << ' ';
                break;
            default:
                break;
        }
    }
};
```

#### 赫夫曼编码

```c++
#include <map>
struct Node {
    bool coded;
    char data;
    double weight;
    Node *parent, *lchild, *rchild;
    /*
    	data 域存数据，lchild 和 rchild 域存左右子结点的地址
    	coded 标志该结点是否在计算编码子树时被合并过
    */
    Node() {
        data = '\0';
        coded = false;
        parent = lchild = rchild = NULL;
    }
};
class HuffmanTree {
    int num;
    Node *root, *nodes;
    map<char, string> data2code;
public:
    /*
    	num 存储需编码的字符数
    	root 指向赫夫曼树的根结点，nodes[] 存储需编码的字符
    	data2code 存储字符对应的编码
    	构造函数通过传入 字符数组、权重数组、字符个数 进行结点信息的初始化
    	nodes[] 的长度为 2*num-1 是因为每次合并需要生成一次新的结点，共合并 num-1 次
    	[0, num-1] 存储原始结点，[num, 2*num-2] 存储新结点，[2*num-2] 最终成为根结点
    	赫夫曼树建立后，调用 _initCode() 建立 data2code 映射表
    */
    HuffmanTree(char *info, double *weights, int n) {
        num = n;
        nodes = new Node[2 * num - 1];
        for (int i = 0;i < num; i++) {
            nodes[i].data = info[i];
            nodes[i].weight = weights[i];
        }
        _initCode();
    }
    ~HuffmanTree() {
        delete[] nodes;
    }
    void show() {
        for (int i = 0; i < num; i++)
            cout << nodes[i].data << ": " << data2code[nodes[i].data] << endl;
    }
    /* 直接通过映射表 data2code 进行明文 plaintext 所有字符的编码转换 */
    string encode(string plaintext) {
        string ciphertext;
        for (string::iterator it = plaintext.begin(); it != plaintext.end(); it++)
            ciphertext += data2code[*it];
        return ciphertext;
    }
    /*
    	通过密文 ciphertext 中的字符按顺序进行从 root 开始字符检索
    	每次检索不断遍历，直到到达树的有效结点，或 ciphertext 所有字符结束
    	若 ciphertext 未检索结束，此时匹配到字符，说明成功匹配到一个字符，并且能继续新一轮的匹配
    	若 ciphertext 所有字符结束时，仍未检索到字符，说明最后的编码段匹配失败，直接返回 "error"
    	若 ciphertext 所有字符结束的同时，也检索到了字符，说明密文已经完全匹配
    	最后，匹配失败则返回 "error"，否则返回解码后的明文 plaintext
    */
    string decode(string ciphertext) {
        string plaintext;
        Node *node;
        for (string::iterator it = ciphertext.begin(); it != ciphertext.end(); it++) {
            for (node = root; !node->data && it != ciphertext.end(); it++) {
                if (*it == '0') {
                    node = node->lchild;
                } else if (*it == '1') {
                    node = node->rchild;
                } else {
                    return "error";
                }
            }
            if (!node->data && it == ciphertext.end()) {
                return "error";
            } else {
                plaintext += node->data;
                it--;
            }
        }
        return plaintext;
    }
private:
    /*
    	此函数同时包括赫夫曼树的建立和映射表的创建
    	
    	赫夫曼树的建立：
    	搜索最小权值的未合并结点，以及次小权值的未合并结点
    	将新结点 node[num+i] 的 weight 赋值为最小和次小权值结点的 weight 之和
    	将新结点 node[num+i] 的 lchid 和 rchild 指向最小和次小权值结点
    	将这两个结点的 parent 指向新结点 node[num+i]
    	重复以上操作共 num-1 次，得到赫夫曼树的根结点 [2*num-2]
    	令 root 指向根结点 [2*num-2]，这时赫夫曼树已经建立完毕
    	
    	映射表的创建：
    	通过遍历每一个原始结点，逆向搜索至根结点 root
    	每次搜索时，若得知自身为左孩子结点，则对应编码前缀追加 '0'
    	若得知自身为右孩子结点，则对应编码前缀追加 '1'
    	追加到前缀是因为，逆向搜索的话，与正向检索产生的赫夫曼编码正好相反
    */
    void _initCode() {
        for (int i = 0; i < num - 1; i++) {
            int min1, min2;
            for (int j = 0; j < num + i; j++)
                if (!nodes[j].coded) {
                    min1 = j;
                    break;
                }
            for (int j = 0; j < num + i; j++)
                if (!nodes[j].coded && nodes[j].weight < nodes[min1].weight)
                    min1 = j;
            for (int j = 0; j < num + i; j++)
                if (!nodes[j].coded && j != min1) {
                    min2 = j;
                    break;
                }
            for (int j = 0; j < num + i; j++)
                if (!nodes[j].coded && j != min1 && nodes[j].weight < nodes[min2].weight)
                    min2 = j;
            nodes[num + i].weight = nodes[min1].weight + nodes[min2].weight;
            nodes[num + i].lchild = nodes + min1;
            nodes[num + i].rchild = nodes + min2;
            nodes[min1].parent = nodes[min2].parent = nodes + num + i;
            nodes[min1].coded = nodes[min2].coded = true;
        }
        root = nodes + 2 * num - 2;
        Node *node;
        for (int i = 0; i < num; i++) {
            data2code[nodes[i].data] = "";
            for (node = nodes + i; node->parent; node = node->parent) {
                if (node->parent->lchild == node)
                    data2code[nodes[i].data] = "0" + data2code[nodes[i].data];
                else
                    data2code[nodes[i].data] = "1" + data2code[nodes[i].data];
            }
        }
    }
};
```

### 树和森林

```c++
template <typename T>
struct Node {
    T data;
    Node *lchild, *rchild;
    /* data 域存数据，lchild 和 rchild 域存左右子结点的地址 */
    Node() {
        lchild = rchild = NULL;
    }
};
template <typename T>
class BinaryTree {
    Node<T> *root;
    T null_element;
    int length;
public:
    /* PRE: 先序遍历，MID: 中序遍历，AFT: 后序遍历 */
    const static char PRE = 'P', MID = 'M', AFT = 'A';
    /*
        随机存储结构，顺序存取结构
        root 代表二叉树的根结点，null_element 代表空结点标志，length 存储二叉树长度
        通过二叉树的先序遍历顺序进行输入以创建链式二叉树
        链式二叉树的创建将会调用私有函数 _initBinaryTree() 或 _initTree() 进行递归创建
        传入的 _null_element 代表空结点标志，这个参数不是必需的
    */
    BinaryTree(T _null_element) {
        length = 0;
        null_element = _null_element;
        T data;
        cin >> data;
        if (data != null_element) {
            root = new Node<T>;
            root->data = data;
            length++;
            _initBinaryTree(root);
        }
    }
    BinaryTree(T _null_element, int branch) {
        length = 0;
        null_element = _null_element;
        T data;
        cin >> data;
        if (data != null_element) {
            root = new Node<T>;
            root->data = data;
            length++;
            _initTree(root, branch);
        }
    }
    ~BinaryTree() {
        delete[] root;
    }
    bool empty() {
        return length == 0;
    }
    int size() {
        return length;
    }
    Node<T> *getRoot() {
        return root;
    }
    void traverse(char mode) {
        _traverse(mode, root);
        cout << endl;
    }
private:
    /*
    	此函数为二叉树分支构造函数
        通过先序遍历方式进行二叉树的递归创建
        只要输入的不是空结点标志，就创建新结点
        为新结点设置好数据后，递归创建该结点
    */
    void _initBinaryTree(Node<T> *node) {
        T data;
        cin >> data;
        if (data != null_element) {
            node->lchild = new Node<T>;
            node->lchild->data = data;
            length++;
            _initBinaryTree(node->lchild);
        }
        cin >> data;
        if (data != null_element) {
            node->rchild = new Node<T>;
            node->rchild->data = data;
            length++;
            _initBinaryTree(node->rchild);
        }
    }
    /*
    	此函数为普通树分支构造函数
        通过先序遍历方式进行树的递归创建
        只要输入的不是空结点标志，就创建新结点
        为新结点设置好数据后，递归创建该结点
        需要注意的是，此分支构造函数直接将树转化为二叉树
        转化过程主要是：
        一个结点若存在第一个有效子结点，则接到结点的左支
        此后的新的有效子结点都不断接到右支
        每次创建的新结点都要判断是否是有效子结点以决定是否调用分支构造函数
    */
    void _initTree(Node<T> *node, int branch) {
        T data;
        cin >> data;
        if (data != null_element) {
            node->lchild = new Node<T>;
            node->lchild->data = data;
            _initTree(node->lchild, branch);
        }
        Node<T> *p = node->lchild;
        for (int i = 1; i < branch; i++) { 
            cin >> data;
            if (data != null_element) {
                p->rchild = new Node<T>;
                p->rchild->data = data;
                _initTree(p->rchild, branch);
                p = p->rchild;
            }
        }
    }
    /* 通过传入 mode 以确定遍历顺序，该私有函数将被 show() 调用 */
    void _traverse(char mode, Node<T> *node) {
        switch (mode) {
            case PRE:
                cout << node->data << ' ';
                if (node->lchild != NULL)
                    _traverse(mode, node->lchild);
                if (node->rchild != NULL)
                    _traverse(mode, node->rchild);
                break;
            case MID:
                if (node->lchild != NULL)
                    _traverse(mode, node->lchild);
                cout << node->data << ' ';
                if (node->rchild != NULL)
                    _traverse(mode, node->rchild);
                break;
            case AFT:
                if (node->lchild != NULL)
                    _traverse(mode, node->lchild);
                if (node->rchild != NULL)
                    _traverse(mode, node->rchild);
                cout << node->data << ' ';
                break;
            default:
                break;
        }
    }
};
template <typename T>
class Forest {
    BinaryTree<T> **forest;
    Node<T> *root;
    int tree_num;
public:
    const static char PRE = 'P', MID = 'M', AFT = 'A';
    /*
        由于直接 new 数组不能调用含参构造函数，需要用到指针
        通过二级指针作为多棵树的指针数组，每棵树都通过 new 调用含参构造函数直接转化为二叉树
        最后通过将第 i+1 棵树连接到第 i 棵树 的右支
        就能将整个森林转化为一棵二叉树
    */
    Forest(T _null_element, int branch, int tree) {
        tree_num = tree;
        forest = new BinaryTree<T>*[tree_num];
        for (int i = 0; i < tree_num; i++)
            forest[i] = new BinaryTree<T>(_null_element, branch);
        root = forest[0]->getRoot();
        Node<T> *p = root;
        for (int i = 1; i < tree_num; i++) {
            p->rchild = forest[i]->getRoot();
            p = p->rchild;
        }
    }
    ~Forest() {
        for (int i = 0; i < tree_num; i++)
            delete forest[i];
        delete[] forest;
    }
    void show(char mode) {
        _traverse(mode, root);
        cout << endl;
    }
private:
    void _traverse(char mode, Node<T> *node) {
        switch (mode) {
            case PRE:
                cout << node->data << ' ';
                if (node->lchild != NULL)
                    _traverse(mode, node->lchild);
                if (node->rchild != NULL)
                    _traverse(mode, node->rchild);
                break;
            case MID:
                if (node->lchild != NULL)
                    _traverse(mode, node->lchild);
                cout << node->data << ' ';
                if (node->rchild != NULL)
                    _traverse(mode, node->rchild);
                break;
            case AFT:
                if (node->lchild != NULL)
                    _traverse(mode, node->lchild);
                if (node->rchild != NULL)
                    _traverse(mode, node->rchild);
                cout << node->data << ' ';
                break;
            default:
                break;
        }
    }
};
```

## 图

### 图的遍历

```c++
#include <queue>
/* 基于邻接矩阵的图 */
class Graph {
    /*
    	weighted 标志是否带权，visited[] 标志顶点是否被访问过
    	num 存储顶点数目，connectedComponentNum 存储无向图连通分量数目
    	vertex[] 存储顶点信息，adjacencyMatrix[][] 存储邻接矩阵信息
    	INF 存储带权图中的无穷大，mode 存储图的类别（有向图 DIGRAPH 或无向图 UNDIGRAPH ）
    */
    bool weighted, *visited;
    int num, connectedComponentNum;
    string *vertex;
    double **adjacencyMatrix;
    double INF;
    string mode;
public:
    /*
    	通过输入 顶点数目、顶点信息、边数目，边信息 以创建图
    	_mode 应为 "DIGRAPH" 或 "UNDIGRAPH"，默认为无权图
    	若为带权图，则初始化邻接矩阵所有关系为 INF，否则直接初始化为 0
    */
    Graph(int n, string _mode, bool _weighted = false) {
        connectedComponentNum = 0;
        num = n;
        weighted = _weighted;
        mode = _mode;
        INF = 1 << (sizeof(int) * 8 - 1) - 1;
        visited = new bool[num];
        vertex = new string[num];
        adjacencyMatrix = new double*[num];
        for (int i = 0; i < num; i++)
            cin >> vertex[i];
        for (int i = 0; i < num; i++) {
            adjacencyMatrix[i] = new double[num];
            for (int j = 0; j < num; j++)
            	adjacencyMatrix[i][j] = weighted ? (i != j ? INF : 0) : 0;
        }
        string v1, v2;
        int i, j, edge_num;
        double weight;
        cin >> edge_num;
        for (int e = 0; e < edge_num; e++) {
            i = j = 0;
            cin >> v1 >> v2;
            if (weighted)
                cin >> weight;
            while (vertex[i] != v1)
                i++;
            while (vertex[j] != v2)
                j++;
            if (mode == "UNDIGRAPH") {
                adjacencyMatrix[i][j] = adjacencyMatrix[j][i] = weighted ? weight : 1;
            } else if (mode == "DIGRAPH") {
                adjacencyMatrix[i][j] = weighted ? weight : 1;
            }
        }
    }
    /*
    	通过输入 顶点数目、顶点信息、邻接矩阵信息 以创建图
    	由于是直接输入邻接矩阵信息，该构造函数只能创建无权图，且也不需要初始化邻接矩阵为 0
    */
    Graph(int n) {
        connectedComponentNum = 0;
        num = n;
        INF = 1 << (sizeof(int) * 8 - 1) - 1;
        visited = new bool[num];
        vertex = new string[num];
        adjacencyMatrix = new double*[num];
        for (int i = 0; i < num; i++)
            cin >> vertex[i];
        for (int i = 0; i < num; i++) {
            adjacencyMatrix[i] = new double[num];
            for (int j = 0; j < num; j++)
                cin >> adjacencyMatrix[i][j];
        }
    }
    ~Graph() {
        delete[] visited;
        delete[] vertex;
        for (int i = 0; i < num; i++)
            delete[] adjacencyMatrix[i];
        delete[] adjacencyMatrix;
    }
    void showVertex() {
        for (int i = 0; i < num; i++)
            cout << vertex[i] << ' ';
        cout << endl;
    }
    /* 分类显示邻接矩阵，无权图则直接输出每一个值，带权图则需要判断是否无穷大 */
    void showAdjacencyMatrix() {
        for (int i = 0; i < num; i++) {
            for (int j = 0; j < num; j++)
                if (weighted) {
                    if (adjacencyMatrix[i][j] == INF)
                        cout << "oo" << ' ';
                    else
                        cout << adjacencyMatrix[i][j] << ' ';
                } else {
                    cout << adjacencyMatrix[i][j] << ' ';
                }
            cout << endl;
        }
    }
    /*
    	显示每一个顶点的入度、出度和度（有向图）或度（无向图），入度 ID ，出度 OD ，度 TD
    	无向图：无入度和出度，度 TD 为 对应行或列的关系求和，由于是对称矩阵，对行或列求和都一样
    	有向图：入度 ID 为对应列的关系求和，出度 OD 为对应行的关系求和，度 TD 为 ID + OD
    */
    void showDegree() {
        int *ID, *OD, *TD;
        ID = new int[num];
        OD = new int[num];
        TD = new int[num];
        for (int i = 0; i < num; i++)
            ID[i] = OD[i] = 0;
        for (int i = 0; i < num; i++) {
            for (int j = 0; j < num; j++) {
                if (weighted) {
                    ID[i] += adjacencyMatrix[j][i] != INF ? 1 : 0;
                    OD[i] += adjacencyMatrix[i][j] != INF ? 1 : 0;
                } else {
                    ID[i] += adjacencyMatrix[j][i] ? 1 : 0;
                    OD[i] += adjacencyMatrix[i][j] ? 1 : 0;
                }
            }
            if (mode == "UNDIGRAPH")
                TD[i] = ID[i];
            else
                TD[i] = ID[i] + OD[i];
        }
        if (mode == "UNDIGRAPH") {
            for (int i = 0; i < num; i++)
                cout << vertex[i] << ": TD = " << TD[i] << endl;
        } else {
            for (int i = 0; i < num; i++) {
                cout << vertex[i] << ": OD = " << OD[i];
                cout << ", ID = " << ID[i];
                cout << ", TD = " << TD[i] << endl;
            }
        }
        delete[] ID;
        delete[] OD;
        delete[] TD;
    }
    /*
    	获取无向图的连通分量，需要通过遍历图以计算，此处使用深度优先得遍历，hide 是遍历时不显示的标志
    	只通过某一个顶点能够到达的所有顶点及其路径总体作为一个连通分量
    	通过遍历直至所有顶点被访问后，可求得连通分量
    */
    int getConnectedComponentNum() {
        this->traverse("DFS", true);
        return connectedComponentNum;
    }
    /*
    	遍历图，分为基于深度优先搜索的遍历("DFS")和基于广度优先搜索的遍历("BFS")
    	该遍历函数需要传入的 traverse_mode 应为 "DFS" 或 "BFS"，默认遍历时按顺序显示遍历的顶点
    	首先初始化 visited[] 为false，声明所有顶点为未被访问的状态，随后进入外循环
    	进入外循环后，首先找到第一个未被访问的顶点，随后先令连通分量 +1 ，而后进行对应遍历
    	遍历结束后，重新进入外循环，直至所有结点都被访问过后结束
    */
    void traverse(string traverse_mode, bool hide = false) {
        int i, unvisited;
        for (int i = 0; i < num; i++)
            visited[i] = false;
        while (true) {
            for (i = 0; i < num && visited[i]; i++)
                continue;
            if ((unvisited = i) >= num)
                break;
            connectedComponentNum++;
            if (traverse_mode == "DFS") {
                _dfs(unvisited, hide);
            } else if (traverse_mode == "BFS") {
                _bfs(unvisited, hide);
            }
        }
        if (!hide)
            cout << endl;
    }
private:
    /*
    	DFS：
    	通过遍历某个未被访问点，而后递归遍历每个与其相连的未被访问的顶点以进行遍历
    */
    void _dfs(int v, bool hide = false) {
        if (!hide)
            cout << vertex[v] << ' ';
        visited[v] = true;
        for (int j = 0; j < num; j++)
            if (adjacencyMatrix[v][j] && !visited[j])
                _dfs(j, hide);
    }
    /*
    	BFS：
    	通过访问某个未被访问点，而后令其入队主队列，并进入外循环
    	将已被访问的顶点，先入队临时队列，此时主队列为空
    	再从临时队列中筛选与其相连的未被访问的顶点，对其访问后，重新入队主队列，此时临时队列为空
    	重新进入外循环，直至主队列为空，这代表所有顶点已经都被访问过
    */
    void _bfs(int v, bool hide = false) {
        queue<int> q;
        if (!hide)
            cout << vertex[v] << ' ';
        visited[v] = true;
        q.push(v);
        while (!q.empty()) {
            queue<int> tmp_q;
            while (!q.empty()) {
                tmp_q.push(q.front());
                q.pop();
            }
            while (!tmp_q.empty()) {
                int tmp_v = tmp_q.front();
                for (int j = 0; j < num; j++)
                    if (adjacencyMatrix[tmp_v][j] && !visited[j]) {
                        if (!hide)
                            cout << vertex[j] << ' ';
                        visited[j] = true;
                        q.push(j);
                    }
                tmp_q.pop();
            }
        }
    }
};
```

### 最小生成树

```c++
#include <vector>
#include <algorithm>
struct Path {
    int i, j;
    double w;
    /* 存储带权的从结点 i 到结点 j 的路径 */
    Path(int i, int j, double w): i(i), j(j), w(w) { }
};
/* 为权值排序时用到的比较函数 */
bool cmp(Path p1, Path p2) {
    return p1.w < p2.w;
}
class Graph {
    bool flag, *visited,  **markMatrix;
    int num;
    string *vertex;
    double **adjacencyMatrix;
    double INF;
    string mode;
public:
    /*
    	通过 顶点数目、有向性、以及后续输入的带权边建立图
    	flag 是用于 dfs 时检测是否成环的标记，visited[] 标记顶点是否被访问过
    	markMatrix[][] 记录某路径是否被标记，_mode 应为 "UNDIGRAPH" 或 "DIGRAPH"
    */
    Graph(int n, string _mode) {
        num = n;
        mode = _mode; 
        INF = 1 << (sizeof(int) * 8 - 1) - 1;
        visited = new bool[num];
        vertex = new string[num];
        adjacencyMatrix = new double*[num];
        markMatrix = new bool*[num];
        for (int i = 0; i < num; i++)
            cin >> vertex[i];
        for (int i = 0; i < num; i++) {
            adjacencyMatrix[i] = new double[num];
            markMatrix[i] = new bool[num];
            for (int j = 0; j < num; j++) {
                adjacencyMatrix[i][j] = i != j ? INF : 0;
                markMatrix[i][j] = false;
            }
        }
        string v1, v2;
        int i, j, edge_num;
        double weight;
        cin >> edge_num;
        for (int e = 0; e < edge_num; e++) {
            i = j = 0;
            cin >> v1 >> v2 >> weight;
            while (vertex[i] != v1)
                i++;
            while (vertex[j] != v2)
                j++;
            if (mode == "UNDIGRAPH") {
                adjacencyMatrix[i][j] = adjacencyMatrix[j][i] =  weight;
            } else if (mode == "DIGRAPH") {
                adjacencyMatrix[i][j] = weight;
            }
        }
    }
    ~Graph() {
        delete[] visited;
        delete[] vertex;
        for (int i = 0; i < num; i++)
            delete[] adjacencyMatrix[i];
        delete[] adjacencyMatrix;
        delete[] markMatrix;
    }
    void MinimumSpanningTree(string _mode, string _vertex = "") {
        int v = 0;
        while (vertex[v] != _vertex)
            v++;
        if (_mode == "Prim")
            _Prim(v);
        else if (_mode == "Kruskal")
            _Kruskal();
    }
private:
    /*
    	普里姆(Prim)算法：
    	记已选顶点集为 select，未选顶点集为 unselect，选定某一个顶点并入 select
    	通过该顶点开始，不断将 unselect 中最邻近 select 的顶点并入 select 以生成最小生成树
    	最小生成树(Minimum Spanning Tree, MST)其实是最小权重生成树的简称
    	具体步骤：
    	1) 初始化 seleted[] 为 false，其标志 select 的顶点为 true，初始化最短路径长度 pathLen 为 0
    	2) 初始化 minDist[] 为 INF，其记录 unselect 中的点到 select 中最邻近的点的距离
    	3) 初始化 path[] 为 -1，其记录当前顶点的上一个连接点
    	4) 置 selected[v] 为 true，将传入的结点并入 select，并建立 rec[] 按顺序记录并入的路径
    	5) 进入循环，共进行 num-1 次：
    	6) 遍历所有 unselect 的顶点，若它们与 select 的点有路径，且长度小于 minDist[]，则更新
    	7) minDist[] 需要与 path[] 同步更新，以记录与其最邻近的点
    	8) 每次更新 minDist[] 和 path[] 是因为只有最新并入 select 的结点才有可能使之改变
    	9) 遍历 unselect 过后，通过循环找到 minDist[] 中最短的路径
    	10) 将最短路径顶点并入 select，并记录下其下标 min_vertex
    	11) 更新 pathLen，将新边加入 rec，更新最新并入顶点 v 为 min_vertex，置 selected[v] 为 true
    	12) 继续循环至结束，按顺序输出并入的边
    */
    void _Prim(int v) {
        int min_vertex, current_min_dist;
        bool *selected = new bool[num];
        int *path = new int[num];
        double *minDist = new double[num];
        double pathLen = 0;
        for (int i = 0; i < num; i++) {
            selected[i] = false;
            minDist[i] = INF;
            path[i] = -1;
        }
        selected[v] = true;
        vector<Path> rec;
        for (int t = 0; t < num - 1; t++) {
            for (int i = 0; i < num; i++) {
                if (!selected[i] &&adjacencyMatrix[v][i] != INF && adjacencyMatrix[v][i] < minDist[i]) {
                    minDist[i] = adjacencyMatrix[v][i];
                    path[i] = v;
                }
            }
            current_min_dist = INF;
            for (int i = 0; i < num; i++) {
                if (!selected[i] && minDist[i] < current_min_dist) {
                    current_min_dist = minDist[i];
                    min_vertex = i;
                }
            }
            pathLen += adjacencyMatrix[path[min_vertex]][min_vertex];
            rec.push_back(Path(path[min_vertex], min_vertex, adjacencyMatrix[path[min_vertex]][min_vertex]));
            v = min_vertex;
            selected[min_vertex] = true;
        }
        cout << pathLen << endl;
        cout << "Prim:" << endl;
        for (int i = 0; i < rec.size(); i++)
            cout << vertex[rec[i].i] << " " << vertex[rec[i].j] << " " << rec[i].w << endl;
        delete[] selected;
        delete[] minDist;
        delete[] path;
    }
    /*
    	克鲁斯卡尔(Kruskal)算法：
    	通过将所有边按照权值进行排序，并在当其作为路径时不成环的情况下加入生成树，以生成最小生成树
    	最小生成树(Minimum Spanning Tree, MST)其实是最小权重生成树的简称
    	具体步骤：
    	1) 初始化 vec[] 记录所有的边，并根据权值进行升序排列
    	2) 按顺序遍历 vec[] 中所有边，若在当其作为路径时不成环，将其作为虽短路径的部分
    	3) 将 markMatrix[][] 的对应的边置为 true，声明其已被加入生成树，同时输出该边
    	4) 当加入的边到达 num-1 个的时候，最小生成树的生成完成
    */
    void _Kruskal() {
        vector<Path> vec;
        for (int i = 0; i < num; i++)
            for (int j = 0; j < num; j++)
                if (j > i)
                    vec.push_back(Path(i, j, adjacencyMatrix[i][j]));
        sort(vec.begin(), vec.end(), cmp);
        cout << "Kruskal:" << endl;
        for (int i = 0, ctr = 0; i < vec.size() && ctr < num - 1; i++) {
            if (!_circle(vec[i])) {
                ctr++;
                markMatrix[vec[i].i][vec[i].j] = markMatrix[vec[i].j][vec[i].i] = true;
                cout << vertex[vec[i].i] << " " << vertex[vec[i].j] << " " << adjacencyMatrix[vec[i].i][vec[i].j] << endl;
            }
        }
    }
    /*
    	通过 dfs 检测是否已经存在路径从 path.i 能够到达 path.j，若有，则加入该路径会成环
    	初始化 flag 为 false，表明当前能保证不成环，进入 dfs 函数开始遍历
    	若遍历过程中遇到 path.j，flag 将会被置为 true，表明将会成环
    */
    bool _circle(Path path) {
        int i = path.i, j = path.j;
        flag = false;
        for (int k = 0; k < num; k++)
            visited[k] = false;
        _circle_dfs(i, j);
        return flag;
    }
    /*
    	检测是否成环所用的 dfs 函数，在当前保证不成环的前提下(flag = false)进入函数
    	每次遍历仅遍历未被访问且当前已被加入最短路径的那些顶点
    	一旦遇到 path.j(=dst)即置 flag 为 true，并退出函数，最终 _circle() 将会返回 flag(=true)
    */
    void _circle_dfs(int src, int dst) {
        if(!flag) {
            visited[src] = true;
            for (int i = 0; i < num; i++)
                if (!visited[i] && markMatrix[src][i]) {
                    if (i == dst) {
                        flag = true;
                        return ;
                    } else {
                        visited[i] = true;
                        _circle_dfs(i, dst);
                    }
                }
        }
    }
};
```

### 最短路径

```c++
#include <vector>
#include <stack>
/* 基于邻接矩阵的图 */
class Graph {
    /*
    	visited[] 标志顶点是否被访问过，num 存储顶点数目
    	vertex[] 存储顶点信息，adjacencyMatrix[][] 存储邻接矩阵信息
    	INF 存储带权图中的无穷大，mode 存储图的类别（有向图 DIGRAPH 或无向图 UNDIGRAPH ）
    */
    bool *visited;
    int num;
    string *vertex;
    double **adjacencyMatrix;
    double INF;
    string mode;
public:
    /*
    	通过输入 顶点数目、顶点信息、边数目，边信息 以创建带权图
    	_mode 应为 "DIGRAPH" 或 "UNDIGRAPH"，带权图的权值初始化为无穷大
    */
    Graph(int n, string _mode) {
        num = n;
        mode = _mode; 
        INF = 1 << (sizeof(int) * 8 - 1) - 1;
        visited = new bool[num];
        vertex = new string[num];
        adjacencyMatrix = new double*[num];
        for (int i = 0; i < num; i++)
            cin >> vertex[i];
        for (int i = 0; i < num; i++) {
            adjacencyMatrix[i] = new double[num];
            for (int j = 0; j < num; j++)
                adjacencyMatrix[i][j] = i != j ? INF : 0;
        }
        string v1, v2;
        int i, j, edge_num;
        double weight;
        cin >> edge_num;
        for (int e = 0; e < edge_num; e++) {
            i = j = 0;
            cin >> v1 >> v2 >> weight;
            while (vertex[i] != v1)
                i++;
            while (vertex[j] != v2)
                j++;
            if (mode == "UNDIGRAPH") {
                adjacencyMatrix[i][j] = adjacencyMatrix[j][i] = weight;
            } else if (mode == "DIGRAPH") {
                adjacencyMatrix[i][j] = weight;
            }
        }
    }
    ~Graph() {
        delete[] visited;
        delete[] vertex;
        for (int i = 0; i < num; i++)
            delete[] adjacencyMatrix[i];
        delete[] adjacencyMatrix;
    }
    /*
    	显示从 v1 到 v2 的最短路径，可选的最短路径生成方式 _mode 有 "Dijkstra" 和 "Floyd"
    	迪杰斯特拉(Dijkstra)算法和弗洛伊德(Floyd)算法均为最短路径算法
    */
    void shortestPath(string vertex1, string vertex2, string _mode) {
        int v1 = 0, v2 = 0;
        while (vertex[v1] != vertex1)
            v1++;
        while (vertex[v2] != vertex2)
            v2++;
        if (_mode == "Dijkstra")
            _Dijkstra(v1, v2);
        else if (_mode == "Floyd")
            _Floyd(v1, v2);
    }
private:
    /*
    	迪杰斯特拉(Dijkstra)算法：
    	从一个顶点出发，每次更新较短距离数组，并扩展一个与最新并入顶点最近的点，直至搜索到目标顶点 v2
    	具体步骤：
    	1) path[] 存储对应顶点的上一个顶点，初始化时，若顶点与出发点 v1 相连则置为 v1，否则置为 -1
    	2) dist[] 存储对应顶点到 v1 的最短距离，初始化时，直接设置为邻接矩阵中 v1 到其的距离
    	3) 置 visited[] 全为 false，然后将 visited[v1] 置为 true，path[v1] 置为 -1
    	4) 进入循环，直至搜索到目标顶点 v2：
    	5) viaVertex 记录当前中介顶点，closestVertex 记录与中介顶点最近的顶点
    	6) pathLen 记录与 v1 最近的未被选择的顶点，每次循环置为 INF
    	7) 遍历所有未被选择的顶点，若从 v1 到 via 再到 v[i] 的距离小于 v1 到 v[i]，更新 dist[i]
    	8) 无论是否满足上一条，只要小于 pathLen，更新 pathLen 和 closest
    	9) 将 visited[i] 置为 true 表明并入最短路径，并更新中介顶点 via 为 closest
    	10) 不断循环至搜索到 v2，通过 path[] 反向搜索从 v1 到 v2 的最短路径，并将途经点压入栈
    	11) 通过 vec 记录栈的顶点，并且输出，输出的同时计算权值和 cost，随后将其输出
    */
    void _Dijkstra(int v1, int v2) {
        int viaVertex, closestVertex;
        int *path = new int[num];
        double *dist = new double[num];
        double pathLen;
        for (int i = 0; i < num; i++) {
            visited[i] = false;
            dist[i] = adjacencyMatrix[v1][i];
            path[i] = adjacencyMatrix[v1][i] == INF ? -1 : v1;
        }
        visited[v1] = true;
        path[v1] = -1;
        for (viaVertex = v1, closestVertex = -1, pathLen = INF; viaVertex != v2; pathLen = INF) {
            for (int i = 0; i < num; i++)
                if (!visited[i]) {
                    if (dist[viaVertex] + adjacencyMatrix[viaVertex][i] < dist[i]) {
                        dist[i] = dist[viaVertex] + adjacencyMatrix[viaVertex][i]; 
                        path[i] = viaVertex;
                    }
                    if (dist[i] < pathLen) {
                        pathLen = dist[i];
                        closestVertex = i;
                    }
                }
            visited[closestVertex] = true;
            viaVertex = closestVertex;
        }
        stack<int> stk;
        for (int v = v2; v >= 0; v = path[v])
            stk.push(v);
        vector<int> vec;
        while (!stk.empty()) {
            vec.push_back(stk.top());
            stk.pop();
        }
        double cost = 0;
        cout << "Dijkstra: " << "[" << vertex[vec[0]] << "]";
        for (int i = 1; i < vec.size(); cost += adjacencyMatrix[vec[i - 1]][vec[i]], i++)
            cout << "->" << "[" << vertex[vec[i]] << "]";
        cout << endl << "cost: " << cost << endl;
        delete[] path;
        delete[] dist;
    }
    /*
    	弗洛伊德(Floyd)算法：
    	通过遍历每一个点，将其作为中介顶点，用双层循环遍历任意两个顶点，不断更新两点间最短距离，最后得到任意两点间的最短路径
    	具体步骤：
    	1) distMatrix[][] 存储任意两点间的最短距离，seqMatrix[][] 记录从某点到达另一点必须经过的点
    	2) 初始化 distMatrix 为邻接矩阵，初始化 seqMatrix 为终点元素
    	3) 遍历每一个点 v[k]，将其作为中介顶点，用双层循环遍历任意两个顶点 v[i] 和 v[j]
    	4) 若 v[i] 到 v[k] 再到 vj 的距离小于 v[i] 和 v[j] 之间的距离，更新 distMatrix[i][j]
    	5) 同时更新 seqMatrix[i][j] 为 seqMatrix[i][k]，声明到达 v[j] 前必须到达 v[k]
    	6) 利用 distMatrix 从 v1 开始搜索直到遇到 v2，并将途经的顶点加入 vec
    	7) 由于最后 v2 和它的上一个顶点未被记录，需要再加入 v[k] 和 v2 至 vec
    	8) 输出 vec 中途径的顶点，输出的同时计算权值和 cost，随后将其输出
    */
    void _Floyd(int v1, int v2) {
        double **distMatrix = new double*[num];
        int **seqMatrix = new int*[num];
        for (int i = 0; i < num; i++) {
            distMatrix[i] = new double[num];
            seqMatrix[i] = new int[num];
            for (int j = 0; j < num; j++) {
                distMatrix[i][j] = adjacencyMatrix[i][j];
                seqMatrix[i][j] = j;
            }
        }
        for (int k = 0; k < num; k++)
            for (int i = 0; i < num; i++)
                for (int j = 0; j < num; j++)
                    if (distMatrix[i][k] + distMatrix[k][j] < distMatrix[i][j]) {
                        distMatrix[i][j] = distMatrix[i][k] + distMatrix[k][j];
                        seqMatrix[i][j] = seqMatrix[i][k];
                    }
        int vk;
        vector<int> vec;
        for (vk = v1; seqMatrix[vk][v2] != v2; vk = seqMatrix[vk][v2])
            vec.push_back(vk);
        vec.push_back(vk);
        vec.push_back(v2);
        double cost = 0;
        cout << "Floyd: " << "[" << vertex[vec[0]] << "]";
        for (int i = 1; i < vec.size(); cost += adjacencyMatrix[vec[i - 1]][vec[i]], i++)
            cout << "->" << "[" << vertex[vec[i]] << "]";
        cout << endl << "cost: " << cost << endl;
        for (int i = 0; i < num; i++) {
            delete[] distMatrix[i];
            delete[] seqMatrix[i];
        }
        delete[] distMatrix;
        delete[] seqMatrix;
    }
};
```

## 查找

### 静态查找表

#### 顺序表的查找

```c++
// 顺序表类（输入数据无序）
class SequenceList {
    int n, *sl;
public:
    SequenceList(int n): n(n) {
        sl = new int[n + 1];
        for (int i = 1; i <= n; i++)
            cin >> sl[i];
    }
    ~SequenceList() {
        delete[] sl;
    }
/*
	顺序表查找：
	将待查找的值放在 [0]，从右向左遍历查找，最后直接返回索引值（初始为 n），若返回 0 说明查找失败
*/
    int search(int data) {
        int i;
        sl[0] = data;
        for (i = n; i > 0; i--)
            if (sl[i] == *sl)
                return i;
        return i;
    }
};
```

#### 有序表的查找

```c++
// 有序表类（输入数据有序）
class OrderList {
    int n, *ol;
public:
    OrderList(int n): n(n) {
        ol = new int[n];
        for (int i = 0; i < n; i++)
            cin >> ol[i];
    }
    ~OrderList() {
        delete[] ol;
    }
/*
	有序表查找：
	利用折半查找，若在循环中找到数据，直接返回索引；若循环结束后 l > r，说明查找失败，返回 -1
*/
    int search(int data) {
        int l = 0, r = n - 1, m;
        for (m = (n - 1) / 2; l <= r; m = (l + r) / 2) {
            if (ol[m] == data)
                return m;
            else if (ol[m] < data)
                l = m + 1;
            else
                r = m - 1;
        }
        return -1;
    }
};
```

#### 索引顺序表的查找

```c++
// 索引顺序表类（输入数据无序）
class IndexOrderList {
    int n, *iol, sep, *max_values;
public:
    // 索引顺序表在构造时还需要确定分块数量 sep 和输入每一个块的最值，并存储于 *max_values
    IndexOrderList(int n): n(n) {
        iol = new int[n];
        for (int i = 0; i < n; i++)
            cin >> iol[i];
        cin >> sep;
        max_values = new int[sep];
        for (int i = 0; i < sep; i++)
            cin >> max_values[i];
    }
    ~IndexOrderList() {
        delete[] iol;
        delete[] max_values;
    }
/*
	索引顺序表（分块）查找：
	通过最值筛从左到右选数据块，在可能的数据块中使用顺序查找
	若在某一个块中找到，直接返回索引
	若所有块都找不到，说明查找失败，返回 -1
*/
    int search(int data) {
        int part_len = n / sep;
        for (int part = 0; part < sep; part++)
            if (max_values[part] >= data)
                for (int i = 0, *p = iol + part * part_len; i < part_len; p++, i++)
                    if (*p == data)
                        return part * part_len + i;
        return -1;
    }
};
```

### 动态查找表

#### 二叉排序树

```c++
// 二叉排序树的节点类，包括数据域 data，左右孩子节点指针 lchild、rchild，以及父母节点指针 parent
template <typename T>
struct Node {
    T data;
    Node *lchild, *rchild, *parent;
    Node(): lchild(NULL), rchild(NULL), parent(NULL) {};
};
// 二叉排序树类，包括节点数 n，根节点 root
template <typename T>
class BinarySortTree  {
private:
    int n;
    Node<T> *root;
public:
    BinarySortTree(int n) {
        T data;
        cin >> data;
        root = new Node<T>();
        root->data = data;
        for (int i = 1; i < n; i++) {
            cin >> data;
            _insert(data, root);
        }
    }
    ~BinarySortTree() {
        _destoryTree(root);
    }
    void _destoryTree(Node<T> *node) {
        Node<T> *l = node->lchild, *r = node->rchild;
        delete node;
        if (l)
            _destoryTree(l);
        if (r)
            _destoryTree(r);
    }
    void traverse() {
        _midOrder(root);
        cout << endl;
    }
    bool search(T data) {
        return _search(data, root);
    }
    // 在二叉排序树中查找给定元素是否存在
    bool _search(T data, Node<T> *node) {
        // 当前节点数据就是所要找的数据，查找成功
        if (data == node->data)
            return true;
        // 当前节点数据小于所要找的数据，且存在左孩子，递归查找左孩子节点
        if (data < node->data && node->lchild)
            return _search(data, node->lchild);
        // 当前节点数据大于所要找的数据，且存在右孩子，递归查找右孩子节点
        if (data > node->data && node->rchild)
            return _search(data, node->rchild);
        // 以上条件都不满足，查找失败
        return false;
    }
    void remove(T data) {
        _remove(data, root);
    }
    // 在二叉排序树中删除给定元素，若不存在则二叉排序树结构不变
    void _remove(T data, Node<T> *node) {
        // 若当前节点数据就是所要找的数据
        if (node->data != data) {
            // 若当前节点数据小于所要找的数据，且存在左孩子，递归尝试删除左孩子节点
            if (data < node->data && node->lchild)
                _remove(data, node->lchild);
            // 否则，若当前节点数据大于所要找的数据，且存在右孩子，递归尝试删除右孩子节点
            else if (data > node->data && node->rchild)
                _remove(data, node->rchild);
        // 否则，当前节点数据不是所要找的数据
        } else {
            // 若不存在左右孩子
            if (node->lchild == NULL && node->rchild == NULL) {
                // 若当前节点是父母节点的左孩子，则令父母节点的左孩子未空
                if (node == node->parent->lchild)
                    node->parent->lchild = NULL;
                // 否则，当前节点是父母节点的右孩子，则令父母节点的右孩子未空
                else
                    node->parent->rchild = NULL;
                // 删除当前节点
                delete node;
            // 否则，若只存在左孩子，而右孩子不存在
            } else if (node->rchild == NULL) {
                // 若当前节点是父母节点的左孩子，直接令父母节点的左孩子为自己的左孩子
                if (node == node->parent->lchild)
                    node->parent->lchild = node->lchild;
                // 否则，当前节点是父母节点的右孩子，直接令父母节点的右孩子为自己的左孩子
                else
                    node->parent->rchild = node->lchild;
                // 删除当前节点
                delete node;
            // 否则，若只存在右孩子，而左孩子不存在
            } else if (node->lchild == NULL) {
                // 若当前节点是父母节点的左孩子，直接令父母节点的左孩子为自己的右孩子
                if (node == node->parent->lchild)
                    node->parent->lchild = node->rchild;
                // 否则，当前节点是父母节点的右孩子，直接令父母节点的右孩子为自己的右孩子
                else
                    node->parent->rchild = node->rchild;
               	// 删除当前节点
                delete node;
            // 否则，左右孩子均存在
            } else {
                // 找到右子树中最左的节点，该节点也是右子树的最大节点
                Node<T> *replace = node->rchild;
                while (replace->lchild != NULL)
                    replace = replace->lchild;
                // 将右子树的最大节点的数据复制到当前节点，然后递归删除该右子树的最大节点
                node->data = replace->data;
                _remove(replace->data, replace);
            }
        }
    }
    void insert(T data) {
        _insert(data, root);
    }
    // 在二叉排序树中插入数据
    void _insert(T data, Node<T> *node) {
        // 若传入的数据小于当前节点的数据
        if (data < node->data) {
            // 若左孩子存在，递归尝试从左孩子节点处开始插入数据
            if (node->lchild != NULL) {
                _insert(data, node->lchild);
            // 否则，左孩子不存在，直接插入数据
            } else {
                node->lchild = new Node<T>;
                node->lchild->data = data;
                node->lchild->parent = node;
            }
        // 否则，若传入的数据大于等于当前节点的数据
        } else if (data >= node->data) {
            // 若右孩子存在，递归尝试从右孩子节点处开始插入数据
            if (node->rchild != NULL) {
                _insert(data, node->rchild);
            // 否则，右孩子不存在，直接插入数据
            } else {
                node->rchild = new Node<T>;
                node->rchild->data = data;
                node->rchild->parent = node;
            }
        }
    }
    // 中序遍历二叉排序树
    void _midOrder(Node<T> *node) {
        if (node->lchild != NULL)
            _midOrder(node->lchild);
        cout << node->data << ' ';
        if (node->rchild != NULL)
            _midOrder(node->rchild);
    }
};
```

### 哈希表

#### 线性探测再散列

```c++
// 哈希表类，包括哈希表的元素个数 n，哈希表的长度 table_len
// 哈希表 *table，原始数据 *data，哈希函数取模的基数 KEY
class HashTable {
    int n, table_len, *table, *data;
    int KEY;
public:
    // 通过传入哈希表长和数据个数进行构造
    HashTable(int tl, int n): table_len(tl), n(n) {
        // 自定义哈希函数取模基数
        KEY = 11;
        table = new int[table_len];
        data = new int[n];
        // 初始化哈希表数据为 -1，表示数据为空
        for (int i = 0; i < table_len; i++)
            table[i] = -1;
        // 输入 n 个数据进行哈希表的创建
        for (int i = 0; i < n; i++) {
            cin >> data[i];
            // 获取初始哈希值
            int k = Hash(data[i]);
            // 若初始位置存在值，按照线性探测再散列方式，从左到右循环移动，直到找到空位，填入数据
            while (table[k] >= 0)
                k = (k + 1) % table_len;
            table[k] = data[i];
        }
    }
    ~HashTable() {
        delete[] table;
        delete[] data;
    }
    // 在哈希表中查找某元素是否存在
    bool search(int v) {
        // 获取初始哈希值
        int k = Hash(v);
        // 若初始位置存在值，但不是所查找的值，按照线性探测再散列方式，从左到右循环移动，直到找到数据或到达空位
        while (table[k] != v && table[k] >= 0)
            k = (k + 1) % table_len;
        // 若循环结束后，所在处为空位，则表示未查找到
        if (table[k] < 0)
            return false;
        // 否则，查找成功
        return true;
    }
    // 显示哈希表的内容
    void disp() {
        for (int i = 0; i < table_len; i++) {
            if (table[i] >= 0)
                cout << table[i] << ' ';
            else
                cout << "NULL" << ' ';
        }
    }
private:
    // 哈希函数
    int Hash(int v) {
        return v % KEY;
    }
};
```

#### 二次探测再散列

```c++
// 哈希表类，包括哈希表的元素个数 n，哈希表的长度 table_len
// 哈希表 *table，原始数据 *data，哈希函数取模的基数 KEY
class HashTable {
    int n, table_len, *table, *data;
    int KEY;
public:
    // 通过传入哈希表长和数据个数进行构造
    HashTable(int bl, int n): table_len(bl), n(n) {
        KEY = 11;
        table = new int[table_len];
        data = new int[n];
        // 初始化哈希表数据为 -1，表示数据为空
        for (int i = 0; i < table_len; i++)
            table[i] = -1;
        // 输入 n 个数据进行哈希表的创建
        for (int i = 0; i < n; i++) {
            cin >> data[i];
            // 获取初始哈希值
            int sgn = 1, t = 0, k = Hash(data[i]);
            // 若初始位置存在值，按照二次探测再散列方式，进行0,+1,-1,+2,-2,...的循环移动，直到找到空位，填入数据
            while (table[(k + sgn * t * t + table_len) % table_len] >= 0)
                if ((sgn = -sgn) > 0)
                    t++;
            table[(k + sgn * t * t + table_len) % table_len] = data[i];
        }
    }
    ~HashTable() {
        delete[] table;
        delete[] data;
    }
    // 在哈希表中查找某元素是否存在
    bool search(int v) {
        // 获取初始哈希值
        int sgn = -1, t = 0, k = Hash(v);
            // 若初始位置不存在值，按照二次探测再散列方式，进行0,+1,-1,+2,-2,...的循环移动，直到找到数据或到达空位
        while (table[(k + sgn * t * t + table_len) % table_len] >= 0 && table[(k + sgn * t * t + table_len) % table_len] != v)
            if ((sgn = -sgn) > 0)
                t++;
        k = (k + sgn * t * t + table_len) % table_len;
        // 若循环结束后，所在处为空位，则表示未查找到
        if (table[k] < 0)
            return false;
        // 否则，查找成功
        return true;
    }
    // 显示哈希表的内容
    void disp() {
        int i;
        for (i = 0; i < table_len; i++) {
            if (table[i] >= 0)
                cout << table[i] << ' ';
            else
                cout << "NULL" << ' ';
        }
    }
private:
    // 哈希函数
    int Hash(int v) {
        return v % KEY;
    }
};
```

## 内部排序

```c++
/* 
	冒泡排序：
	在给定范围内从左到右，每次出现相邻元素逆序就进行交换，最终最大的元素在该范围的最右边
	然后缩小范围，重复上述步骤，进行 n - 1 趟排序，排序完成
	优化后，通过判断一趟排序是否出现交换，若未出现交换，则元素已经顺序排列，退出排序
	复杂度：O(n^2)
*/
void bubble_sort(int *arr, int n) {
    int i, j, t, swapped;
    // n - 1 趟排序
    for (i = n - 1; i > 0 && !swapped; i--) {
        // 将是否交换的标志置为 0
        swapped = 0;
        // 从左到右依次对比相邻元素，范围为 [0, n - 1]
        for (j = 0; j < i; j++) {
            // 若相邻元素逆序，则进行交换，同时将是否交换的标志置为 1
            if (arr[j] > arr[j + 1]) {
                t = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = t;
                swapped = 1;
            }
        }
    }
}
/*
	选择排序：
	在给定范围内，先假定 k 为最小元素下标，然后从左到右遍历，遇到更大的元素则更新下标
	若在第 i 次循环，下标在遍历后有更新，则将该下标对应元素与第 i 个元素交换
	从而找到 [i, n - 1] 中最小的元素并换到 [i] 的位置
	然后缩小范围，重复上述步骤，进行 n - 1 趟排序，排序完成
	复杂度：O(n^2)
*/
void selection_sort(int *arr, int n) {
    int i, j, k, t;
    // n - 1 趟排序
    for (i = 0; i < n - 1; i++) {
        // 假定 k = i 为最小元素下标
        k = i;
        // 从 [i + 1] 开始，从左向右遍历，找到最小元素并更新最小元素下标
        for (j = i + 1; j < n; j++)
            if (arr[k] > arr[j])
                k = j;
        // 若 k 发生改变，交换 [k] 与 [i]
        if (k != i) {
            t = arr[k];
            arr[k] = arr[i];
            arr[i] = t;
        }
    }
}
/*
	直接插入排序：
	在已排序好的序列中，从右往左遍历
	将所有比当前元素大的向右挪动一位，直到遇到比当前元素小的
	每次挪动都覆盖后一个的位置，因此要先保存当前最右边的元素
	在挪动完成后，令存储的最右边的元素填补到空出的位置，从而完成 [0, i] 的排序
	重复上述步骤，进行 n - 1 趟排序，排序完成
	复杂度：O(n^2)
*/
void insertion_sort(int *arr, int n) {
    int i, j, t;
    // n - 1 趟排序
    for (i = 1; i < n; i++) {
        // 挂起 [i] 的数据
        t = arr[i];
        // 右挪所有比 [i] 大的元素
        for (j = i - 1; j >= 0 && arr[j] > t; j--)
            arr[j + 1] = arr[j];
        // 将 [i] 填入空出的位置
        arr[j + 1] = t;
    }
}
/*
	希尔排序：
	原理类似直接插入排序，但排序的元素之间的间隔为 gap，gap 每趟排序减小一倍
	在 gap 减小的排序过程中，元素将逐渐变为大致有序
	当 gap == 1 时，进行一趟直接插入排序，排序完成
	复杂度：O(n(logn)^2)
*/
void shell_sort(int *arr, int n) {
	int i, j, k, gap, t;
    // 初始化 gap = n >> 1，每趟直接插入排序后，gap >>= 1
	for (gap = n >> 1; gap > 0; gap >>= 1)
        // 由于间隔为 gap，将有多次排序的起点，分别是 [0, gap - 1]
        for (i = 0; i < gap; i++)
            // 此处属于直接插入排序，原理类似直接插入排序
            for (j = i + gap; j < n; j += gap) {
                t = arr[j]; 
                for (k = j - gap; arr[k] > t && k >= i; k -= gap)
                    arr[k + gap] = arr[k];
                arr[k + gap] = t;
            }
}
/*
	堆调整函数：
	若左右子节点不为空，且发现最大的子节点的值大于当前值，与之交换，并调整该子节点
	否则，若左子节点不为空，且左子节点的值发于当前值，与之交换，并调整左子节点
	否则，不做调整
*/
void heapify(int *arr, int n, int i) {
    int t;
    // 计算左右子节点的下标
    int l = (i << 1) + 1;
    int r = l + 1;
    // 左子节点存在
    if (r < n) {
        // 左右子节点均存在，但右子节点的值更大
        if (arr[l] < arr[r]) {
            // 若该子节点的值大于当前值，进行交换，并调整该子节点
            if (arr[i] < arr[r]) {
                t = arr[i];
                arr[i] = arr[r];
                arr[r] = t;
                heapify(arr, n, r);
            }
        // 左右子节点均存在，但左子节点的值更大
        } else {
            // 若该子节点的值大于当前值，进行交换，并调整该子节点
            if (arr[i] < arr[l]) {
                t = arr[i];
                arr[i] = arr[l];
                arr[l] = t;
                heapify(arr, n, l);
            }
        }
    // 右子节点不存在，但左子节点存在
    } else if (l < n) {
        // 若该子节点的值大于当前值，进行交换，并调整该子节点
        if (arr[i] < arr[l]) {
            t = arr[i];
            arr[i] = arr[l];
            arr[l] = t;
            heapify(arr, n, l);
        }
    }
}
/*
	堆排序：
	将数组初始化成为大顶堆
	将堆顶元素与 [0, i] 范围内的最右边元素进行交换
	因堆顶元素被更改，重新调整堆顶
	重复上述步骤，进行 n - 1 趟排序，排序完成
	复杂度：O(nlogn)
*/
void heap_sort(int *arr, int n) {
    int i, t;
    // 初始化一个大顶堆
    for (i = n / 2 - 1; i >= 0; i--)
        heapify(arr, n, i);
   	// 将堆顶元素与 [0, i] 范围内的最右边元素进行交换，共进行 n - 1 趟
    for (i = n - 1; i >= 0; i--) {
        t = arr[i];
        arr[i] = arr[0];
        arr[0] = t;
        heapify(arr, i, 0);
    }
}
/*
	归并排序：
	若序列长度不大于 2，进行简单的判断和交换
	若序列长度大于 2，将序列二分为两个子序列进行递归的归并排序
	子序列归并完成后，开辟新空间
	将两个有序的子序列写入新的空间，数据根据两个子序列元素的大小关系进行交替写入
	交替写入完成后，将剩余未写入的元素直接补到最后
	将新空间的数据复制到原序列，排序完成
	复杂度：O(nlogn)
*/
void merge_sort(int *arr, int n) {
    int t;
    // 序列长度不大于 2
    if (n <= 2) {
        if (n == 2 && arr[0] > arr[1]) {
            t = arr[0];
            arr[0] = arr[1];
            arr[1] = t;
        }
    // 序列长度大于 2
    } else {
        int l, r, left_range, right_range, *left_ptr, *right_ptr, *tmp_arr;
        // 计算左右子序列的范围
        left_range = n >> 1;
        right_range = n - left_range;
        // 计算左右子序列的指针
        left_ptr = arr;
        right_ptr = arr + left_range;
        // 递归使用归并排序进行子序列的排序
        merge_sort(left_ptr, left_range);
        merge_sort(right_ptr, right_range);
        // 分配等大小的空间进行数据的归并
        tmp_arr = (int *)malloc(n * sizeof(int));
        // 初始化左右子序列的数据下标为 0
        t = l = r = 0;
        // 遍历子序列进行数据归并
        while (l < left_range && r < right_range) {
            while (l < left_range && left_ptr[l] <= right_ptr[r])
                tmp_arr[t++] = left_ptr[l++];
            while (r < right_range && right_ptr[r] <= left_ptr[l])
                tmp_arr[t++] = right_ptr[r++];
        }
        // 将剩余未写入的元素直接补到最后
        while (l < left_range)
            tmp_arr[t++] = left_ptr[l++];
        while (r < right_range)
            tmp_arr[t++] = right_ptr[r++];
        // 将新空间的数据复制到原序列
        for (t = 0; t < n; t++)
            arr[t] = tmp_arr[t];
        // 释放分配的空间
        free(tmp_arr);
    }
}
/*
	快速排序：
	若序列长度不大于 1，不做任何处理
	若序列长度大于 1，找到所有比 [0] 小的元素放在左边，所有比 [0] 大的元素放在右边
	然后递归对两个子序列进行快速排序
	将 [0] 挂起，同时置左下标 l 为 0，右下标 r 为 n - 1
	先让 r 向左遍历，直到遇到比 [0] 小的元素，将该元素复制到 l 所在位置，l 右移
	再让 l 向右遍历，直到遇到比 [0] 大的元素，将该元素复制到 r 所在位置，r 左移
	重复以上过程，直至 l >= r，这时候应有 l == r，将挂起的 [0] 填至 [l](即[r])
	递归对 [l](即[r]) 两边的序列进行快速排序，最终完成排序
	复杂度：O(nlogn)
*/
void quick_sort(int *arr, int n) {
    int l = 0, r = n - 1, t = *arr;
    // 序列长度不大于 1，不做任何处理
    if (n <= 1)
        return ;
   	// 序列长度大于 1，找到所有比 [0] 小的元素放在左边，所有比 [0] 大的元素放在右边
    while (l < r) {
        // 让 r 向左遍历，直到遇到比 [0] 小的元素
        while (l < r && t < arr[r])
            r--;
        // 将该元素复制到 l 所在位置，l 右移
        if (l < r)
            arr[l++] = arr[r];
        // 让 l 向右遍历，直到遇到比 [0] 大的元素
        while (l < r && arr[l] < t)
            l++;
        // 将该元素复制到 r 所在位置，r 左移
        if (l < r)
            arr[r--] = arr[l];
    }
    // 这时 l == r，将挂起的 [0] 填至 [l](即[r])
    arr[l] = t;
    // 递归对 [l](即[r]) 两边的序列进行快速排序
    quick_sort(arr, l);
    quick_sort(arr + l + 1, n - l - 1);
}
/*
	基数排序：
	基数排序是特殊的桶排序，此处使用二进制位进行排序
	找到所有数据中最大的数据，并计算其二进制数码的最高位，从而得到分配收集的趟数
	由于有 0 和 1 两个桶，需要分配两个桶的内存 [2][n]，同时还需要元素计数器 ctr[2]
	对于每一趟，首先置 0 和 1 两个桶的计数为 ctr[0] = ctr[1] = 0
	然后计算数字某个二进制位的数码 (0/1)，并将数据防入对应的桶，同时对应的桶的元素计数 + 1
	将分配到 0 和 1 两个桶的数据重新收集到原序列
	每次分配和收集，数据都会趋于有序，同时位运算的运算速度很快，排序速度也能更高效
	多次分配与收集过后，序列达到有序，完成排序
	复杂度：O(nk)
*/
void radix_sort(int *arr, int n) {
    char mark;
    int i, j, k, max_value = *arr, *p, *r, zero_num, range;
    // 有 0 和 1 两个桶，分配两个桶的内存 [2][n]
    int **radix = (int **)malloc(2 * sizeof(int *));
    for (i = 0; i < 2; i++)
        radix[i] = (int *)malloc(n * sizeof(int));
    // 元素计数器 ctr[2]，用于记录两个桶的元素个数
    int *ctr = (int *)malloc(2 * sizeof(int));
    // 找到所有数据的最大值
    for (i = 0; i < n; i++)
        if (max_value < arr[i])
            max_value = arr[i];
   	// 计算数据中最大值的二进制数码的最高位，从而得到分配收集的趟数 range
    for (range = 0; max_value; range++)
        max_value >>= 1;
    // 进行 range 次分配收集
    for (i = 0; i < range; i++) {
        // 元素计数器清零
        ctr[0] = ctr[1] = 0;
        // 计算数字某个二进制位的数码 (0/1)，并将数据防入对应的桶，同时对应的桶的元素计数 + 1
        for (j = 0; j < n; j++) {
            mark = (arr[j] & (1 << i)) >> i;
            radix[mark][ctr[mark]++] = arr[j];
        }
        // 将分配到 0 和 1 两个桶的数据重新收集到原序列
        for (j = 0, p = arr, r = radix[j]; j < 2; r = radix[++j])
            for (k = 0; k < ctr[j]; k++)
                *p++ = r[k];
    }
    // 释放两个桶和计数器的空间
    for (i = 0; i < 2; i++)
        free(radix[i]);
    free(ctr);
}
/*
	计数排序：
	找到数据中的最大值与最小值，从而得到数据范围
	根据数据范围分配一段内存，将数据的值直接映射到这段内存的索引值，令索引所在的值 + 1
	从这段内存最低位开始遍历，索引所在的值代表了元素出现的次数
	按照元素顺序出现的次数，将元素直接写入原序列，完成排序
	复杂度：O(n)
*/
void counting_sort(int *arr, int n) {
    int i, j, t, range, *tmp_arr, *p = arr,  _min = *arr, _max = *arr;
    // 找到数据的最大值和最小值
    for (i = 1; i < n; i++) {
        if (_min > arr[i])
            _min = arr[i];
        else if (_max < arr[i])
            _max = arr[i];
    }
    // 计算数据的范围，从而能够将数据的值直接映射成新内存的索引
    range = _max - _min + 1;
    // 分配新内存，同时对内存清零，其中，分配的内存大小就是数据的范围
    tmp_arr = (int *)calloc(range, sizeof(int));
    // 将数据的值直接映射成对应索引，令索引所在的值 + 1
    for (i = 0; i < n; i++)
        tmp_arr[arr[i] - _min]++;
    // 按照元素顺序出现的次数，将元素直接写入原序列，完成排序
    for (i = 0; i < range; i++) {
        for (j = tmp_arr[i], t = i + _min; j > 0; j--)
            *p++ = t;
    }
    // 释放分配的空间
    free(tmp_arr);
}
```

