# Hash Map


`Hash Map`은 Map 인터페이스를 구현한 Map Collection이다.   
Map 인터페이스를 상속하기에, Map의 성질을 그대로 갖는다.   

* `Map`은 Key, Value로 구성된 Entry객체를 저장하는 구조를 가진 자료구조이다.  
* Value는 중복 값이 저장될 수 있지만, Key는 그렇지 않다. (key는 unique해야한다.)    
* 기존에 저장된 key와 동일한 key를 저장하면, 기존의 키는 사라지고 새로운 키로 대체된다.
* Hash Map 에 저장되는 key, value는 모두 Object 타입으로 저장된다. 따라서 Hash Map안에 Hash Map을 저장할 수 있다.
* Hash Map은 Hashing을 사용하기 때문에 많은 양의 데이터를 검색하는데 있어 뛰어난 성능을 보인다.
* Hash Map은 내부에 Key, Value를 지정하는 자료 구조를 가지고 있다. Hash 함수를 사용해 Key, Value가 저장되는 위치를 결정한다. 그렇기 때문에 사용자는 그 위치를 알 수 없고, 삽입하는 순서와 무관하다.
* List 처럼 저장공간을 한칸씩 늘리지 않고 2배로 늘리기 때문에, 초기 Map의 용량을 지정해주는 것이 좋다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcfpMTT%2FbtqEvxLt6qb%2FMXYNWUvXCKfRvNWjDMZoq0%2Fimg.png" width="50%">

### Hash Map 사용하기(Java)
```java
// Hash Map 선언
HashMap<String, String> map = new HashMap<String, String>();
HashMap<String, String> map2 = new HashMap<>(map); // new에서 파라미터 타입 생략 가능, map의 모든 값을 가진 HashMap생성
```    

### Hash Map의 메서드  

|Method|설명|
|---|:---|
|HashMap(int initialCapacity)|(생성자)지정한 값을 초기 용량으로 하는 객체 생성|
|HashMap(int initialCapacity, float loadFactor)|(생성자)지정한 초기 용량과 loadFactor를 가진 객체 생성|
|HashMap(Map m)|HashMap(Map m)|
|void `clear()`|모든 객체 삭제|
|Object `clone()`|객체를 복사해서 반환|
|boolean containsKey(Object key)|지정한 key가 포함되어있는지 확인|
|boolean containsValue(Object value)|지정한 value가 포함되어 있는지 확인|
|Set entrySet()|지정한 키와 값을 엔트리(key+value)의 형태로 Set에 저장해서 반환|
|Object `get(Object key)`|지정한 key의 매핑된 value를 반환.|
|Object getOrDefault(Object key, Object defaultValue)|지정한 key의 value를 반환. key를 못 찾으면 defaultValue로 지정한 객체를 반환|
|boolean `isEmpty()`|	객체가 비어있는지 확인|
|Set keySet()|저장된 모든 key가 저장된 Set을 반환|
|Object `put(Object key, Object value)`|지정한 key와 vlaue를 저장|
|void putAll(Map m)	| 지정한 Map에 저장된 모든 요소를 저장|
|Object `remove(Object key)`	|지정한 key로 저장된 객체(key+value) 제거|
|Object `replace(Object key, Object value)`	|지정한 key의 값을 지정한 value로 대체|
|boolean replace(Object key, Object oldValue, Object newValue	|지정한 key와 oldValue가 모두 일치하는 경우에만 새로운 객체newValue로 대체|
|int `size()`	|저장된 객체의 수 반환|
|Collection `values()`	|저장된 모든 값을 컬렉션 형태로 변환|     

### Hash Map 사용 예
```java
public class Main {
     public static void main(String[] args) {
        HashMap<String, Integer> map = new HashMap<>();
        
        // 1. put() 
        map.put("Apple", 1000);
        map.put("Banana", 2000);
        map.put("Pear", 500);
        
       // 2. get()
       System.out.println(map.get("Apple")); // print -> 1000
       
       // 3. values()
        System.out.println(map.values()); // [1000, 2000, 500]
       
       // 4. entrySet(), iterator()
       //HashMap에 넣은 key, value를 set에 넣고 iterator에 담아준다.
       //Iterator itr = map.entrySet().iterator(); 과 동일 
       Set<Entry<String, Integer>> set = map.entrySet();
       Iterator<Entry<String, Integer>> itr = set.iterator();
       
       while(itr.hasNext()) {
          Map.Entry<String, Integer> e = (Map.Entry<String, Integer>)itr.next();
          System.out.println("Key : "  + e.getKey() + ", Value : " + e.getValue() + "won");
       }   
 }       
```   

### JavaScript에서 Hash Map       
기본적으로 JavaScript의 `Object`는 값에 키를 할당할 수 있고, 그 키로 값을 얻을 수 있고, 삭제나, 키에 값이 존재하는 지 등을 확인하는 등 
Map과 유사하게 동작한다. 그리고 Javascript에는 원래 Map이 존재하지 않기 때문에 `Object`를 Map 대신 사용하곤 했다.     
이제 JavaScript 에도 Map이 도입이 되었고, `Object`와의 차이는 아래와 같다.       
* `Map`은 명시적으로 제공한 키 외에는 어떤 키도 가지지 않는다. 하지만, `Object`는 프로토 타입을 가지므로 기본키가 존재할 수도 있지만, 직접 제공한 키와 충돌할 가능성이 존재한다.
* `Map`의 키는 함수, 객체 등을 포함한 모든 값이 가능, `Object`의 키는 반드시 String or Symbol이어야 한다.
* `Map`의 키는 정렬됩니다. 따라서 Map의 순회는 삽입순으로 이뤄집니다. `Object`의 키는 정렬되지 않습니다.
* `Map`의 항목 수는 size 속성을 통해 쉽게 알아낼 수 있다. `Object`의 항목 수는 직접 알아내야 합니다.의 항목 수는 직접 알아내야 합니다.
* `Map`은 순회 가능하므로, 바로 순회할 수 있다.`Object`를 순회하려면 먼저 모든 키를 알아낸 후, 그 키의 배열을 순회해야 한다.
* `Map`은 잦은 키-값 쌍의 추가와 제거에서 더 좋은 성능을 보인다. `Object`는 잦은 키-값 쌍의 추가와 제거를 위한 최적화가 없다.


### Method
[JavaScript Map method](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Map) 


-----------------------------------------------------------     
### HashMap을 사용하는 문제(백준)  
* [10816번 숫자카드2](https://www.acmicpc.net/problem/10816)

-----------------------------------------------------------      
## Reference  
* [자바 HashMap 사용법 & 예제 총정리](https://coding-factory.tistory.com/556)
* [Map - HashMap](https://velog.io/@cocodori/Map)
* [Map](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Map)
