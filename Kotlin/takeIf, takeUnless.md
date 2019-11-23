# takeIf, takeUnless 함수는 언제 사용할까

Kotlin의 표준함수에는 `if` 문과 다를바 없어보이는 `takeIf` 와 `takeUnless` 함수가 있다.

**(모든 `if` 문을 대체하는 극단적인 사용법은 절대 추천하지 않는다고 한다.)**

---

## 살펴보기
먼저 `takeIf` 함수의 정의부터 살펴보자.
```kotlin
public inline fun <T> T.takeIf(predicate: (T) -> Boolean): T?
    = if (predicate(this)) this else null
```
- **T** 의 확장함수이며, 즉 **T.takeIf** 로 사용할 수 있다.
- **takeIf** 의 조건 함수 **predicate** 는 파라미터로 **T** 객체를 전달 받는다.
- **takeIf** 의 조건 함수 **predicate** 의 결과에 따라 **T** 객체인 자기 자신 **(this)** 이나 **null** 을 반환한다.

```kotlin
public inline fun <T> T.takeUnless(predicate: (T) -> Boolean): T?
    = if (!predicate(this)) this else null
```
- **takeUnless** 함수는 **takeIf** 함수와 반대로 동작한다.
- **takeUnless** 는 조건함수가 **true** 일때 **null** 을 반환하고, **false** 일때 자기 자신 **(this)** 을 반환한다.

> 간단히 말하자면, takeIf()함수는 null 이 아닌 객체에서 호출될 수 있고,
>
> predicate(Boolean을 리턴하는) 함수를 인자로 받는다.
>
> 만약 predicate가 true를 반환하는 식이라면, takeIf()는 null이 아닌 그 객체를 리턴할 것이고,
>
> predicate가 만족되지 않아 false를 반환한다면 null을 리턴할 것이다.

아래의 함수를
```kotlin
return if (x.isValid()) x else null
```
아래처럼 사용할 수 있다.
```kotlin
return x.takeIf{ it.isValid() }
```

> takeUnless()함수는 반대다. 
>
> predicate함수가 false를 반환하면 null이 아닌 객체를 리턴하고,
>
> true를 반환하면 null을 반환한다.

아래의 함수를
```kotlin
return if (!x.isError()) x else null
```
아래처럼 사용할 수 있다.
```kotlin
return x.takeUnless{ it.isError() }
```
---

## 사용법
위의 특징들을 바탕으로 `if` 문 코드에 상응하는 `takeIf` 함수의 사용법을 알 수 있다.

### T의 확장함수(즉, T.takeIf로 사용할 수 있다.)
`null`여부를 체크하고 해당 결과에 따라 작업을 수행하는 케이스에서 이점을 가진다.
```kotlin
// 원본 코드
if (someObject != null && status) {
    doThis()
}

// 개선된 코드
someObject?.takekIf{ status }?.apply{ doThis() }
```

### takeIf의 조건 함수 predicate는 파라미터로 T객체를 전달 받는다.
`predicate` 함수의 파라미터로 `T` 객체를 사용할때, 더 단순한 코드를 작성할 수 있다.
```kotlin
// 원본 코드
if (someObject != null && someObject.status) {
    doThis()
}

// 나아진 코드
if( someObject?.status == true) {
    doThis()
}

// 개선 된 코드
someObject?.takeIf{ it.status }?.apply{ doThis() }
```
나아진 코드 예시처럼 작성할수도 있지만, 원본 코드에서는 쓰지않던 `true` 와의 비교가 필요하다.

이상적인 코드라고는 할 수가 없다고 한다.

### takeIf의 조건함수 predicate의 결과에 따라 T객체인 자기 자신(this)이나 null을 반환한다.
`takeIf` 의 조건함수인 `predicate` 의 결과가 참일때 자기 자신 **(this)** 을 반환 하므로

이를 이용해서 연산을 연결해서 처리하는 체이닝 기법을 사용할 수 있다.
```kotlin
// 원본 코드
if (someObject != null && someObject.status) {
    someObject.doThis()
}

// 개선 된 코드
someObject?.takeIf{ it.status }?.doThis()
```

또는 `takeIf` 함수가 조건이 만족하지 않으면 `null` 을 반환하는 특성을 이용하여

어떤 데이터를 반환하거나 엘비스 연산자 **(?:)** 를 사용해서 함수를 종료하는 방법으로 사용할 수 있다.
```kotlin
// takeIf의 조건이 만족되지 않으면 에러를 던진다.
val index = input.indexOf(keyword).takeIf { it >= 0 } ?: error("ERROR")

// takeIf의 조건이 만족되지 않으면 return 하여 종료 합니다.
val outFile = File(outputDir.path).takeIf { it.exists() } ?: return false
```
---

## 주의할 점
다음의 코드와 같은 경우를 주의해야 한다..
```kotlin
// 구문은 맞지만 논리적으로는 틀렸다.
someObject?.takeIf{ status }.apply{ doThis() }
// 올바른 예시. (apply 앞의 안전한 호출 연산자 ?. 여부에 주의.)
someObject?.takeIf{ status }?.apply{ doThis() }
```
첫번째 예시에서 `apply` 함수는 `takeIf` 함수가 `null` 을 반환 해도 호출되기 때문에

`status`의 참, 거짓의 여부에 상관없이 `doThis()` 함수가 실행된다. 

**(이 예시에서 `doThis()` 함수는 `someObject` 의 함수가 아니다.)**

`takeIf` 함수의 뒤에 존재하는 안전한 호출 연산자 **?.** 는 작은 차이지만 논리적으로 매우 중요하다.

---

## takeIf()와 takeUnless()가 독이 되는 경우

### 차이점 (연산의 순서에 따른 초과 연산 -> 그에 따른 부수효과로 이어진다.)
아래의 코드를 보자.
```kotlin
return if (x.isValid()) doWorkWith(x) else null
위의 코드를 아래와 같이 쓰고싶을 것이다.
```kotlin
return doWorkWith(x).takeIf { x.isValid() }
```
그러나 위의 코드를 실행 할 경우 에러가 발생한다.

그 이유는 `doWorkWith(x)` 함수가 `predicate` 평가 시점 이전에  호출되기 때문이다.

**(즉,predicate가 false일 때 조차 불필요하게 연산을 한다.)**

`x` 가 유효한지 유효하지 않은지 판단하기 이전에 `doWorkWith(x)` 함수가 호출되는 셈이다.

만약 `doWorkWith()` 함수가 오직 유효한 입력값에서만 동작하는 함수라면 에러가 난다.

---

## takeIf()와 takeUnless()가 약이 되는 경우

### 객체가 식이 아닐 때
`takeIf()` 함수를 호출하는 것이 그저 값 **(객체)** 일 때 문제를 피할 수 있다.

식에 대해서 `takeIf()` 함수를 호출한다면 에러를 유발하기 쉽다.

### 어떤 함수가 특정 객체의 조건부로 호출될 때
```kotlin
return if (someString.isNotBlank()) {
    someMoreWork(someString)
} else {
    null
}
```
위의 코드는 아래처럼 바뀔 수 있다.
```kotlin
return someString.takeIf { it.isNotBlank() }?.let { someMoreWork(it) }
```
