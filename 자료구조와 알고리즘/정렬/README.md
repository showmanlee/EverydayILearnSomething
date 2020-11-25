## 정렬 알고리즘

말 그대로 배열을 정렬하는데 사용하는 알고리즘들이다.

### 선택 정렬(*Selection Sort*)

> '정렬되지 않은 부분'에서 가장 큰 원소를 찾아 배열 뒤쪽의 '정렬된 부분' 바로 앞쪽의 원소와 위치를 바꾼 뒤, '정렬되지 않은 부분'의 크기가 1이 될 때까지(가장 작은 원소만 남을 때까지) 이를 반복한다. ('정렬된 부분'을 앞쪽으로 둬도 된다 - 이 경우 '작은'과 '큰'을 서로 바꿔준다)

이를 코드로 나타내면 다음과 같다:

```c++
void selectionSort(vector<int>& arr){
    for (int i = arr.size() - 1; i > 0; i--){
        int max = i;                    // 가장 큰 원소가 있는 인덱스
        for (int j = 0; j < i; j++){
            if (arr[j] > arr[max])       
                max = j;                // 정렬 안 된 부분을 순회하면서 가장 큰 원소가 있는 인덱스 찾기
        }
        swap(arr[max], arr[i]);         // 정렬 안 된 부분의 가장 뒷쪽과 가장 큰 원소가 있는 곳의 원소 swap
    }
}
```

시간복잡도는 모든 경우에서 `O(n^2)`이다.

```
위치 지정을 위한 외부 루프 순회: n-1회
최댓값 탐색을 위한 내부 루프에서의 비교: n-1~1회
총 비교 횟수: (n-1)+(n-2)....+2+1 = n(n-1)/2회
swap 과정: 외부 루프 당 1회(i <-> max)
총 시간복잡도: n(n-1)/2 = n(n-1) = O(n^2)
```

### 버블 정렬(*Bubble Sort*)

> 배열의 처음부터 끝까지 인접한 원소끼리 값을 비교해 앞의 원소가 뒤의 원소보다 큰 경우 교환한다. 이 과정이 끝나면 정렬 과정에 참여한 원소 중 마지막 원소를 제외한 나머지 원소를 대상으로 위의 과정을 반복하고, 이를 하나만 남을 때까지 반복한다.

이를 코드로 나타내면 다음과 같다:

```c++
void bubbleSort(vector<int>& arr){
    for (int i = arr.size() - 1; i > 0; i--){   // 정렬할 범위 지정
        for (int j = 0; i < i; j++)
            if (arr[j] > arr[j+1])              // 만약 나보다 내 앞이 더 작다면 교환
                swap(arr[j], arr[j+1]);
    }
}
```

시간복잡도는 모든 경우에서 `O(n^2)`이다. 그러나 정렬시키기 위해 필요한 swap 수가 한 칸씩 swap되는 특성 상 선택 정렬보다 더 많고, 때문에 **선택 정렬보다 더 오래 걸린다.**

```
위치 지정을 위한 외부 루프 순회: n-1회
swap 여부를 위한 내부 루프에서의 비교: n-1~1회
총 비교 횟수: (n-1)+(n-2)....+2+1 = n(n-1)/2회
swap 과정: 최상의 경우 0번, 최악의 경우 외부 루프당 비교 횟수만큼 swap
총 시간복잡도: n(n-1)/2 = n(n-1) = O(n^2)
```

### 삽입 정렬(*Insertion Sort*)

> 앞쪽에 정렬되어 있는 부분 바로 뒤의 원소를 선택해 앞쪽의 원소들과 비교하며 삽입할 위치를 지정한다. 그 다음 그 자리 이후의 원소들을 한 칸 뒤로 옮긴 후 그 자리에 선택한 원소를 삽입한다. 이 과정을 두 번째 원소부터 마지막 원소까지 반복한다.

이를 코드로 나타내면 다음과 같다:

```c++
void insertionSort(vector<int>& arr){
    for (int i = 1; i < arr.size(); i++){               // 두 번째 원소부터 선택
        int idx = i - 1, key = arr[i]                   // 삽입할 위치(idx), 선택한 원소를 따로 저장(key)
        for (; idx >= 0 && arr[idx] > key; idx--)       // idx가 0 이상이고 arr[idx]가 key값보다 더 클 때 한 칸 옮기고 idx--
            arr[idx + 1] = arr[idx];
        arr[idx + 1] = key;                             // 최종 확정된 곳에 key값 삽입
    }
}
```

시간복잡도는 최선의 경우(이미 정렬되어있는 경우)일 때 `O(n)`, 최악의 경우(역순으로 정렬된 경우)일 때 `O(n^2)`, 평균적으로는 `O(n^2)`이다.

```
위치 지정을 위한 외부 루프 순회: n-1회
내부 루프에서 선택한 원소와 바로 앞의 원소와의 최초 비교: 1회
이후 삽입 위치가 바뀔 때마다 이동 횟수와 비교 횟수가 1회씩 증가, 최대 n-1회까지 반복
    ((n-1)+(n-2)....+2+1 = n(n-1)/2회)
총 시간복잡도: O(n^2)
```

#### 셸 정렬(*Shell Sort*)

> 정렬해야 할 배열을 일정한 기준(보통 인덱스 간격)에 따라 분류하여 비연속적인 부분 배열을 만들어 각 부분 배열 별로 삽입 정렬한다. 이후 부분 배열 수를 점점 줄여가며 삽입 정렬을 수행하다가, 마지막에는 전체 배열을 삽입 정렬한다.

삽입 정렬이 정렬이 어느 정도 진행된 배열을 대상으로 더 빠르게 작동한다는 것에서 착안, 여러 개의 부분 배열 별로 삽입 정렬을 수행하는 알고리즘이다. 

이를 코드로 나타내면 다음과 같다:

```c++
void shellSort(vector<int>& arr){
    int gap = arr.size();                   // 간격 - 초기값은 배열의 크기
    while(gap > 0){
        gap /= 2;                           // 매 반복마다 간격값을 /2(결과가 짝수면 홀수로 바꾸기)
        if (gap % 2 == 0)
            gap++;
        for (int g = 0; g < gap; g++){      // 부분 리스트에 삽입 정렬 진행
            for (int i = g + gap; i < arr.size(); i += gap){               
                int idx = i - gap, key = arr[i]                   
                for (; idx >= g && arr[idx] > key; idx -= gap)       
                    arr[idx + gap] = arr[idx];
                arr[idx + gap] = key;                             
            }
        }
    }
}
```

이를 이용해 평균 시간복잡도를 `O(n^1.5)`로 줄일 수 있다.

### 병합 정렬(*Merge Sort*)

> 정렬되지 않은 배열의 크기가 0이나 1이 될 때까지 반으로 나눈다. 이렇게 나눠진 배열을 양옆에 있는 배열끼리 재귀적으로 합치면서 정렬한다. 합치는 과정에서는 정렬된 두 배열에 있는 원소들을 하나씩 비교하여 작은 값을 통합된 배열에 먼저 넣고, 이렇게 한 배열이 비면 다른 배열의 모든 원소를 그대로 넣는다.

**분할 정복**을 이용한 정렬 방식으로, 분할-정복 과정에서 분할된 배열을 저장할 추가 저장 공간이 필요하다(연결 리스트로 구현하면 추가 공간을 확보하지 않아도 된다).

이를 코드로 나타내면 다음과 같다:

```c++
void mergeSort(vector<int>& arr, int left, int right){
    // 배열, 좌측값(0), 우측값(arr.size() - 1)
    int mid = (left + right) / 2;
    if (left < right){      // left가 right보다 크거나 같으면 나눠진 크기가 1 이하 - 더 나눌 필요 없음
        mergeSort(arr, left, mid);              // 좌측 분할
        mergeSort(arr, left + 1, right);        // 우측 분할
        merge(arr, left, mid, right);           // 병합 및 정렬
    }
}

void merge(vector<int>& arr, int left, int mid, int right){
    // 배열, 좌측값, 중앙값, 우측값
    int i = left;       // 좌측 순회
    int j = mid + 1;    // 우측 순회
    vector<int> v;      // 정렬 상태를 저장하는 임시 배열
    
    while(i <= mid && j <= right){      // 양쪽 배열이 안 비워진 상태
        if (arr[i] < arr[j]){           // 둘 중 작은 쪽을 임시 배열에 저장, 해당 원소를 분할 배열에서 제거
            v.push_back(arr[i]);
            i++;
        }
        else{
            v.push_back(arr[j])
            j++;
        }
    }

    // 한쪽 배열이 모두 비워진 경우, 다른 배열의 원소 모두 임시 배열에 저장
    if (i > mid){
        while(j <= right){
            v.push_back(arr[j])
            j++;
        }
    }
    if (j > right){
        while(i <= mid){
            v.push_back(arr[i])
            i++;
        }
    }

    // 정렬된 임시 배열을 원래 배열로 복사
    int t = left;
    for (int s : v){
        arr[t] = s;
        t++;
    }
}
```

시간복잡도는 언제나 `O(nlogn)`이다.

```
비교 횟수: nlogn(n/2쌍을 1번 비교, n/4 쌍을 2번 비교... 2쌍을 n/2번 비교, 1쌍을 n번 비교)
삽입 횟수: 2nlogn(비교 횟수의 2배 - 큰 것도 넣고 작은 것도 넣음)
총 시간복잡도: 3nlogn = O(nlogn)
```

### 퀵 정렬(*Quick Sort*)

> (주로)맨 앞의 원소를 기준 원소(*Pivot*)으로 지정하고, 나머지 원소를 Pivot보다 작으면 Pivot 왼쪽, 크면 오른쪽으로 옮긴다. 이후 Pivot 위치를 기준으로 왼쪽 배열과 오른쪽 배열으로 나누어 분할된 배열의 크기가 1 이하가 아니면 같은 과정을 수행해 정렬한다.

역시 **분할 정복**을 이용한 정렬 알고리즘으로, 병합 정렬과 달리 정렬 과정에 별도의 공간이 필요없고 불규칙한 배열을 대상으로는 같은 `O(nlogn)` 알고리즘보다 더 빨라 C++ STL를 비롯한 여러 라이브러리에서 기본으로 제공한다.

이를 코드로 나타내면 다음과 같다:

```c++
void quickSort(vector<int>& arr, int left, int right){
    // 배열, 좌측값(0), 우측값(arr.size() - 1)
    if (left < right){  // left가 right보다 크거나 같으면 나눠진 크기가 1 이하 - 더 나눌 필요 없음        
        int mid = partition(arr, left, right)       // 중간값 산출 및 pivot 배치
        quickSort(arr, left, mid - 1);              // 좌측 분할
        quickSort(arr, mid + 1, right);             // 우측 분할
    }
}

int partition(vector<int>& arr, int left, int right){
    // 배열, 좌측값(0), 우측값(arr.size() - 1)
    int low = left + 1, high = right;               // 분할을 위한 low, high 커서 지정
    int pivot = arr[left]                           // Pivot값은 맨 앞의 원소
    // low와 high가 교차할 때까지 탐색 반복
    while(low <= right)
        // 교차하거나 low/high 커서가 Pivot보다 큰/작은 값을 가리킬 때까지 커서 이동
        while(low <= right && arr[low] < pivot){
            low++;
        }
        while(low <= right && arr[high] > pivot){
            high--;
        }
        // 커서가 멈춘 상태가 교차한 상황이 아닐 경우 두 커서에 있는 값 swap
        if (low < right)
            swap(arr[low], arr[high]);
    }
    // Pivot값을 중앙으로 이동
    swap(arr[high], arr[left]);

    // Pivot 위치 반환
    return high;
}
```

시간복잡도는 최악의 경우(이미 정렬 or 역정렬되어 있는 경우) `O(n^2)`, 그 외의 경우는 `O(nlogn)`이다. 최악의 경우를 막기 위해서는 Pivot 위치 선정에 추가적인 알고리즘이 필요하다.

```
비교 횟수: nlogn(n/2쌍을 1번 비교, n/4 쌍을 2번 비교... 2쌍을 n/2번 비교, 1쌍을 n번 비교)
    - 단, 최악의 경우에는 n^2(1개와 n-1개로 분할, 1개와 n-2개로 분할... 1개와 2개로 분할, 1개씩 분할)
이동 횟수: 최대 n/2번
총 시간복잡도: O(nlogn)(or O(n^2))
```

### 힙 정렬(*Heap Sort*)

> 힙에 모든 데이터를 넣고, 힙에서 빠지는 데이터 순서대로 정렬한다.

이진 트리 기반 자료구조 **힙**을 이용한 알고리즘이다. 정렬을 위해 별도의 힙을 생성해야 한다.

STL 적용 시, 코드는 다음과 같다(최소 힙으로 변환 시).

```c++
void quickSort_simple(vector<int>& arr){
    priority_queue<int, vector<int>, greater<int>> pq;
    for (int i : arr)
        pq.push(i);
    for (int i = 0; i < arr.size(); i++){
        arr[i] = pq.top();
        pq.pop();
    }
}
```

시간복잡도는 언제나 `O(nlogn)`이다.

```
힙 삽입/삭제 연산의 복잡도: O(logn)(최대 힙의 깊이(logn)만큼 비교 수행)
이러한 힙 삽입/삭제 연산이 2n번 반복
총 시간복잡도: 2n * logn = O(nlogn)
```

### 특수 정렬 알고리즘

#### 기수 정렬(*Radix Sort*)

> 가장 낮은 자릿수의 숫자에 따라 마련된 10개의 버킷(큐)에 데이터를 순서대로 넣고, 이후 0번 버킷부터 순서대로 데이터를 빼 정렬을 수행한다. 이 과정을 가장 높은 자릿수까지 반복한다.

**정렬하려는 배열의 값이 모두 일정 자릿수 이하의 음이 아닌 정수인 배열**에서만 사용할 수 있는 정렬 알고리즘으로, 원소간 비교를 수행하지 않는다.

이를 코드로 나타내면 다음과 같다:

```c++
void radixsort(vector<int>& arr, int k){
    // 배열, 자릿수
    queue<int> bucket[10];          // 큐로 구현한 버킷
    for (int i = 0; i < k; i++){
        int p = pow(10, i);             // 자릿수 추출
        for (int j : arr)               // 버킷에 삽입
            bucket[j / p % 10].push(j);
        int idx = 0;
        for (int i = 0; i < 10; i++){   // 버킷 비워서 정렬하기
            while(!bucket[i].empty()){
                arr[i] = bucket[i].front();
                bucket[i].pop();
                idx++;
            }
        }
    }
}
```

시간복잡도는 `O(n)`이다.

```
버킷에 데이터 삽입/삭제 연산: 2n번
이 과정을 각 자릿수(k)마다 반복
총 시간복잡도: 2n * k = O(n)
```

#### 카운팅 정렬(*Counting Sort*)

> 배열에서 등장하는 개별 원소의 수를 센 뒤, 이를 이용해 배열을 채워넣어 정렬한다.

**정렬하려는 배열의 값이 모두 음이 아닌 정수이고, 분포도가 낮은 배열**에서만 사용할 수 있는 정렬 알고리즘으로, 원소의 개수를 세서 이를 기반으로 값을 새로 채워넣는 방식으로 정렬한다. 정렬한 결과를 저장하는 별도의 배열이 필요하다.

이를 코드로 나타내면 다음과 같다.

```c++
vector<int> countingSort(vector<int> arr, int max){
    // 배열, 최댓값
    vector<int> count(max + 1);             // 원소 수를 세는 배열
    vector<int> res(arr.size());            // 정렬된 배열
    for (int i : arr)
        count[i]++;                         // 원소 수 카운팅
    for (int i = 1; i < max + 1; i++)
        count[i] += count[i-1];             // 카운트를 이용해 누적합 배열로 바꾸기
    for (int i = arr.size() - 1; i >= 0; i--){  // 원래 배열을 거꾸로 순회하며 누적합을 이용해 배열 채워넣기
        res[count[arr[i]]] = arr[i];
        count[arr[i]]--;
    }
    return res;
}
```

시간복잡도는 `O(n)`이다.

```
배열 순회 및 정렬 저장: 2n회
총 시간복잡도: O(n)
```

### 안정 정렬과 불안정 정렬

어떠한 키값을 기준으로 정렬 시(i.e. pair에서 첫 번째 키를 기준으로 정렬) **같은 키값을 가진 원소의 순서가 정렬 후에도 유지**되면 **안정 정렬**(*Stable Sort*), 그렇지 않으면 **불안정 정렬**(*Unstable Sort*)라 부른다. 

 * 안정 정렬: **삽입 정렬, 버블 정렬, <u>병합 정렬</u>, 기수 정렬**
 * 불안정 정렬: **선택 정렬, 셸 정렬, <u>퀵 정렬</u>, 힙 정렬, 카운팅 정렬**

STL에서는 불안정 정렬(퀵 정렬) 기반의 `sort`와 안정 정렬(병합 정렬) 기반의 `stable_sort`를 제공한다.