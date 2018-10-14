# 8장 고차 함수: 파라미터와 반환 값으로 람다 사용
- 람다는 추상화하기 좋은 도구
- 람다를 인자로 받거나 반환하는 함수인 고차 함수

## 1 고차 함수 정의 (High-Oder Function)
- `고차 함수`는 `람다`나 `함수 참조`를 **인자로 넘기거나** `람다`나 `함수 참조`를 **반환하는** 함수다.
- 일반 함수는 특정 타입의 매개 변수를 전달받거나 결과를 특정 타입의 데이터로 반환(혹은 Unit), 매개변수와 반환에 함수를 이용할 수 있는 특징이 있다.

### 1.1 함수 타입
```
val sum: (Int, Int) -> Int = {x, y -> x + y}
```
> (Int, Int) : **파라미터 타입** </br>
> Int : **반환 타입**



### 1.2 인자로 받은 함수 호출
- 예제1
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

- 예제2
```
fun ruyFun(x1: Int, argFun: (Int) -> Int) {
    val result = argFun(x1)
    println("x1 : $x1, funResult : $result"
}

ruyFun(10, {x, -> x * x})
결과 : "x1 : 10, funResult : 100"
```


### 1.3 자바에서 코틀린 함수 타입 사용

### 1.4 디폴트 값을 지정한 함수 타입 파라미터나 널이 될 수 있는 함수 타입 파라미터

... 어렵다..
