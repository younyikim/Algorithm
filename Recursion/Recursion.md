# Recursion
`Recursion`은 `재귀`라고도 하며, 메서드 내에서 자기 자신을 반복적으로 호출하는 것을 의미한다.     

* 문제를 해결하기 위해서 원래 범위의 문제에서 더 작은 범위의 하위 문제를 해결함으로써 전체 문제를 해결하는 방식이다.    

* Recursion 함수를 사용하는 경우, 함수 내에서 자기 자신을 계속 호출하기 때문에  
반복문이 무한 루프에 빠지듯, 재귀 함수도 무한 재귀에 빠질 수 있다.   
이를 방지하기 위해선 반드시 종료 구간을 정해주는 것이 중요하다. 

* Recursion은 동일한 동작을 하는 반복문으로 대체할수 있지만, 반복문보단 성능이 좋지 못하다.     

* Recursion은 `단점`은, 코드가 간결하지만 한번에 이해하기는 어렵다는 점이있다.

## Recursion의 예시
1. 팩토리얼
2. 피보나치 수열
3. 제곱
4. etc..

재귀 함수의 기본 구조

```java
returnType methodName() {
    //execute code
    methodName();
}    
```    

(예시) 1~10 사이의 숫자를 모두 더하기
아래의 예시에서 종료 조건은 k가 0이하인 경우이다.   


```java
public class Main {
    public static void main(String[] args) {
      int result = sum(10);
      System.out.println(result);
    }
    
    public static int sum(int k) {
        if(k > 0) { // 중지 조건, 탈출 조건
          return k + sum(k - 1);
        } else {
          return 0;
        }
      }
}      
```    
이 프로그램은 아래와 같이 동작하게 된다.   

`10 + sum(9)`  

`10 + (9 + sum(8))`   

`10 + 9 + (8  + sum(7))`   
....    

`10 + 9 + 8 + 7 + 6 + 5 + 4 + 3 + 2 + 1 + sum(0)`     

`10 + 9 + 8 + 7 + 6 + 5 + 4 + 3 + 2 + 1 + 0`      


## Recursion을 이용하는 문제(백준)
* [백준 단계별 풀이 10단계 - 재귀](https://www.acmicpc.net/step/19)   

------------------------------------------      

### Reference  
* [Java Recursion](https://www.w3schools.com/java/java_recursion.asp_)
