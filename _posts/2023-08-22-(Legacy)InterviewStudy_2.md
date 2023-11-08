---
title: "8/22 면접 준비"
author: Jeremiah Lee
date: 2023-08-22
categories: [ 면접 준비, 스터디 ]
tags: [취준 스터디]
pin: true
math: true
mermaid: true
image: "/assets/img/StudyGroupLog/interview_jelly.png"
path: /commons/devices-mockup.png
lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
alt: Something
---
***

### **Part 1-2 DataStructure**
<https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/DataStructure#hash-table>
1. Array vs Linked List
2. Stack and Queue
3. Tree
4. Binary Heap
5. Red Black Tree
6. Hash Table
7. Graph
   <br><br>
   <br><br>

#### **1. Array vs Linked List**
##### Array
- 가장 기본적인 자료구조인 배열 자료구조는, 논리적 저장 순서와 물리적 저장 순서가 일치한다. 따라서 인덱스(index)로 해당 원소(element)에 접근할 수 있다.
- 찾고자 하는 원소의 인덱스 값을 알고 있으면 Big-O(1)에 해당 원소로 접근할 수 있다. 즉 random access 가 가능하다는 장점이 있는 것
- 하지만 삭제 또는 삽입의 과정에서는 해당 원소에 접근하여 작업을 완료한 뒤(O(1)), 또 한 가지의 작업을 추가적으로 해줘야 하기 때문에, 시간이 더 걸린다.
- 만약 배열의 원소 중 어느 원소를 삭제했다고 했을 때, 배열의 연속적인 특징이 깨지게 된다. 즉 빈 공간이 생기는 것이다.
- 삭제한 원소보다 큰 인덱스를 갖는 원소들을 shift해줘야 하는 비용(cost)이 발생하고 이 경우의 시간 복잡도는 O(n)가 된다.
- 그렇기 때문에 Array 자료구조에서 삭제 기능에 대한 time complexity 의 worst case 는 O(n)이 된다.
- 삽입의 경우도 마찬가지이다. 만약 첫번째 자리에 새로운 원소를 추가하고자 한다면 모든 원소들의 인덱스를 1 씩 shift 해줘야 하므로 이 경우도 O(n)의 시간을 요구하게 된다.

##### Linked List
- 위 배열의 문제를 해결하기 위한 자료구조가 linked list 이다.
- 각각의 원소들은 자기 자신 다음에 어떤 원소인지만을 기억하고 있다.
- 따라서 이 부분만 다른 값으로 바꿔주면 삭제와 삽입을 O(1) 만에 해결할 수 있는 것이다.
- 하지만 Linked List 역시 한 가지 문제가 있다. 원하는 위치에 삽입을 하고자 하면 원하는 위치를 Search 과정에 있어서 첫번째 원소부터 다 확인해봐야 한다는 것이다.
- Array 와는 달리 논리적 저장 순서와 물리적 저장 순서가 일치하지 않기 때문이다.
- 이것은 일단 삽입하고 정렬하는 것과 마찬가지이다.
- 이 과정 때문에, 어떠한 원소를 삭제 또는 추가하고자 했을 때, 그 원소를 찾기 위해서 O(n)의 시간이 추가적으로 발생하게 된다.
- 결국 linked list 자료구조는 search 에도 O(n)의 time complexity 를 갖고, 삽입, 삭제에 대해서도 O(n)의 time complexity 를 갖는다.
- 이 Linked List 는 Tree 구조의 근간이 되는 자료구조이며, Tree 에서 사용되었을 때 그 유용성이 드러난다.
  <br><br>
  <br><br>

#### **2. Stack and Queue**

##### Stack
- 선형 자료구조의 일종으로 Last In First Out (LIFO) - 나중에 들어간 원소가 먼저 나온다. 또는 First In Last Out (FILO) - 먼저 들어간 원소가 나중에 나온다
- 이것은 Stack 의 가장 큰 특징이다. 차곡차곡 쌓이는 구조로 먼저 Stack 에 들어가게 된 원소는 맨 바닥에 깔리게 된다.
- 그렇기 때문에 늦게 들어간 원소들은 그 위에 쌓이게 되고 호출 시 가장 위에 있는 원소가 호출되는 구조이다.

##### Queue
- 선형 자료구조의 일종으로 First In First Out (FIFO). 즉, 먼저 들어간 원소가 먼저 나온다.
- Stack 과는 반대로 먼저 들어간 원소가 맨 앞에서 대기하고 있다가 먼저 나오게 되는 구조이다.
- Java Collection 에서 Queue 는 인터페이스이다.
- 이를 구현하고 있는 Priority queue등을 사용할 수 있다.
  <br><br>
  <br><br>

#### **3. Tree**
- 트리는 스택이나 큐와 같은 선형 구조가 아닌 비선형 자료구조이다.
- 트리는 계층적 관계(Hierarchical Relationship)을 표현하는 자료구조이다.
- Node (노드) : 트리를 구성하고 있는 각각의 요소를 의미한다.
- Edge (간선) : 트리를 구성하기 위해 노드와 노드를 연결하는 선을 의미한다.
- Root Node (루트 노드) : 트리 구조에서 최상위에 있는 노드를 의미한다.
- Terminal Node ( = leaf Node, 단말 노드) : 하위에 다른 노드가 연결되어 있지 않은 노드를 의미한다.
- Internal Node (내부노드, 비단말 노드) : 단말 노드를 제외한 모든 노드로 루트 노드를 포함한다.

##### 트리의 종류
1. Binary Tree (이진 트리)
- 루트 노드를 중심으로 두 개의 서브 트리(큰 트리에 속하는 작은 트리)로 나뉘어 진다.
- 또한 나뉘어진 두 서브 트리도 모두 이진 트리어야 한다.
- 공집합도 이진 트리로 포함시켜야 한다.
- 트리에서는 각 층별로 숫자를 매겨서 이를 트리의 Level(레벨)이라고 한다.
- 레벨의 값은 0 부터 시작하고 따라서 루트 노드의 레벨은 0 이다.
- 그리고 트리의 최고 레벨을 가리켜 해당 트리의 height(높이)라고 한다.

2. BST (Binary Search Tree)
- 이진 탐색 트리는 이진 트리의 일종이다.
- 단 이진 탐색 트리에는 데이터를 저장하는 규칙이 있다.
- 그리고 그 규칙은 특정 데이터의 위치를 찾는데 사용할 수 있다.
- 규칙 1. 이진 탐색 트리의 노드에 저장된 키는 유일하다.
- 규칙 2. 부모의 키가 왼쪽 자식 노드의 키보다 크다.
- 규칙 3. 부모의 키가 오른쪽 자식 노드의 키보다 작다.
- 규칙 4. 왼쪽과 오른쪽 서브트리도 이진 탐색 트리이다.
- 이진 탐색 트리의 탐색 연산은 O(log n)의 시간 복잡도를 갖는다.
  <br><br>
  <br><br>

#### **4. Binary Heap**
- 자료구조의 일종으로 Tree 의 형식을 하고 있으며, Tree 중에서도 배열에 기반한 Complete Binary Tree이다.
- 배열에 트리의 값들을 넣어줄 때, 0 번째는 건너뛰고 1 번 index 부터 루트노드가 시작된다.
- 이는 노드의 고유번호 값과 배열의 index 를 일치시켜 혼동을 줄이기 위함이다.
- 힙(Heap)에는 최대힙(max heap), 최소힙(min heap) 두 종류가 있다.
- Max Heap이란, 각 노드의 값이 해당 children 의 값보다 크거나 같은 complete binary tree를 말한다.
- Max Heap에서는 Root node 에 있는 값이 제일 크므로, 최대값을 찾는데 소요되는 연산의 시간복잡도가 O(1)이다.
- complete binary tree이기 때문에 배열을 사용하여 효율적으로 관리할 수 있다.
  <br><br>
  <br><br>

#### **5. Red Black Tree**
- RBT(Red-Black Tree)는 BST 를 기반으로하는 트리 형식의 자료구조이다.
- Red-Black Tree 에 데이터를 저장하게되면 Search, Insert, Delete 에 O(log n)의 시간 복잡도가 소요된다.
- 동일한 노드의 개수일 때, depth 를 최소화하여 시간 복잡도를 줄이는 것이 핵심이다.
- 동일한 노드의 개수일 때, depth 가 최소가 되는 경우는 tree 가 complete binary tree 인 경우이다.

##### Red-Black Tree 의 정의
- 각 노드는 Red or Black이라는 색깔을 갖는다.
- Root node 의 색깔은 Black이다.
- 각 leaf node 는 black이다.
- 어떤 노드의 색깔이 red라면 두 개의 children 의 색깔은 모두 black 이다.
- 각 노드에 대해서 노드로부터 descendant leaves 까지의 단순 경로는 모두 같은 수의 black nodes 들을 포함하고 있다.
- 이를 해당 노드의 Black-Height라고 한다.

##### Red-Black Tree 의 특징
- Binary Search Tree 이므로 BST 의 특징을 모두 갖는다.
- Root node 부터 leaf node 까지의 모든 경로 중 최소 경로와 최대 경로의 크기 비율은 2 보다 크지 않다. 이러한 상태를 balanced 상태라고 한다.
- 노드의 child 가 없을 경우 child 를 가리키는 포인터는 NIL 값을 저장한다. 이러한 NIL 들을 leaf node 로 간주한다.
  <br><br>
  <br><br>

#### **6. Hash Table**
- hash는 내부적으로 배열을 사용하여 데이터를 저장하기 때문에 빠른 검색 속도를 갖는다.
- 특정한 값을 Search 하는데 데이터 고유의 인덱스로 접근하게 되므로 average case 에 대하여 Time Complexity 가 O(1)이 되는 것이다.
- 하지만 문제는 이 인덱스로 저장되는 key값이 불규칙하다는 것이다.
- 그래서 '특별한 알고리즘'을 이용하여 저장할 데이터와 연관된 고유한 숫자를 만들어 낸 뒤 이를 인덱스로 사용한다.
- 특정 데이터가 저장되는 인덱스는 그 데이터만의 고유한 위치이기 때문에, 삽입 연산 시 다른 데이터의 사이에 끼어들거나, 삭제 시 다른 데이터로 채울 필요가 없으므로 연산에서 추가적인 비용이 없도록 만들어진 구조이다.

##### Hash Function
- '특별한 알고리즘'을 hash method 또는 해시 함수(hash function)라고 하고 이 메소드에 의해 반환된 데이터의 고유 숫자 값을 hashcode라고 한다.
- 저장되는 값들의 key 값을 hash function을 통해서 작은 범위의 값들로 바꿔준다.

##### 좋은 hash function는 어떠한 조건들을 갖추고 있어야 하는가?
- 일반적으로 좋은 hash function는 키의 일부분을 참조하여 해쉬 값을 만들지 않고 키 전체를 참조하여 해쉬 값을 만들어 낸다.
- 하지만 좋은 해쉬 함수는 키가 어떤 특성을 가지고 있느냐에 따라 달라지게 된다.

##### 충돌 해결
1. Open Address 방식 (개방주소법)
- 해시 충돌이 발생하면, (즉 삽입하려는 해시 버킷이 이미 사용 중인 경우) 다른 해시 버킷에 해당 자료를 삽입하는 방식 이다.
2. Separate Chaining 방식 (분리 연결법)
- 일반적으로 Open Addressing 은 Separate Chaining 보다 느리다.
- Open Addressing 의 경우 해시 버킷을 채운 밀도가 높아질수록 Worst Case 발생 빈도가 더 높아지기 때문이다.
- Separate Chaining 방식의 경우 해시 충돌이 잘 발생하지 않도록 보조 해시 함수를 통해 조정할 수 있다면 Worst Case 에 가까워 지는 빈도를 줄일 수 있다.

##### Open Address vs Separate Chaining
- 두 방식 모두 Worst Case 에서 O(M)이다.
- 하지만 Open Address방식은 연속된 공간에 데이터를 저장하기 때문에 Separate Chaining에 비해 캐시 효율이 높다.
- 따라서 데이터의 개수가 충분히 적다면 Open Address방식이 Separate Chaining보다 더 성능이 좋다.

##### 해시 버킷 동적 확장(Resize)
- 해시 버킷의 개수가 적다면 메모리 사용을 아낄 수 있지만 해시 충돌로 인해 성능 상 손실이 발생한다.
- 그래서 HashMap 은 key-value 쌍 데이터 개수가 일정 개수 이상이 되면 해시 버킷의 개수를 두 배로 늘린다.
- 이렇게 늘리면 해시 충돌로 인한 성능 손실 문제를 어느 정도 해결할 수 있다.
  <br><br>
  <br><br>

#### **7. Graph**
- 정점과 간선의 집합
- 트리 또한 그래프이며, 그 중 사이클이 허용되지 않는 그래프를 말한다.
- 정점과 간선의 연결관계에 있어서 방향성이 없는 그래프를 Undirected Graph 라 하고, 간선에 방향성이 포함되어 있는 그래프를 Directed Graph 라고 한다.
- Undirected Graph 에서 각 정점(Vertex)에 연결된 Edge 의 개수를 Degree 라 한다.
- Directed Graph 에서는 간선에 방향성이 존재하기 때문에 Degree 가 두 개로 나뉘게 된다.
- 각 정점으로부터 나가는 간선의 개수를 Outdegree 라 하고, 들어오는 간선의 개수를 Indegree 라 한다.
- 가중치 그래프란 간선에 가중치 정보를 두어서 구성한 그래프를 말한다.

##### 그래프를 구현하는 두 방법
1. 인접 행렬 (adjacent matrix) : 정방 행렬을 사용하는 방법
- 해당하는 위치의 value 값을 통해서 vertex 간의 연결 관계를 O(1) 으로 파악할 수 있다.
2. 인접 리스트 (adjacent list) : 연결 리스트를 사용하는 방법
- vertex 의 adjacent list 를 확인해봐야 하므로 vertex 간 연결되어있는지 확인하는데 오래 걸린다.

###### 그래프 탐색
- 그래프는 정점의 구성 뿐만 아니라 간선의 연결에도 규칙이 존재하지 않기 때문에 탐색이 복잡하다.
- 따라서 그래프의 모든 정점을 탐색하기 위한 방법은 다음의 두 가지 알고리즘을 기반으로 한다.
1. 깊이 우선 탐색 (Depth First Search: DFS)
- 그래프 상에 존재하는 임의의 한 정점으로부터 연결되어 있는 한 정점으로만 나아간다라는 방법을 우선으로 탐색한다.
- 연결할 수 있는 정점이 있을 때까지 계속 연결하다가 더 이상 연결될 수 있는 정점이 없으면 바로 그 전 단계의 정점으로 돌아가서 연결할 수 있는 정점이 있는지 살펴봐야 하기 때문에 Stack을 주로 사용
2. 너비 우선 탐색 (Breadth First Search: BFS)
- 그래프 상에 존재하는 임의의 한 정점으로부터 연결되어 있는 모든 정점으로 나아간다
- 연락을 취할 정점의 순서를 기록하기 위해 Queue 를 사용
