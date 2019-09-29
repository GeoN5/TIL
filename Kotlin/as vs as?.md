# as vs as?

## as

```kotlin
val str: String = a as String // Unsafe casting
val str2: String? = b as String?
```
- **(캐스팅 할 대상)** `as` **(캐스팅 Type)** 형식으로 쓰인다.
- 첫 번째 줄은 `a`를 `String` Type으로 캐스팅하여 `str`객체에 대입한다.
- Compile 오류는 발생하지 않지만 만일 Runtime 시에 캐스팅이 불가능한 Type일 경우에 `Exception`이 발생한다.
- 위와 같은 캐스팅 방법을 **Unsafe 캐스팅**이라고 한다. **(** 만약 `a`가 `null`일 경우에 `TypeCastException`이 발생한다. **)**
- **Nullable한 값**을 캐스팅해야 할때는 두 번째 줄과 같이 캐스팅 Type 뒤에 `?`연산자를 이용한다.
- `b`가 `null`일 경우 캐스팅이 되지 않고 `str2`에 `null`이 대입되게 된다.
---

## as?

```kotlin
val str: String? = a as? String
```
- 그냥 `as` 키워드는 **Unsafe 캐스팅** 이라고 했다. **(** Rumtime 시에 캐스팅이 불가능하면 `Exception`이 발생하기 때문이다. **)**
- 캐스팅 코드에서 `Exception`이 발생하는 것을 막으려면 `as?`를 사용하면 된다.
- 만약 `a`가 `String` Type으로 캐스팅이 가능하지 않다면 `Exception`이 발생하지 않고 `str`에 `null`값이 대입된다.
- `str`은 Nullable 해야 하므로 `String?` Type으로 선언 되어야 한다.
