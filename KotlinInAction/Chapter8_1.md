# 8장 고차 함수: 파라미터와 반환 값으로 람다 사용
    보충 : 람다는 추상화하기 좋은 도구, 람다를 인자로 받거나 반환하는 함수인 고차 함수

## 1 고차 함수 정의
- `고차 함수`는 다른 함수를 인자로 받거나 함수를 반환하는 함수.

### 1.1 함수 타입

### 1.2 인자로 받은 함수 호출
```
fun twoAndThree(operation: (Int, Int) -> Int) {
  val result = operation(2,3)
  println("The result is $result")
}

>>> twoAndThree(a,b -> a+b)
--> The result is 5
>>>twoAndThree(a,b -> a*b)
--> the result is 6
```

### 1.3 자바에서 코틀린 함수 타입 사용

### 1.4 디폴트 값을 지정한 함수 타입 파라미터나 널이 될 수 있는 함수 타입 파라미터

... 어렵다..
