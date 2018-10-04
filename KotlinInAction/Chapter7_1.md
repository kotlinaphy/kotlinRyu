# 7장 - 연산자 오버로딩과 기타 관례
#### 특징 : 어떤 언어 기능과 미리 정해진 이름의 함수를 연결해주는 기법을 **관례** 라고 한다

## 1. 산술 연산자 오버로딩
	#### 자바에서는 원시 타입에 대해서만 산술 연산자 사용이 가능, 추가로 String에 대해 + 연산자를 사용 가능.

	#### 오버로딩 가능한 이항 산술 연산자
	식 | 함수 이름
	------------ | -------------
	a * b | times
	a / b | div
	a % b | mod(1.1부터 rem)
	a + b | plus
	a - b | minus

	### 1-1. 이항 산술 연산 오버로딩
	#### 연산자 오버로딩
	'''
		data class Point(val x: Int, val y: Int) {
			operator fun plus(other: Point): Point {
				return Point(x + other.x, y + other.y)
			}
		}
		
		>>> val p1 = Point(10, 20)
		>>> val p2 = Point(30, 40)
		>>> println(p1 + p2)
	'''
	
	#### 연산자를 확장 함수로 정의
	'''
		operator fun Point.plus(other: Point): Point {
			return Point(x + other.x, y + other.y)
		}
	'''
	
	#### 연산자를 오버로딩 하는 함수 앞에는 **operator** 가 있어야 한다.

## 2. 복합 대입 연산자 오버로딩
	#### '+=', '-=' 등의 연산자는 **복합 대입 연산자**라고 불린다
	#### 이론적으로 plus(+), plussAssign(+=) 양쪽으로 컴파일이 가능하나, 일관성있게 클래스를 설계하기 위해서 두가지의 연산을 동시에 정의하지 않는게 좋다.
	
## 3. 단항 연산자 오버로딩
#### 오버로딩할 수 있는 단항 산술 연산자
식 | 함수 이름
------------ | -------------
+a | unaryPlus
-a | unaryMinus
!a | not
++a, a++ | inc
--a, a-- | dec
