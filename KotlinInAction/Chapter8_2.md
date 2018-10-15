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
