# 8장 고차 함수: 파라미터와 반환 값으로 람다 사용

## 3 고차 함수 안에서 흐름 제어

### 3.1 람다 안의 return문: 람다를 둘러싼 함수로부터 반환

### 3.2 람다로부터 반환: 레이블을 사용한 return
- 람다 식에서도 `로컬 return` 을 사용할 수 있고, 람다 안에서 `로컬 return` 은 for 루프의 **break** 와 비슷한 역할.
- `로컬 return` 과 `넌로컬 return` 을 구분하기 위해 레이블(label)을 사용.

### 3.3 무명 함수: 기본적으로 로컬 return
- 무명 함수는 코드 블록을 함수에 넘길 때 사용할 수 있는 다른 방법.
- 무명 함수는 일반 함수와 비슷해 보이나 `함수 이름`이나 `파라미터 타입`을 **생략**할 수 있는 차이가 있다.
- filter에 무명 함수 넘기기
```
people.filter(fun (person): Boolean {
  return person.age < 30
})
```
- 식을 본문으로 하는 무명 함수의 반환 타입은 생략이 가능.
```
people.filter(fun (person) = person.age > 30)
```

[8장 고차 함수 책 내용 요약페이지 링크(블로그)](http://devuryu.tistory.com/264)
