# 7장 - 연산자 오버로딩과 기타 관례

## 5. 프로퍼티 접근자 로직 재활용: 위임 프로퍼티
    위임 : 객체가 직접 작업을 수행하지 않고 다른 '도우미 객체'가 그 작업을 처리하게 맡기는 디자인 패턴!!
    -> '도우미 객체'를 '위임 객체'라고 부른다. 

### 5.1 위임 프로퍼티
```
class Foo {
    var p: Type by Delegate()
}
```
- 위의 예제에서 Delegate 클래스의 인스턴스를 위임 객체로 사용.
- by 뒤에 있는 식을 계산해서 위임에 쓰일 객체를 얻음.
- 프로퍼티 위임 관례를 따르는 클래스는 getValue 와 setValue 메소드를 제공해야 함.

- 간단한 코드
```
class Delegate {
    operator fun getValue(...) { ... }
    operator fun setValue(..., value: Type) { ... }
}

class Foo {
    var p: Type by Delegate()
}

>>> val foo = Foo()
>>> val oldValue = foo.p
>>> foo.p = newValue
```

### 5.2 위임 프로퍼티 사용: by lazy()를 사용한 프로퍼티 초기화 지연
    지연 초기화는 객체의 일부분을 초기화하지 않고 남겨뒀다가 실제로 그 부분의 값이 필요할 경우 초기화!!

```
class Person(val name: String) {
    private var _emails: List<Email>? = null
    val emails: List<Email>
        get() {
            if (_emails == null) {
                _emails = loadEmails(this)
            }
            
            return _emails!!
        }
}
```
를  아래와 같이 구현이 가능하다.

```
class Person(val name: String) {
    val emails by lazy{ loadEmails(this) }
}
```
- lazy 함수는 getValue 메소드가 들어있는 객체 반환
- lazy 함수를 by 키워드와 함께 사용해 위임 프로퍼티 만들 수 있다.
- lazy 함수의 인자는 값을 초기화할 때 호출할 람다
- lazy 함수는 기본적으로 스레드 안전하다.

### 5.3 위임 프로퍼티 구현
