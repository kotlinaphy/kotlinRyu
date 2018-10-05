# 7장 - 연산자 오버로딩과 기타 관례

## 2. 비교 연산자 오버로딩
    보충 : 코틀린에서는 산술 연산자와 마찬가지로 원시 타입 값뿐 아니라 모든 객체에 대해 비교 연산이 가능

### 2-1. 동등성 연산자: equals
- 코틀린이 `==` 연산자 호출을 equals 메소드 호출로 컴파일.
- `a == b` 라는 비교는 a가 null이 아닌 경우에만 `a.equals(b)` 호출, a가 null이면 b도 null인 경우에 결과가 true

  > a == b -> a?.equals(b) ?: (b == null)

- `===` 식별자 비교 연산자 를 사용해 equals의 파라미터가 수신 객체와 같은지 판단.
- equals 함수에는 Any에 정의된 메소드이므로 `override` 가 필요!!

### 2-2 순서 연산자: compareTo
- Comparable 에 들어있는 compareTo 메소드는 한 객체와 다른 객체의 크기를 비교해 정수로 나타내준다.
- 비교연산자 `<` `>` `<=` `>=` 는 compareTo 호출로 컴파일!!
- compareTo 가 반환하는 값은 Int
```
class Person(val firstName: String, val lastName: String) : Comparable<Person> {
  override fun compareTo(other: Person): Int {
    return compareValuesBy(this, other, Person::lastName, Person::firstName)
  }
}

>>> val p1 = Person("Alice", "Smith")
>>> val p2 = Person("Bob", "Johnson")
>>> println(p1 < p2)
false
```
