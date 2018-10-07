# 7장 - 연산자 오버로딩과 기타 관례

## 3. 컬렉션과 범위에 대해 쓸 수 있는 관례
    보충 : 컬렉션을 다룰 때 가장 많이 쓰는 연산은 인덱스를 사용해 원소를 읽거나 쓰는 연산과 어떤 값이 컬렉션에 속해있는지 검사하는 연산.
    이런 연산들을 연산자 구문으로 사용이 가능!!

### 3-1. 인덱스로 원소에 접근: get과 set
- 원소에 접근할때의 각괄호`[]`에 대응하는 함수는 get 이다.
```
operator fun Point.get(index: Int): Int {
  return when(index) {
    0 -> x
    1 -> y
    else ->
      throw INdexOutOfBoundsException("Invalid coordinate $index")
  }
}

>>> val p = Point(10, 20)
>>> println(p[1])
->20
```

### 3-2. in 관례
- in 은 객체가 컬렉션에 들어가있는지 검사.
- in 연산자와 대응하는 함수는 contains 이다.

### 3-3. rangeTo 관례
- 범위를 만들려면 `..`구문을 사용해야 한다.


### 3-4. for 루프를 위한 iterator 관례
```
operator fun CharSequence.iterator(): CharIterator

>>> for (c in "abc") {}
```
