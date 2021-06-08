# LinkedList
* 1 . [LinkedList](#LinkedList)   
* 2 . [LinkedList 구현(JAVA)](#LinkedList-구현JAVA)   
  *    1) [Node 구현](#Node-구현)
  *    2) [List Interface](#List-Interface)
  *    3) [클래스 및 생성자](#클래스-및-생성자)
  *    4) [search() Method](#search-Method)   
  *    5) [add() Method](#add-Method)   
  *    6) [remove() Method](#remove-Method)
  *    7) [get(), set(), indexOf(), contains() Method](#get-set-indexOf-contains-Method)
 


* ArrayList와 LinkedList 모두 List에서 구현 방법만 달리했을 뿐 사용방법은 유사하다.  
* LinkedList와 ArrayList 간의 차이점은 Node 객체를 사용하여 연결을 생성한다는 것이다.   
* `ArrayList`의 경우는 최상위의 `Object[] 배열`을 사용하여 데이터를 담는다.    
* 이와 다르게 `LinkedList`는 배열이 아닌 하나의 객체를 두고 그 안에    
  `데이터`와 다른 노드를 가리키는 `Reference 데이터`를 노드로 구성하여 여러 노드를 하나의 체인처럼 연결하여 사용한다.   
  
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbTGFvS%2FbtqMOxDYN8I%2F8UxB1Yew5KzHfdpiykqGJk%2Fimg.png" width="50%">
  
## LinkedList 구현(JAVA)    
<br />   

### Node 구현    

```java
class Node<E> {
  E data;
  Node<E> next;
  
  Node(E data) {
      this.data = data;
      this.next = null;
   }
}   
```   

### List Interface  
```java
public interface List<E> {
    //List에 요소 추가
    boolean add(E value);
    
    //List에 요소를 특정 위치에 추가
    void add(int index, E value);
    
    //리스트의 index 위치에 있는 요소를 삭제합니다. 
    E remove(int index);
    
    //리스트에서 특정 요소를 삭제합니다. 동일한 요소가 여러 개일 경우 가장 처음 발견한 요소만 삭제됩니다.
    boolean remove(Object value);
    
    //리스트에 있는 특정 위치의 요소를 반환합니다.
    E get(int index);

    // 리스트에서 특정 위치에 있는 요소를 새 요소로 대체합니다.
    void set(int index, E value);

    // 리스트에 특정 요소가 리스트에 있는지 여부를 확인합니다.
    boolean contains(Object value);

    // 리스트에 특정 요소가 몇 번째 위치에 있는지를 반환합니다.
    int indexOf(Object value);

    // 리스트에 있는 요소의 개수를 반환합니다.
    int size();

    // 리스트에 요소가 비어있는지를 반환합니다.
    boolean isEmpty();

    // 리스트에 있는 요소를 모두 삭제합니다.
    public void clear();
}     
```     

### 클래스 및 생성자    
```java
public class SingleLinkedList<E> implements List<E> {
    private Node<E> head;
    private Node<E> tail;
    private int size;
    
    public SingleLinkedList() {
        this.head = null;
        this.tail = null;
        this.size = 0;
     }
 }     
```    
### search() Method    
```java
private Node<E> search(int index) {
    if(index < 0 || index >= size) {
        throw new IndexOutOfBoundsException();
     }
     
     Node<E> x = head;
     
     for(int i = 0 i < index; i++) {
        x = x.next;
     }
     return x;
 }    
```    

### add() Method  
add() 메소드는 값을 추가하는 방법이 세가지가 있다.
1. 가장 앞에 추가 : addFirst(E value)
2. 가장 마지막에 추가(default) : addLast(E value)
3. 특정 위치에 추가 : add(int index, E value)

(1) addFirst()      

앞에 노드를 추가하는 방법은 아래와 같다.   
1) 새로운 노드를 생성한다.    
2) 새로운 노드를 head가 가리키는 노드와 연결해준다.
3) head가 새로 추가된 노드를 가리키도록 갱신한다.    


```java
public void addFirst(E value) {
    Node<E> newNode = new Node<E>(value);
    newNode.next = head;
    head = newNode;
    size++;
    
    
    //head가 가리킬 다음 노드가 없는 경우 = 현재 노드가 head노드 한개뿐인 경우
    // 이러한 경우, 새 노드는 새로운 노드이자 마지막 노드이다.
    if(head.next == null) {
        tail = head;
    }  
}    
```    

(2) addLast() : Default    
1) 새로운 노드를 생성한다.
2) 새로운 노드를 tail 노드 다음으로 연결해준다.
3) tail이 새로운 노드를 가리키도록 갱신해준다.   

```java
public void addLast(E value) {
    Node<E> newNode = new Node<E>(value); 
    
    if(size == 0) { // 처음 넣는 노드일 경우 addFisrt로 추가
        addFirst(value);
        return;
    }    
    
    tail.next = newNode;
    tail = newNode;
    size++;
}    
```    
(3) add()   
1) 새로운 노드를 만든다.
2) 넣으려는 위치의 노드와 이전 노드를 찾는다.
3) 이전 노드의 링크를 새로 추가하는 노드로 변경하고, 새로 추가하는 노드의 링크는 이전 노드가 가리키고 있던 노드를 가리키게 갱신한다.   

```java
public void add(int index, E value) {
    if(index < 0 || index > size) {
        throw new IndexOutOfBoundsException();
	  }
    
    // 추가하려는 index가 가장 앞에 추가하려는 경우 addFirst 호출 
    if(index == 0) {
      addFirst(value);
		  return;
	  }
    
	  // 추가하려는 index가 마지막 위치일 경우 addLast 호출
	  if (index == size) {
		  addLast(value);
		  return;
	  }

    Node<E> prev_Node = search(index - 1);
    Node<E> next_Node = prev_Node.next;
    Node<E> newNode = new Node<E>(value);
    
    prev_Node.next = null;
    prev_Node.next = newNode;
    newNode.next = next_Node;
    size++;
}    
```
 
### remove() method     
remove() 메소드는 값을 삭제하는 방법이 세가지가 있다.
1. remove()
2. remove(int index)
3. remove(Object value) 


(1) remove()  

remove는 가장 앞의 요소를 삭제한다.

1. head 노드를 찾는다.
2. head 노드를 삭제한다.
3. head를 다음에 있던 노드로 갱신한다.  

```java
pubic E remove() {
    Node<E> headNode = head;
    
    if(headNode == null) {
        throw new NoSuchElementException();
    }
    
    E element = headNode.data; // 삭제된 노드를 반환하기 위한 임시 변수
    Node<E> nextNode = head.next;
    
    head.data = null;
    head.next = null;
    
    head = nextNode;
    size--;
    
    /**
	 * 삭제된 요소가 리스트의 유일한 요소였을 경우
	 * 그 요소는 head 이자 tail이었으므로 
	 * 삭제되면서 tail도 가리킬 요소가 없기 때문에
	 * size가 0일경우 tail도 null로 변환
	 */
    if(size == 0) {
        tail = null;
    }
    
    return element;
}    
```   

(2) remove(int index)   
1. 삭제할 노드를 찾는다.
2. 삭제할 노드를 삭제한다.
3. 삭제한 노드의 앞의 노드와 뒤의 노드를 연결한다.

```java
public E remove(int index) {
 
	if (index == 0) {
		return remove();
	}
 
	if (index >= size || size < 0) {
		throw new IndexOutOfBoundsException();
  }
  
  Node<E> prevNode = search(index - 1);	// 삭제할 노드의 이전 노드 
	Node<E> removedNode = prevNode.next;	// 삭제할 노드 
	Node<E> nextNode = removedNode.next;	// 삭제할 노드의 다음 노드 
  
  E element = removedNode.data;
  
  prevNode.next = nextNode;
		 
	removedNode.next = null;
	removedNode.data = null;
	size--;
 
	return element;
}  
```    

(3) remove(Object value)    
`remove(int index)`와 동일한 방식으로 동작한다.   
다른 점은, 삭제하려는 요소가 존재하는지를 확인해야한다.    
삭제하려는 요소를 찾지 못한 경우 false를 반환하고,   
찾은 경우 `remove(int index)`와 동일한 방식으로 삭제한다.   

```java
public boolean remove(Object value) {
 
	Node<E> prevNode = head;
	boolean hasValue = false;
	Node<E> x = head;	// removedNode 
		
	// value 와 일치하는 노드를 찾는다.
	for (; x != null; x = x.next) {
		if (value.equals(x.data)) {
			hasValue = true;
			break;
		}
		prevNode = x;
	}
  
  // 만약 삭제하려는 노드가 head라면 기존 remove()를 사용 
	if (x.equals(head)) {
		remove();
		return true;
	}
  
	// 일치하는 요소가 없을경우 false 반환 
	else if (!hasValue) {
		return false;
	}
 
	else {
		// 이전 노드의 링크를 삭제하려는 노드의 다음 노드로 연결
		prevNode.next = x.next;
		
		x.data = null;
		x.next = null;
		size--;
		return true;
	}
}  
```        

### get(), set(), indexOf(), contains() Method   

1. get()    
`search()` 와 `get()` 메소드의 차이는    
`search()`는 노드를 반환하고, `get()`는 값을 반환한다.     

```java
public E get(int index) {
	return search(index).data;
}
```    
2. set()      
set 메소드는 기존에 index에 위치한 데이터를 새로운 데이터로 교체하는 것이다.    

```java
public void set(int index, E value) {
 
	Node<E> replaceNode = search(index);
	replaceNode.data = null;
	replaceNode.data = value;
}
```      

3. indexOf()   
indexOf는 사용자가 찾고자 하는 요소의 위치를 반환하는 메소드다.    
만약 찾고자하는 요소가 중복된다면, 먼저 마주치는 요소의 인덱스를 반환한다.    
주의할 점은, 객체끼리 비교할때는 동등연산자(==)가 아닌 .equals() 로 비교해야한다.   

```java
public int indexOf(Object value) {
	int index = 0;
		
	for (Node<E> x = head; x != null; x = x.next) {
		if (value.equals(x.data)) {
			return index;
		}
		index++;
	}
	// 찾고자 하는 요소를 찾지 못했을 경우 -1 반환
	return -1;
}
```       
4. contains()      
contains는 사용자가 찾고자하는 요소가 존재하는지 안하는지 반환하는 메소드이다.  
존재하면 true, 존재하지 읺으면 false   

```java
public boolean contains(Object item) {
	return indexOf(item) >= 0;
}
```       

------------------------------------------------      
### Reference     
  * [Singly LinkedList (단일 연결리스트) 구현하기](https://st-lab.tistory.com/167)
  * [LinkedList - Java 구현](https://opentutorials.org/module/1335/8857)
 
 



