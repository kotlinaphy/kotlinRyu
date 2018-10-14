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

- 예제3
```
fun String.filters(predicate: (Char) -> Boolean): String {
    val sb = StringBuilder()

    for (index in 0 until length) {
        val element = get(index)
        if (predicate(element)) sb.append(element)
    }

    return sb.toString()
}

>>> println("ab23fl2k".filters { it in 'a'..'z' })
결과 : abflk
```
- String 의 확장함수로 만든 filters()

### 1.3 자바에서 코틀린 함수 타입 사용
- 컴파일된 코드 안에서 함수 타입은 일반 인터페이스로 바뀐다.

### 1.4 디폴트 값을 지정한 함수 타입 파라미터나 널이 될 수 있는 함수 타입 파라미터
- 파라미터를 함수 타입으로 선언할 때도 **디폴트 값**을 정하는 것이 가능!!
```
fun <T> Collection<T>.jointToString (separator: String = ",", prefix: String = "", postfix: String = "",
                                      transform: (T) -> String = {it.toString()}
                                      ): String {
    val result = StringBuilder(prefix)
    
    for ((index, element) in this.withIndex()) {
        if (index > 0) result.append(separator)
        result.append(transform(element))
    }
    
    result.append(postfix)
    return result.toString()
}

>>> val letters = listOf("Alpha", "Beta")
>>> println(letters.joinToString())
결과 : Alpha, Beta

>>> println(letters.joinToString(it.toLowerCase()))
결과 : alpha, beta
```
- [withIndex()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-iterator/index.html) 함수 참조

### 1.5 함수를 함수에서 반환
- 보통은 함수가 함수를 반환할 필요가 있는 경우보다, 함수가 함수를 인자로 받아야 할 필요가 있는 경우가 더 많음.
```
data class Person(val firstName: String, val lastName: String, val phoneNumber: String?)

class ContactListFilters {
  var prefix: String = ""
  var onlyWithPhoneNumber: Boolean = false
  
  fun getPredicate(): (Person) -> Boolean {
    val startsWithPrefix = { p: Person ->
      p.firstName.startsWith(prefix) || p.lastName.startsWith(prefix)
    }
    
    if (!onlyWithPhoneNumber) {
      return startsWithPrefix
    }
    
    return { startsWithPrefix(it) && it.phoneNumber != null}
  }
}

>>> val contacts = listOf(Person("Dmitry", "Jemerov", "123-4567")
                      , Person("jihoon", "ryu", "000-234123")
                      , Person("James", "Dib", "999-000123"))
>>> val contactListFilters = ContactListFilters()
>>> with (contactListFilters) {
      prefix = "ryu"
      onlyWithPhoneNumber = true
    }
>>> println(contacts.filter(contactListFilters.getPredicate()))
결과 : [Person(firstName=jihoon, lastName=ryu, phoneNumber=000-234123)]

```

### 1.6 람다를 홀용한 중복 제거
- `함수 타입`과 `람다 식`은 재활용하기 좋은 코드를 만들 때 쓸 수 있는 훌룡한 도구!!
