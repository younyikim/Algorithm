# Queue
줄을 지어 순서대로 처리되는 것이 Queue    

`FIFO`(First In First Out) 형태를 가진다.   

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbhvAPe%2FbtqHlVqf0RY%2FY4oCoA4wUkEpvIkU80i43K%2Fimg.png" width="50%">   

`Enqueue(삽입)` :  큐 맨 뒤에 데이터를 추가    
`Dequque(삭제)` : 큐 맨 앞의 데이터를 삭제
<br/>            
              
### Queue의 특징
1. 먼저 들어간 자료가 먼저 나오는 FIFO 구조
2. 삽입과 삭제가 다른 쪽에서 이루어진다.
3. 삽입이 이루어지는 곳은 rear, 삭제가 이루어지는 곳을 front라고 한다.
4. 그래프의 너비 우선 탐색(BFS)에서 사용된다.
5. 컴퓨터의 Buffer에서 주로 사용. 

### Queue 사용법(JAVA)
```java
import java.util.LinkedList;
import java.util.Queue;

Queue<Integer> queue = new LinkedList<>(); // int형 queue
Queue<String> queue = new LinkedList<>(); // String형 queue
```
<br/>
Java에서 Class로 구현된 Stack과는 달리, Queue 메모리는 별도의 interface 형태로 제공된다.   
따라서 Queue interface를 직간접적으로 구현한 클래스가 여러가지가 존재한다.    
그 중에서 Dequeue interface를 구현한 `LinkedList Class`가 queue를 구현하는데 가장 많이 사용된다.   

------------------------------------------------

|**Method**|Explain|
|:---:|:---|
|boolean add(E e)|해당 큐의 맨 뒤에 전달된 요소를 삽입한다. 삽입에 성공하면 true를 반환, 실패시 IllegalStateException을 발생|
|E element()|해당 큐의 맨 앞에 있는 요소를 반환|
|boolean `offer(E e)`|해당 큐 맨 뒤에 전달된 요소를 삽입한다.|
|E `peek()`| 해당 큐의 맨 앞에 있는 요소를 반환한다. 큐가 비어있는 경우 null을 반환|
|E `poll()`|해당 큐의 맨 앞에 있는 요소를 반환하고, 해당 요소를 큐에서 제거한다. 비어있는 경우 null 반환|
|E remove()|해당 큐의 맨 앞에 있는 요소를 제거한다.|
------------------------------------------------

### LinkedList를 이용한 Queue 구현(JAVA)    
[LinkedList에 대한 설명(추가 예정)]()

연결리스트로 구현된 큐는 단일 연결리스트로 구현한다.  

데이터를 담는 Node 객체는 `data`와 `reference to next node`로 이루어져있다.   


<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbxDHXo%2FbtqPyqJipxE%2FIkLpjSOaZ7fkOtb4JJNKjk%2Fimg.png" width="20%">

이 Node를 연결한 형식이 LinkedList가 된다.   

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbz4A3X%2FbtqPDIJaMsJ%2FEqGrydOR2k5zwE1mvWZC1K%2Fimg.png" width="50%">  

`offer()` 메소드를 통해 데이터를 추가하면 아래와 같이 동작한다.   

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F3LfGv%2FbtqPwmtPgBI%2FvQSLQg4K4UUO4Y5vdIP7IK%2Fimg.png" width="50%">

`poll()` 메소드를 통해 데이터를 삭제하면 아래와 같이 동작한다.   

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbp8Hly%2FbtqPC0wu8Gi%2FFQ5TpL5QlG6m6VKk3OWQA0%2Fimg.png" width="50%">

<br />   


#### Queue interface
```java
public interface Queue<E> {
  /* 큐의 마지막에 요소를 추가한다. */
  boolean offer(E e);
  
  /* 큐의 첫번째 요소를 삭제하고 삭제된 요소를 반환한다. */
  E poll();
  
  /* 큐의 첫번째 요소를 반환한다.*/
  E peek();
}  
```     

<br />   

#### Queue class & 생성자
```java
public class LinkedListQueue<E> implements Queue<E> {
  private Node<E> head;
  private Node<E> tail;
  private int size;
  
  public LinkedListQueue() {
    this.head = null;
    this.tail = null;
    this.size = 0;
   }
}   
```   
Main Class에서의 호출
```java
LinkedListQueue<Integer> q = new LinkedListQueue<>();
```    
<br />  


#### offer() method
Queue에 데이터를 추가하는 메소드이다. 맨 뒤(rear)에 데이터를 추가한다.     

1. 새로운 노드를 만든다.
2. 새로운 노드와 기존 연결리스트의 tail 노드와 연결시킨다.
3. tail을 새로 추가한 노드로 업데이트 한다.
   
```java
public boolean offer(E value) {
  Node<E> newNode = new Node<E>(value);
  
  //empty
  if(size == 0) {
    head = newNode;
  }
  // 새로운 노드를 기존의 연결리스트에 연결한다.
  else {
    tail.next = newNode;
  }
  
  // tail update
  tail = newNode;
  size++;
  
  return true;
}  
```
<br />   

#### poll() method
Queue에서 데이터를 삭제하는 메소드. 맨 앞(front)에 데이터를 삭제한다.    

1. head 노드를 찾는다.
2. head 노드의 연결을 끊는다.
3. head 노드를 2번째에 있던 노드로 업데이트 해준다.   

```java
 public E poll() {
    
    // 삭제할 노드가 없는 경우 null return
    if(size == 0) {
      return null;
    }
    
    // 삭제할 head를 임시 저장
    E element = head.data;
    
    // head노드의 다음 노드
    Node<E> nextNode = head.next;
    
    // head의 데이터 삭제
    head.data = null;
    head.next = null;
    
    // head가 가리키는 노드를 삭제한 노드의 다음노드를 가리키리도록 update
    head = nextNode;
    size--;
    
    return element;
 }   
```    
<br />   

   
#### peek() method
Queue에서 가장 앞에 있는 데이터를 삭제하지 않고, 확인만 할때 사용하는 메소드이다.   
```java
public E peek() {
  if(size == 0) {
      retrun null;
   }
   
   return head.data;
}   
```
<br />      

#### length(), isEmpty(), clear()
```java
/* 현재 큐에 있는 요소의 개수를 반환*/
public int length() {
  return size;
}

/* 현재 큐가 비어있는지 확인*/
public boolean isEmpty() {
  return size == 0;
}

/* queue의 모든 요소를 삭제한다.*/
public void clear() {
  for(Node<E> x = head; x != null; ) {
    Node<E> next = x.next;
    x.data = null;
    x.next = null;
    x = next;
  }
  size = 0;
  head = tail = null;
}  
    
```

----------------------------------------------------    
### JavaScript로 Queue 구현하기    

JavaScript의 Array 객체는 `push()`, `pop()`, `shift()`, `unshift()` 메서드를 제공한다.    
이를 적절히 사용하면  Queue와 Stack의 자료구조를 구현할 수 있다.   

Queue의 기능으로 동작하고 싶을때는 `push()`와  `shift()` 를 사용하고,     
Stack의 기능으로 동작하고 싶을때는  `push()`와 `pop()`을 사용하면 된다.     

하지만 Queue를 구현할때, `shift()`를 사용하면 배열의 첫번째 요소가 빠져나가기 때문에 모든 원소들을    
앞으로 한칸씩 이동시켜야한다. 이때 `O(1)`의 시간만으로도 가능한 동작이, `O(N)`의 시간이 소요된다.    

그래서 JavaScript로 구현할 때도 LinkedList로 구현하는 것이 바람직하다.   
아래의 코드는 ES6 OOP 형식으로 Queue를 구현하였다.   


```javascript
import LinkedList from '../linked-list/LinkedList';

export default class Queue {
  constructor() {
    this.linkedList = new LinkedList();
  }

  isEmpty() {
    return !this.linkedList.tail;
  }

  peek() {
    if (!this.linkedList.head) {
      return null;
    }

    return this.linkedList.head.value;
  }

  enqueue(value) {
    this.linkedList.append(value);
  }

  dequeue() {
    const removedHead = this.linkedList.deleteHead();
    return removedHead ? removedHead.value : null;
  }

  toString(callback) {
    return this.linkedList.toString(callback);
  }
}
```    

### Queue를 사용하는 문제들 (백준)   

* [2164번 카드2 - 기본동작](https://www.acmicpc.net/problem/2164)
* [2178번 미로탐색 - BFS](https://www.acmicpc.net/problem/2164)
* [7576번 토마토 - BFS](https://www.acmicpc.net/problem/2164)


-----------------------------------------------------    
Reference
* [자바 Queue 클래스 사용법 & 예제 총정리](https://coding-factory.tistory.com/602)
* [Stack과 Queue
](http://tcpschool.com/java/java_collectionFramework_stackQueue)
* [연결리스트를 이용한 Queue (큐) 구현하기](https://st-lab.tistory.com/184)
* [JS Queue 구현해보기](https://velog.io/@ansrjsdn/JS-Queue-%EA%B5%AC%ED%98%84%ED%95%B4%EB%B3%B4%EA%B8%B0)



