## 트리

**비선형 자료구조**이자 **그래프**의 일종으로, **0개 이상의 자식 노드를 가진 노드들로 구성**된 계층 형태의 연결 그래프이다. `사이클이 없는 그래프`(*Directed Acyclic Graphs; **DAG***)로도 정의할 수 있다.

> 구현 코드는 나중에 손볼 필요 있음

### 개념과 용어

```c++
struct Node{
  int num;
  vector<Node*> child;
}
```

 * 트리는 하나의 **루트 노트**를 가지고, 루트 노트는 0개 이상의 다른 **자식 노드(서브 트리)**를 가진다. 루트가 아닌 다른 노드들은 하나의 **부모 노드**를 가지고, 0개 이상의 다른 **자식 노드(서브 트리)**를 가진다.
 * 노드(*Node*)와 노드는 하나의 간선(*Edge*)으로 연결되어 있으며, 노드간 사이클(순환 구조)을 가지지 않고 한 방향으로 흘러 루트 노드(*Root Node*)에서 단말 노드(*Leaf Node*)로 향하게 된다.
 * 관련 용어
   * **루트 노드(*Root Node*)**: 부모 노드가 없는 최상위 노드
   * **단말 노드(*Leaf Node*)**: 자식 노드가 없는 최하위 노드
   * **부모 노드(*Parent Node*)**: 상위 레벨에서 나와 연결된 노드
   * **자식 노드(*Child Node*)**: 하위 노드에서 나와 연결된 노드
   * **형제 노드(*Sibling Node*)**: 나와 같은 부모 노드를 공유하는 노드
   * **서브 트리(*Sub Tree*)**: 루트 노드가 아닌 노드를 루트 노드로 하여 만든 부분 트리
   * **깊이(*Depth*)**: 루트 노드에서 어떤 노드에 접근하기 위해 거쳐야 하는 간선 수
   * **레벨(*Level*)**: 깊이가 같은 노드들의 집합(루트는 0)
   * **높이(*Height*)**: 가장 깊히 있는 노드의 깊이
   * **너비(*Width*)**: 가장 많은 노드를 가지고 있는 레벨의 노드 개수
   * **차수(*Degree*)**: 내가 가진 서브 트리의 개수(트리 입장에서는 노드 중 가장 큰 차수)
   * **크기(*size*)**: 자신과 자신 아래의 모든 노드들의 수
   * **포레스트(*Forest*)** 서로 연결되지 않은 트리들의 모임

### 이진 트리(*Binary Tree*)

```c++
struct Node{
  int num;
  Node* left;
  Node* right;
}
// or
int tree[MAX];
```

**모든 노드의 최대 차수가 2인**, 즉 모든 노드가 최대 2개까지의 자식 노드를 가질 수 있는 트리이다.

이진 트리가 아닌 트리 역시 *Left-Child Right-Sibling(**LCRS**)* 표현을 통해 이진 트리로 변환할 수 있다. 자신의 자식 노드들 중 `가장 왼쪽의 자식 노드를 왼쪽 자식 노드`로, `자신 바로 오른쪽의 형제 노드를 오른쪽 자식 노드`로 두면 된다. 

모든 노드가 최대 2개까지의 자식 노드를 가지는 것이 보장되기 때문에, 이진 트리는 **1차원 배열**로도 구현할 수 있다. 루트 노드를 `1번`으로 두고, `n번` 노드의 자식 노드는 `n*2번`과 `n*2+1번` 노드, 부모 노드는 `n/2번` 노드로 두면 된다. 이러한 표현법은 트리가 균형적일 때, 특히 **포화 이진 트리**일 때 가장 효율적이지만, 트리가 불균형한 경우, 특히 **편향 이진 트리**일 경우 노드 사이에 빈 공간이 많아져 매우 비효율적이게 된다.

 * **정 이진 트리(*Full Binary Tree*)**: 모든 노드의 차수가 0 또는 2인 이진 트리
 * **완전 이진 트리(*Complete Binary Tree*)**: 마지막 노드를 제외한 모든 레벨이 빈 공간 없이 채워지고, 마지막 레벨의 노드들은 왼쪽부터 채워진 이진 트리(배열로 표현했을 때 `1~n번`까지의 노드가 빠짐없이 채워진 상태)
 * **포화 이진 트리(*Perfect Binary Tree*)**: 노드가 모든 레벨에서 빈틈없이 채워진 이진 트리(높이가 `h`일 때, `2^h - 1`개의 노드가 채워진 경우), 그 자체로 완전 이진 트리이기도 함
 * **편향 이진 트리(*Skewed Binary Tree*)**: 단말 노드를 제외한 모든 노드가 한 쪽으로만 자식을 가지고 있는 이진 트리.
 * **균형 이진 트리(*Balanced Binary Tree*)**: 양쪽 서브 트리의 깊이 차가 1 이하인 이진 트리

### 이진 트리의 순회법

 * **전위 순회(*Pre-Order Traversal*)**: 자신 -> 좌측 -> 우측 순으로 순회한다.
 * **중위 순회(*In-Order Traversal*)**: 좌측 -> 자신 -> 우측 순으로 순회한다.
 * **후위 순회(*Post-Order Traversal*)**: 좌측 -> 우측 -> 자신 순으로 순회한다.
 * **레벨 순회(*Level-Order Traversal*)**: 레벨 순서대로 순회하고, 같은 레벨의 노드들은 왼쪽에 있는 노드부터 순회한다.

전위/중위/후위 순회는 스택(재귀)을 이용한 **깊이 우선 순회(*DFS*)**의 일종이고, 레벨 순회는 큐를 이용한 **너비 우선 순회(*BFS*)**의 일종이다.

```
     F
  B     G
A   D     I
   C E   H
```
이러한 트리(BST)가 있을 경우 순회 결과는 다음과 같다.
 * 전위 순회: `FBADCEGIH`
 * 중위 순회: `ABCDEFGHI`
 * 후위 순회: `ACEDBHIGF`
 * 레벨 순회: `FBGADICEH`

### 스레드 이진 트리(*Thread Binary Tree*)

```c++
struct Node{
  int num;
  Node* left;
  Node* right;
  bool isThreadLeft;
  bool isThreadRight;
}
```

**중위 순회 과정에 스택이나 부모 포인터를 사용하지 않기 위해** 가리키는 곳이 없는 오른쪽 포인터는 **다음 중위 순회 노드**에, 가리키는 곳이 없는 왼쪽 포인터는 **이전 중위 순회 노드**에 연결한 이진 트리이다.

이 경우 연결된 노드가 스레드 노드인지 아닌지를 구별하기 위한 별도의 표시(isThread)가 필요하다.

### 이진 탐색 트리(*Binary Search Tree; **BST***)

 * [알고리즘 시각화](https://www.cs.usfca.edu/~galles/visualization/BST.html)

효율적인 탐색을 위해 이진 트리 구조를 이용한 자료구조이다. 이진 탐색 트리는 다음과 같은 특성이 있다.

 * **모든 노드의 키는 유일하다.**
 * **왼쪽 서브 트리의 키들은 자신보다 작고, 오른쪽 서브 트리의 키들은 자신보다 크다.**
 * **자신이 데리고 있는 서브 트리들 역시 이진 탐색 트리이다.**

이진 탐색 트리는 검색 시 시간복잡도가 `O(h)`가 된다. 여기서 `h`는 트리의 높이로, 처음 이진 탐색 트리를 만들 때 데이터를 어떻게 넣느냐에 따라 트리의 형태가 달라져 탐색 시간에 영향을 주게 된다. 만약 초반 삽입 과정에서 데이터가 적절하게 들어와 `완전 이진 트리`의 형태가 되면 시간복잡도가 `O(logn)`이 되지만, 처음부터 점점 커지는/작아지는 값이 들어오는 등 부적절한 삽입 과정으로 `편향 이진 트리`의 형태가 되면 시간복잡도가 `O(n)`이 되어 `연결 리스트`와 큰 차이가 없어진다.

이러한 상황을 막기 위해 이진 탐색 트리를 균형 트리로 변환하는 알고리즘으로 **AVL 트리, 레드-블랙 트리, B-트리** 등이 있다.

참고로 이진 탐색 트리를 **중위 순회**하면 **정렬된 결과**를 얻을 수 있다.

#### 구현

```c++
struct node{
  int num;
  node* left;
  node* right;
};
class BST {
	node* root;
public:
	// 선언
	BST() {
		root = NULL;
	}

	// 삽입
	void insert(int n) {
		if (root == NULL) {              // 첫 삽입
			root = new node;
			root->num = n;
			root->left = root->right = NULL;
		}
		else {                           // 이후 삽입
			node* cur = root;             // 커서
			while (true) {
				if (cur->num > n) {          // 왼쪽
					if (cur->left == NULL) {   // 신규 삽입
						cur->left = new node;
						cur->left-> num = n;
						cur->left->left = cur->left->right = NULL;
						break;
					}
					else                      // 다음 노드로
						cur = cur->left;
				}
				else if (cur->num < n) {     // 오른쪽
					if (cur->right == NULL) {   // 신규 삽입
						cur->right = new node;
						cur->right->num = n;
						cur->right->left = cur->right->right = NULL;
						break;
					}
					else                      // 다음 노드로
						cur = cur->right;
				}
				else                        // 중복일 경우 삽입 안함
					break;
			}
		}
	}

	// 탐색
	bool search(int n) {
		node* cur = root;       // 커서
		while (cur != NULL) {
			if (cur->num == n)    // 검색 성공
				return true;
			if (cur->num < n)     // 커서 옮기기
				cur = cur->left;
			else
				cur = cur->right;
		}
		return false;           // NULL이므로 검색 실패
	}

	// 삭제
	void remove(int n) {
		node* cur = root;       // 커서
		node* parent = NULL;    // 커서의 부모
		while (cur != NULL) {     // 지워야 할 노드 검색
			if (cur->num == n)
				break;
			parent = cur;         // 부모 노드 저장
			if (cur->num < n)
				cur = cur->left;
			else
				cur = cur->right;
		}
		if (cur == NULL)        // 지워야 할 노드가 없다면 패스
			return;

		if (cur->left == NULL && cur->right == NULL) {         // 지워야 할 노드가 말단 노드라면
			if (parent == NULL)            // 루트 노드를 지워야 한다면
				root = NULL;
			else if (parent->left == cur)  // 부모의 왼쪽 또는 오른쪽 제거
				parent->left = NULL;
			else
				parent->right = NULL;
			delete cur;
		}
		else if (cur->left == NULL || cur->right == NULL) {    // 지워야 할 노드가 하나의 서브 트리를 가진다면
			if (parent == NULL)           // 루트 노드를 지워야 한다면
				root = NULL;
			else {                         // 그렇지 않다면 - 내가 가진 서브 트리를 내가 연결됐던 부모의 서브 트리로 넘겨주기
				node* serve = (cur->left == NULL) ? cur->right : cur->left;
				if (parent->left == cur)
					parent->left = serve;
				else
					parent->right = serve;
			}
			delete cur;
		}
		else {                                                 // 지워야 할 노드가 두 개의 서브 트리를 가진다면
		  // 지워야 할 노드의 왼쪽 서브 트리의 가장 큰 노드 또는 오른쪽 서브 트리의 가장 작은 노드를 선택해 지워야 할 노드의 서브 트리들을 연결한다.
			node* replace_parent = cur;           // 교환할 노드의 부모
			node* replace = cur->right;           // 교환할 노드
			while (replace->left != NULL) {         // 교환할 노드 찾기
				replace_parent = replace;
				replace = replace->left;
			}
			// 이 시점에서 교환할 노드의 서브 트리는 하나밖에 없음
			if (replace_parent->left == replace)      // 교환할 노드가 지워야할 노드 바로 아래 없다면
				replace_parent->left = replace->right;  // 교환할 노드의 부모의 교환할 노드가 달렸던 서브트리를 교환할 노드의 남은 서브 트리로 변경
			else                                      // 교환할 노드가 지워야할 노드 바로 아래 있다면
				replace_parent->right = replace->right; // 지워야할 노드에 교환할 노드의 남은 서브 트리 장착
			cur->num = replace->num;                  // 지워야 할 노드를 교환할 노드의 번호로 변경
			delete replace;                           // 교환할 노드 삭제
		}
	}
};
```

### AVL 트리(*AVL Tree*)

 * [알고리즘 시각화](https://www.cs.usfca.edu/~galles/visualization/AVLtree.html)

노드에 ***Balance Factor(BF)*** 개념을 적용해 **자동으로 균형 이진 트리의 형태**가 되도록 만든 BST. BF는 `왼쪽 서브 트리의 높이 - 오른쪽 서브 트리의 높이`로, AVL에서는 노드 삽입/삭제 시 ***Rotation*** 과정을 통해 모든 노드의 BF값을 `-1~1`로 유지시킨다.

Rotation 과정은 왼쪽/*오른쪽*으로 수행할 수 있다. 

1. 자신(M)의 왼쪽/*오른쪽* 서브 트리의 루트 노드(A)를 자신의 위치로 옮긴다.
2. 자신은 A의 오른쪽/*왼쪽* 노드로 들어간다.
3. A에서 떨어져나간 A의 오른쪽/*왼쪽* 서브 트리를 자신의 왼쪽/*오른쪽* 서브 트리로 만든다.

이를 그림과 코드로 구현하면 다음과 같다:

```
     P            P            P
   M      ->    A     ->     A
 A   c        a b M        a   M
a b                c          b c
```

```c++
// P에게 새롭게 바뀐 루트 노드를 반환(M->A)
Node* rightRotation(Node* M){
  Node* A = M->left;
  Node* b = A->right;
  A->right = M;
  M->left = b;
  return A;
}
P->left = rightRotation(P->left);
// leftRotation은 여기서 L/R을 반대로
```

AVL 트리에서는 노드의 값과 함께 현재 노드의 높이도 같이 저장한다. 트리 삽입/삭제 과정에서 삽입/삭제 위치에서 루트 노드로 올라오면서 높이값을 갱신시켜주는데(`max(왼쪽 높이, 오른쪽 높이) + 1(NULL = 0)`), 이 때 `|왼쪽의 높이 - 오른쪽의 높이|`(`|BF|`)가 2 이상이 되면 Rotation을 수행한다. 이때 높이가 더 높은 서브 트리를 골라 4가지 Rotation 중 하나를 수행한다.

```
- X에서 불균형이 발생한(|BF| > 1) 상황
- Y는 X의 서브 트리 중 높이가 더 높은 것
- Z는 Y의 서브 트리 중 높이가 더 높은 것(두 서브 트리의 높이가 같다면 X-Y와 같은 방향의 것으로)

X            X       X          X
  Y        Y           Y      Y
   Z      Z           Z        Z
 (1)      (2)        (3)      (4)
```

 * (1) - RR 형태인 경우에는 X를 기준으로 `Left Rotation` 수행
 * (2) - LL 형태인 경우에는 X를 기준으로 `Right Rotation` 수행
 * (3) - RL 형태인 경우에는 Y를 기준으로 `Right Rotation` 수행 후 X를 기준으로 `Left Rotation` 수행
 * (4) - LR 형태인 경우에는 Y를 기준으로 `Left Rotation` 수행 후 X를 기준으로 `Right Rotation` 수행

이 과정에서 root가 바뀔 수 있으니 이를 고려해야 한다.

삭제하려는 노드의 서브 트리가 하나 이하인 경우에는 (자신의 위치를 서브 트리에게 넘기고) 해당 위치에서 올라가며 높이를 체크하면 되지만, 서브 트리가 2개인 경우에는 왼쪽 서브 트리의 가장 큰 노드의 값을 삭제하려는 위치의 노드 값으로 가져온 후, 그 값이 있던 노드를 삭제하고 높이를 체크하면 된다.

### 레드-블랙 트리(*Red-Black Tree*)

 * [알고리즘 시각화](https://www.cs.usfca.edu/~galles/visualization/RedBlack.html)

모든 노드에 `Red`, `Black`의 색깔을 주어 **자동으로 균형 이진 트리의 형태**가 되도록 만든 BST. 알고리즘이 상당히 복잡하지만 평균적인 성능이 가장 좋아 실제 상황에서 자주 쓰이며, STL의 Map 역시 레드-블랙 트리 기반으로 구현되어 있다.

레드-블랙 트리에서 모든 노드는 Red와 Black 등 두 가지 색깔 중 하나를 가질 수 있으며, 다음 조건에 맞춰 색상이 칠해진다.
 
 * **루트 노드는 Black이다.**
 * **모든 단말 노드(NIL)는 Black이다.**
 * **Red 노드의 자식 노드들은 Black이다.** = *Red 노드는 연달아 나올 수 없다.*
 * **루트 노드에서 단말 노드로 가는 경로들에는** (NIL을 제외하고) **모두 같은 양의 Black 노드가 있다.**

이러한 조건이 만족되면 루트 노드에서 가장 먼 단말 노드와의 거리와 가장 가까운 거리의 차가 항상 두 배 미만으로 나게 된다. 항상 차를 1 이하로 유지하려고 하는 AVL 트리보다는 덜 균형적이나, 이정도 오차는 실무에서 큰 문제가 되지 않는다. BST를 생성하는 과정 역시 AVL 트리보다 더 효율적이다.

### B-트리(*B-Tree*)
 
 * [알고리즘 시각화](https://www.cs.usfca.edu/~galles/visualization/BTree.html)

#### B+ 트리(*B+ Tree*)

 * [알고리즘 시각화](https://www.cs.usfca.edu/~galles/visualization/BPlusTree.html)

### 힙(*Heap*)

 * [알고리즘 시각화](https://www.cs.usfca.edu/~galles/visualization/Heap.html)

### 트라이(*Trie*)

 * [알고리즘 시각화](https://www.cs.usfca.edu/~galles/visualization/Trie.html)