# 7장 - 연산자 오버로딩과 기타 관례

## 4. 구조 분해 선언과 Component 함수
- 구조 분해를 사용하면 복합적인 값을 분해해서 여러 다른 변수를 한꺼번체 초기화가 가능!
- 구조 분해는 무한으로 가능하지는 않다.

### 예제
```
data class NameCom(val name: String, val extension: String)

fun splitFileName(fullName: String) : NameCom {
    val result = fullName.split('.', limit = 2)
    return NameCom(result[0], result[1])
}

>>> val (name, ext) = splitFileName("developer.ryu")
>>> println(name)
>>> println(ext)

developer
ryu
```
val (name, ext) = `SOMETHING`
  > val name = SOMETHING.component1() <br/>
  > val ext = SOMETHING.component2()
  
### 4.1 구조 분해 선언과 루프
- 함수 본문 내의 선언문뿐 아니라 변수 선언이 들어갈 수 있는 곳이라면 사용 가능!!
- 특히 맵의 원소에 대해 이터레이션할 때 유용!!

### Map 사용 예제
```
fun iteratorMap(map: Map<String, String>) {
  for((key, value) in map) {
    println("@key -> @value")
  }
}
```
    참고 : 코틀린 표준 라이브러리에는 `Map`에 대한 확장 함수로 iterator 이 들어있음. 그렇기에 `Map`이 직접 이터레이션 한다.


