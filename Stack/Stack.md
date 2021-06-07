# Stack

Stack은 '쌓다', '더미'의 의미를 가진다.
`LIFO`(Last In First Out) 의 형태를 가진다.    

<img src="https://upload.wikimedia.org/wikipedia/commons/9/9f/Stack_data_structure.gif" width="20%">   

`push()` : 맨 위에 데이터를 삽입     
`pop()` : 맨 위의 데이터를 꺼냄   

### Stack의 특징
1. 먼저 들어간 데이터가 나중에 나온다. `LIFO`
2. interrupt 처리, 수식의 계산, 서브루틴 복귀 번지 저장 등에 사용된다.
3. 깊이 우선 탐색(DFS)에서 사용된다.
4. 재귀 함수(Recursion)을 호출할때 사용된다.


### Stack의 사용법(JAVA)   
```java
import java.util.Stack;
Stack<Integer> stack = new Stack<>();
```   
  
Stack 클래스는 List 컬랙션 클래스의 Vector 클래스를 상속받아, 전형적인 Stack 메모리 구조의 클래스를 제공한다.     
Stakc 클래스는 내부에서 최상위 타입 배열인 Object[] 배열을 사용하여 데이터를 관리한다.   


------------------------------------------------

|**Method**|Explain|
|:---:|:---|
|boolean empty() |해당 스택이 비어 있으면 true를, 비어 있지 않으면 false를 반환함.|
|E `pop()`|	해당 스택의 제일 상단에 있는(제일 마지막으로 저장된) 요소를 반환하고, 해당 요소를 스택에서 제거함.|
|E `peek()`| 해당 스택의 제일 상단에 있는(제일 마지막으로 저장된) 요소를 반환함.|
|E `push(E item)`|해당 스택의 제일 상단에 전달된 요소를 삽입함.|
|int search()|해당 스택에서 전달된 객체가 존재하는 위치의 인덱스를 반환함. 이때 인덱스는 제일 상단에 있는(제일 마지막으로 저장된) 요소의 위치부터 0이 아닌 1부터 시작함.|  

------------------------------------------------   

### Stack 구현(JAVA)   
위에서 언급했듯 Stack은 Vector 클래스를 상속한다. 그렇기 때문에 Vector 클래스의 메소드를 사용할 수 있다.   

Vector 클래스는 ArrayList와 구조와 매우 유사하다.   


  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdSJsU0%2FbtqNQwjCXSN%2FECnUFjavCCLUYEpEThbIpK%2Fimg.png" width="30%"> &<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Ft814C%2FbtqNRIYaz3z%2FNH37SKmCeHCb5wm6r6JsQ1%2Fimg.png" width="30%">   
  
 ### Stack Interface
 ```java
 public interface StackInterface<E> {
  //Stack의 맨 위에 요소를 추가
  E push(E item);
  
  //Stack의 맨 위의 요소를 제거하고, 제거된 요소를 반한
  E pop();
  
  //Stack의 맨 위 요소를 제거하지 않고, 반환
  E peek();
  
  int search();
  
  int size();
  
  void clear();
  
  boolean empty();
}  
 ```   
 
 ### Stack Class 및 생성자  
 ```java
  public class Stack<E> implements StackInterface<E> {
    private static final int DEFAULT_CAPACITY = 10; // 최소 용적 크기
    private static final Object[] EMPTY_ARRAY = {};
    
    private Object[] array;
    private int size;
    
    //생성자 1
    public Stack() {
      this.array =  EMPTY_ARRAY;
      this.size = 0;
    }  
    
    // 생성자2 (초기 공간 할당 O) 
	  public Stack(int capacity) {
		  this.array = new Object[capacity];
		  this.size = 0;
	  }
 }   
 ```   
 
 ### resize() method   
 `resize()` 메소드는 동적할당을 위한 메소드이다.   
 들어오는 데이터의 개수에 따라 최적화된 용적(크기)를 갖는 것이 이상적이다.  
 
 따라서 `resize()` 메소드는 size가 용적에 얼마만큼 차있는지 확인하고,   
 적절할 크기에 맞게 배열의 용적을 변경하는 것이 이상적이다.   
 
 ```java
 private void resize() {
    //empty array
    if(Arrays.equals(array, EMPTY_ARRAY)) {
        array = new Object[DEFAULT_CAPACITY];
        return;
    }
    
    int arrayCapacity = array.length;
    
    //용적이 가득 찬 경우
    if(size == arrayCapacity) {
      int newSize = arrayCapacity * 2;
      
      array = Arrays.copyOf(array, newSize);
      return;
    }
    
    // 용적의 절반 미만 차지하고 있는 경우
    if(size < (arrayCapacity / 2)) {
      int int newCapacity = (arrayCapacity / 2);
		
	  	array = Arrays.copyOf(array, Math.max(DEFAULT_CAPACITY, newCapacity));
		  return;
     }
 }    
 ```   
 
 ### push() method   
 ```java
 public E push(E item) {
    if(size == array.length) {
      resize();
    }
    
    array[size++] = item;
    
    return item;
 }   

 ```   
 
 ### pop() method   
 ```java
 public E pop() {
    //empty
    if(size == 0) {
      throw new EmptyStackException();
    }
    
    E obj = (E) array[size - 1];
    
    // delete element
    array[size - 1] = null; 
    
    size --;
    resize();
    
    return obj;
 }   
 ```      
  ### peek() method   
 ```java
 public E peek() {
    //empty
    if(size == 0) {
      throw new EmptyStackException();
    }
    
    return (E) array[size - 1];
 }   
 ```
 
 ### search() method    
 `search()` 메소드는 찾으려는 데이터가 상단의 데이터로부터 얼만큼 떨어져 있는가에 대한 상대적 위치 값을 반환하는 메소드이다.   
 Top으로 부터 떨어져있는 거리를 반환하면 원하는 결과를 얻을 수 있다.    
 
 
 <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcuMLNe%2FbtqNOJjxwr4%2FeYS619UK8UexzM3Le8Zvc0%2Fimg.png" width="30%">   
 
 ```java
 public int search(Object value) {
  for(int i = size - 1; i >= 0; i--) {
    if(array[i].equals(value)) {
      retrun size - i;
    }
  }
  //찾을 수 없는 경우
  return -1;
}  
 ```   
 
 ### size(), empty(), clear() method   
 ```java
 public int size() {
    return size;
 }
 
 public boolean empty() {
    return size == 0;
 }   
 
 public void clear() {
    for(int i = 0; i < size; i++) {
      array[i] = null;
     }
     size = 0;
     resize();
  }   
 ```    
 
 --------------------------------------------------    
 
 ### JavaScript로 Stack 구현하기   
 Queue와 동일하게 Stack도 Array 객체가 제공하는 다양한 메서드를 사용하여 쉽게 구현할 수 있다.   
 
 Stack은 `push()`와 `pop()` 메서드를 사용하여 쉽게 사용할 수 있다.   
 ```javascript
 let stack = [];
 
 stack.push(1); // 1 추가 , stack status : [1]
 stack.push(2); // 2 추가 , stack status : [2 , 1]
 
 console.log(stack.pop()); // 2 출력, stack status : [1]
 ```   
 
 ES6 OOP 형식으로 Stack을 구현할 수도 있다.
 ```javascript
 import LinkedList from '../linked-list/LinkedList';

export default class Stack {
  constructor() {
    this.linkedList = new LinkedList();
  }

  /**
   * @return {boolean}
   */
  isEmpty() {
    return !this.linkedList.tail;
  }

  /**
   * @return {*}
   */
  peek() {
    if (!this.linkedList.tail) {
      return null;
    }

    return this.linkedList.tail.value;
  }

  /**
   * @param {*} value
   */
  push(value) {
    this.linkedList.append(value);
  }

  /**
   * @return {*}
   */
  pop() {
    const removedTail = this.linkedList.deleteTail();
    return removedTail ? removedTail.value : null;
  }

  /**
   * @return {*[]}
   */
  toArray() {
    return this.linkedList
      .toArray()
      .map(linkedListNode => linkedListNode.value)
      .reverse();
  }

  /**
   * @param {function} [callback]
   * @return {string}
   */
  toString(callback) {
    return this.linkedList.toString(callback);
  }
}
 ```     
 
 --------------------------------------
 ### Stack을 사용한 문제들 (백준)   
 * [1874번 스택 수열](https://www.acmicpc.net/problem/1874)
 * [17298번 오큰수](https://www.acmicpc.net/problem/1874)
 * [10828번 스택 - 기초](https://www.acmicpc.net/problem/10828)  


--------------------------------------   
Reference     
* [How do you implement a Stack and a Queue in JavaScript?](https://stackoverflow.com/questions/1590247/how-do-you-implement-a-stack-and-a-queue-in-javascript)
* [Stack (스택) 구현하기](https://st-lab.tistory.com/174?category=856997)
* [Stack과 Queue](http://tcpschool.com/java/java_collectionFramework_stackQueue)
* [자바 Stack 클래스 사용법 & 예제 총정리](https://coding-factory.tistory.com/601?category=758267)
* [File:Stack data structure.gif](https://commons.wikimedia.org/wiki/File:Stack_data_structure.gif)
 
