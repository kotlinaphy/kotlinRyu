# 8장 고차 함수: 파라미터와 반환 값으로 람다 사용

## 2 인라인 함수: 람다의 부가 비용 없애기
    inline 변경자를 어떤 함수에 붙이면 컴파일러는 그 함수를 호출하는 모든 문장을 함수 본문에 해당하는 바이트코드로 바꿔치기 해준다.

### 2.1 인라이닝이 작동하는 방식
- 어떤 함수를 `inline`으로 선언하면 그 함수의 본문이 `인라인`된다.</br>
    > 함수를 호출하는 코드를 함수를 호출하는 바이트코드 대신에 함수 본문을 번역한 바이트코드로 컴파일한다는 뜻.

- 예제1
```
inline fun <T> synchronized(lock: Lock, action: () -> T): T {
  lock.lock()
  try {
    return action()
  }
  finally {
    lock.unlock()
  }
}

val l = Lock()
syncronized(1) {
  // ....
}
```
-> 코틀린 표준 라이브러리는 아무 타입의 객체나 인자로 받을 수 있는 synchronized 함수를 제공한다.

- 인라인 함수를 호출하면서 람다를 넘기는 대신에 함수 타입의 변수를 넘길 수도 있다.
```
class LockOwner(val lock: Lock) {
  fun runUnderLock(body: () -> Unit) {
    synchronized(lock, body)
  }
}
```

### 2.2 인라인 함수의 한계
- 인라이닝을 하는 방식으로 인해 람다를 사용하는 모든 함수를 인라이닝할 수는 없다.
- 일반적으로 인라인 함수의 본문에서 람다 식을 바로 호출하거나 람다 식을 인자로 전달받아 바로 호출하는 경우에는 그 람다를 인라이닝할 수 있다.
- `고차 함수`는 함수형 프로그래밍에 중요한 기법이나 `고차 함수`에 람다 함수를 전달하고 이 람다 함수를 이용하는 코드가 많아져서 런타임 때 성능 문제가 발생할 수 있다.
- 컴파일 단계에서 `inline`으로 정의한 함수의 내용이 호출되는 곳에 정적으로 포함되므로 런타임 때 함수 호출이 그만큼 줄고 성능에 도움이 된다.

### 2.3 컬렉션 연산 인라이닝
- 코틀린의 filter 함수는 인라인 함수이다.
```
data calss Person(val name: String, val age: Int)
val people = listOf(Person("Jihoon", 32), Person("DongHwa", 28))
>>> println(people.filter{it.age > 30})
결과 : [Person(name=Jihoon, age=32)]
```

- 인라인 함수를 연속해서 쓰면 부가 비용이 든다. 그렇기에 asSequence 를 활용하면 된다. 다만, 컬렉션의 크기가 클 경우만 한해서 사용.
```
>>> println(people.filter{it.age > 30}.map(Person::name))
```
-> 위의 함수는 2개의 인라인 함수를 사용했다. 두 함수의 본문은 인라이닝되며, 추가 객체나 클래스 생성은 없다. 다만 특징으로는 리스트를 걸러낸 결과를 저장하는 중간 리스트를 만든다.

### 2.4 함수를 인라인으로 선언해야 하는 경우
- `inline`키워드로 인해서 성능이 향상되길 원한다면 람다를 인자로 받는 함수에만 사용해야 한다.

### 2.5 자원 관리를 위해 인라인된 람다 사용
    자원 : 파일, 락, 데이터베이스 트랜잭션 등 여러 다른 대상을 가리킴.
- 람다로 중복을 없앨 수 있는일반적인 패턴 중 한 가지는 어떤 작업을 하기 전에 자원을 획득하고 작업을 마친 후 자원을 해제하는 자원 관리!
> 일반적으로 try/finally문 방법 : try블록을 시작하기 직전에 자원 획득, finally 블록에서 자원 해제

- 코틀린에서 제공하는 withLock 함수는 Lock 인터페이스의 확장 함수.
```
val l: Lock = ...
l.withLock { /.../ }

코틀린 라이브러리의 withLock 함수 정의
fun<T> Lock.withLock(action: () -> T): T {
    lock()
    
    try {
        return action()
    } finally {
        unlock()
    }
}
```



