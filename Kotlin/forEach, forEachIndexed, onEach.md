# Kotlin forEach, forEachIndexed, onEach

## forEach

```kotlin
fun main() {
    val list = listOf(1, 2, 3, 4, 5, 6)
    
    //forEach: 각 요소를 람다식으로 처리
    list.forEach{
        print("$it ")
    }
}

출력 : 1 2 3 4 5 6
```

---

## forEachIndexed

```kotlin
fun main() {
    val list = listOf(1, 2, 3, 4, 5, 6)

    //forEachIndexed: index 포함
    list.forEachIndexed { index, value ->
        println("index[$index] : $value")
    }
}

출력 :
index[0] : 1
index[1] : 2
index[2] : 3
index[3] : 4
index[4] : 5
index[5] : 6
```

forEach는 각 요소를 람다식으로 처리한 후 컬렉션을 반환하지 않는다.

onEach를 사용하면 각 요소를 람다식으로 처리하고 각 컬렉션을 반환 받을 수 있다.

---

## onEach

```kotlin
fun main() {
    val list = listOf(1, 2, 3, 4, 5, 6)

    val returnedList = list.onEach { print("$it ") }
    print("\n$returnedList")
}

출력 : 
1 2 3 4 5 6 
[1, 2, 3, 4, 5, 6]
```

```kotlin
fun main() {
    val map = mapOf(1 to "Android", 2 to "Java", 3 to "Kotlin")

    val returnedMap = map.onEach {
        println("Key : ${it.key}, Value : ${it.value}")
    }
    print(returnedMap)
}

출력 : 
Key : 1, Value : Android
Key : 2, Value : Java
Key : 3, Value : Kotlin
{1=Android, 2=Java, 3=Kotlin}
```

