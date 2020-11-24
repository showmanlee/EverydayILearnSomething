## 선형 자료구조

**한 원소 뒤에 원소가 하나만 존재**하는, 다시 말해 자료가 **선형으로 배치**된 자료구조를 말한다. 대표적으로 **배열, 연결 리스트, 스택, 큐, 데크** 등이 있다.

### 배열(*Array*)

**물리적으로 연속된 메모리 공간**에 저장된 자료구조.

연속적으로 저장된다는 특징 덕분에 각 배열의 원소들은 인덱스를 통해 접근할 수 있다(*Random Access*). 어떤 배열 원소가 저장되는 메모리 주소는 `배열 시작 위치 + (인덱스 * 자료형 크기)`로 규칙적이기 때문이다. 따라서 데이터 접근/수정 과정의 시간복잡도는 `O(1)`이다.

그러나 배열에 데이터를 삽입/삭제하기 위해서는 삽입하려는 위치 이후의 모든 원소들의 위치를 옮겨주어야 한다. 따라서 데이터 삽입/삭제 과정의 시간복잡도는 `O(n)`이다.

배열에서 특정 값을 검색하는 과정의 시간복잡도도 `O(n)`지만, 배열이 정렬되어 있다면 **이분 탐색** 알고리즘을 통해 시간복잡도를 `O(logn)`으로 줄일 수 있다.

배열은 그 특성상 할당 시 크기를 지정해야 하며, 해당 크기 이상의 자료를 저장할 수는 없다. 그러나 **동적 배열**은 원소 수에 따라 크기가 동적으로 지정된다.

대부분의 프로그래밍 언어에서 배열 자료구조를 기본으로 지원하며, STL에서는 동적 배열을 `vector`라는 Container로 제공한다.

### 연결 리스트(*Linked List*)

데이터와 포인터로 구성된 **노드**가 서로 이어진 형태로 구성된 자료구조. 노드는 다른 노드를 가리키는 포인터를 가지고 있으며, 이 포인터의 개수에 따라 한쪽 방향으로만 연결된 **단방향** 리스트와 양쪽으로 연결된 **양방향** 리스트로 나뉜다.

배열과 달리 **물리적으로는 불연속적인 메모리 공간에 저장**되어 있으며, 때문에 배열과 달리 Random Access가 불가능하다. 따라서 데이터 접근/수정 과정의 시간복잡도는 `O(1)`이고, 특정 값 검색 과정의 시간복잡도도 `O(n)`이다.

반대로 데이터를 삽입/삭제하기 위해서는 양옆 노드의 포인터만 고쳐주면 되기 때문에 시간복잡도가 `O(1)`이다.

STL에서는 연결 리스트를 `list(양방향), forward_list(단방향)`라는 Container로 제공한다.

#### 구현

```c++
// 노드
struct Node{
    int num;
    Node* next;
};

// 추가/삽입
void add(Node* list, int i, int n){
    // 연결 리스트, '인덱스', 추가할 값
    Node* cur = list;
    while(i > 0 && cur != NULL){
        i--;
        cur = cur->next;
    }
    if (cur == NULL) {          // 빈 리스트일 때
        cur = new Node;
        cur->num = n;
        cur->next = NULL;
    }              
    Node* p = new Node;
    p->num = n;
    p->next = cur->next;
    cur->next = p;
}

// 삭제
void remove(Node*& list, int i){
    // 연결 리스트, '인덱스'
    Node* cur = list, prev = NULL;
    while(i > 0 && cur->next != NULL){
        i--;
        prev = cur;
        cur = cur->next;
    }
    if (prev != NULL && cur != NULL){       // 원소가 여러 개일 때
        prev->next = cur->next;
        delete cur;
    }
    else if (prev == NULL && cur != NULL){  // 원소가 1개거나 첫 노드를 제거할 때
        list = cur->next;
        delete cur;
    }
}

// 노드 검색
bool search(Node* list, int n){
    // 연결 리스트, 검색할 값
    Node* cur = list;
    while(cur != NULL){
        if (cur->num == list)
            return true;
        cur = cur->next;
    }
    return false;
}

// 노드 개수
int count(Node* list){
    int ret = 0;
    Node* cur = list;
    while(cur != NULL){
        ret++;
        cur = cur->next;
    }
    return ret;
}
```

### 스택(*Stack*)

후입선출(*Last In First Out; **LIFO***) 기반의 선형 자료구조. 나중에 쌓은 책을 들어내야 먼저 쌓은 책을 뺄 수 있는 책 더미를 생각하면 쉽다. 삽입(*Push*)과 삭제(*Pop*) 과정이 한쪽 끝(*top*)에서만 이루어진다. 

스택은 배열로 구현할 수 있는데, 이때 스택의 크기가 배열 최대 크기에 도달할 경우 문제가 발생할 수 있으므로 이를 최대한 막아야 한다.

STL에서는 스택을 `stack`이라는 Container로 제공한다.

#### 구현

```c++
class Stack{
    int arr[1001];          // 값을 저장하는 배열
    int top;                // 스택 꼭대기의 위치

public:
    Stack(){
        top = -1;
    }

    // 삽입
    void push(int n){
        if (top >= 1000)
            throw ERR;
        top++;
        arr[top] = n;
    }

    // 확인
    int peek(){
        if (top < 0)
            throw ERR;
        return arr[top];
    }

    // 삭제
    int pop(){
        if (top < 0)
            throw ERR;
        int ret = arr[top];
        top--;
        return ret;
    }

    // 스택이 비었는가?
    bool empty(){
        return top == -1;
    }

    // 현재 스택의 크기
    int size(){
        return top + 1;
    }
};
```

### 큐(*Queue*)

선입선출(*First In First Out; **LIFO***) 기반의 선형 자료구조. 먼저 들어간 사람이 먼저 나가는 대기열을 생각하면 쉽다. 삽입(*Enqueue*)과 삭제(*Dequeue*)가 각각 다른 끝(*rear/front*)에서 이루어진다.

큐는 배열로 구현하면 front/rear 포인터가 삽입/삭제 과정에서 점점 뒷쪽으로 밀려 배열의 끝에 도달해 원하는 양보다 적은 데이터만 저장할 수 있게 된다. 이런 문제는 큐를 연결 리스트로 구현하거나 배열 기반 **원형 큐**로 구현하면 해결할 수 있다. **원형 큐**란 저장 배열의 시작과 끝을 이어 저장 배열을 원형으로 관리하는 큐로, 포인터가 배열의 끝에 도달해도 배열 시작 지점으로 돌아가기 때문에 항상 배열 크기만큼 데이터를 저장할 수 있다.

STL에서는 큐를 `queue`라는 Container로 제공한다.

#### 구현(원형 큐)

```c++
class Queue{
    int arr[1000];
    int front;
    int rear;
    int capa;

public:
    Queue(){
        front = 0;
        rear = -1;
        capa = 0;
    }

    // 삽입
    void enqueue(int n){
        if (capa >= 1000)
            throw ERR;
        capa++;
        rear = (rear + 1) % 1000;
        arr[rear] = n;
    }

    // 확인
    int peek(){
        if (capa == 0)
            throw ERR;
        return arr[front];
    }

    // 삭제
    int dequeue(){
        if (capa == 0)
            throw ERR;
        int ret = arr[front];
        capa--;
        front = (front + 1) % 1000;
        return ret;
    }

    // 큐가 비었는가?
    bool empty(){
        return capa == 0;
    }

    // 큐의 크기
    int size(){
        return capa;
    }
};
```

#### 구현(연결 리스트)

```c++
struct Node{
    int num;
    Node* next;
}

class Queue{
    Node* front;        // 앞쪽 노드
    Node* rear;         // 뒷쪽 노드

public:
    Queue(){
        front = rear = NULL;
    }

    // 삽입
    void enqueue(int n){
        Node* p = new Node;
        p->num = n;
        p->next = NULL;
        if (rear != NULL)
            rear->next = p;
        rear = p;
        if (front == NULL)
            front = rear;
    }

    // 확인
    int peek(){
        if (front == NULL)
            throw ERR;
        return front->num;
    }

    // 삭제
    int dequeue(){
        if (front == NULL)
            throw ERR;
        int ret = front->num;
        Node* temp = front;
        front = front->next;
        if (front == NULL)
            rear = NULL;
        delete temp;
        return ret;
    }

    // 큐가 비었는가?
    bool empty(){
        return front == NULL;
    }
    
    // 큐의 크기
    int size(){
        Node* cur = front;
        int ret = 0;
        while(cur != NULL){
            ret++;
            cur = cur->next;
        }
        return ret;
    }
};
```

### 데크(*Deque*)

양방향으로 삽입/삭제가 가능한 선형 자료구조. 앞/뒤(*head/tail*)에서만 접근이 가능한 리스트라고 볼 수 있다.

앞이나 뒤에서 삽입/삭제가 이루어지는 특성상 양방향 연결 리스트로 구현하는 것이 좋다.

STL에서는 덱을 `deque`라는 Container로 제공한다.

#### 구현

```c++
struct Node{
    int num;
    Node* front;
    Node* rear;
}

class Deque{
    Node* head;
    Node* tail;

public:
    Deque{
        head = tail = NULL;
    }

    // 삽입(앞)
    void add_head(int n){
        Node* p = new Node;
        p->num = n;
        if (head != NULL)
            head->front = p;
        p->rear = head;
        p->front = NULL;
        head = p;
    }
    // 삽입(뒤)
    void add_tail(int n){
        Node* p = new Node;
        p->num = n;
        if (tail != NULL)
            tail->read = p;
        p->front = tail;
        p->rear = NULL;
        tail = p;
    }
    // 확인(앞)
    int peek_head(){
        if (head == NULL)
            throw ERR;
        return head->n;
    }
    // 확인(뒤)
    int peek_tail(){
        if (tail == NULL)
            throw ERR;
        return tail->n;
    }
    // 삭제(앞)
    void remove_head(){
        if (head == NULL)
            throw ERR;
        Node* temp = head;
        head = head->rear;
        if (head == NULL)
            tail = NULL;
        delete temp;
    }
    // 삭제(뒤)
    void remove_tail(){
        if (tail == NULL)
            throw ERR;
        Node* temp = tail;
        tail = tail->front;
        if (tail == NULL)
            front = NULL;
        delete temp;
    }
    // 데크가 비었는가?
    bool empty(){
        return head == NULL && rear == NULL;
    }
    // 데크의 크기
    bool size(){
        int ret = 0;
        Node* cur = head;
        while(cur != NULL){
            ret++;
            cur = cur->rear;
        }
        return ret;
    }
};
```