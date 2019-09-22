# 코틀린에서 제공하는 collection 관련 API

## filter
조건에 따라 collection의 원소를 filtering 하는 기능을 합니다.

```kotlin
fun main() {
    val list = listOf(1, 2, 3, 4)
    println (list.filter { it % 2 == 0 })
}

출력: [2, 4]
```

```kotlin
data class Person(val name: String, val age: Int)

fun main() {
    val people = listOf(Person("Alice", 29), Person("Bob", 31))
    println (people.filter { it.age > 30 })
}

출력: [Person(name=Bob, age=31)]
```

## map
원소를 원하는형태로 변환하는 기능을 하며 이들의 반환타입은 list입니다.

```kotlin
data class Person(val name: String, val age: Int)

fun main() {
    val people = listOf(Person("Alice", 29), Person("Bob", 31))
    println(people.map { it.name })
}

출력: [Alice, Bob]
```

## all
collection 전체가 주어진 조건을 만족하는지 판단합니다. 반환타입은 Boolean입니다.

```kotlin
fun main() {
    val list = listOf(1, 2, 3)
    println(list.all { it == 3 })
}

출력: false
```

## any
collection 원소중에 하나라도 주어진 조건을 만족하는지를 판단합니다. 반환타입은 Boolean입니다.

```kotlin
fun main() {
    val list = listOf(1, 2, 3)
    println(list.any { it == 3 })
}

출력: true
```

## count
주어진 조건을 만족하는 원소의 개수를 반환합니다. 반환타입은 Int입니다.

```kotlin
fun main() {
    val list = listOf(1, 2, 3)
    print("${list.filter { it > 1 }.size} ")
    print(list.count { it > 1 })
}

출력: 2 2
```
collection의 size를 이용해도 값은 동일하지만,

size의 경우 filter의 결과인 새로운 list가 만들어지고 size를 측정하므로

위 경우에는 count를 이용하는게 더 효율적입니다. (**count는 중간 결과(list)를 만들지 않습니다.**)

## find
주어진 조건에 만족하는 첫번째 원소만 반환, 조건에 만족하지 않으면 null을 반환합니다.
```kotlin
fun main() {
    val list = listOf(1, 2, 3)
    println(list.find { it > 1 })
}

출력: 2
```

## groupBy
주어진 조건에 따라서 grouping이 됩니다. 반환타입: ex)Map<Int,List<Person>> , Map<String, List<String>>
```kotlin
data class Person(val name: String, val age: Int)

fun main() {
    val people = listOf(Person("Alice", 31),
            Person("Bob", 29), Person("Carol", 31))
    println(people.groupBy { it.age })
}

출력: {31=[Person(name=Alice, age=31), Person(name=Carol, age=31)], 29=[Person(name=Bob, age=29)]}
```
위 코드의 return값은 Map<Int, List<Person>> 이 됩니다.

-> key: age, value: 해당되는 person 객체를 담는 리스트


## flatMap
주어진 람다로 Map을 만들고 이를 다시 flat하게 만드는 함수

즉 map을 처리하고 난 다음의 결과가 list인경우 이 list의 원소를 다시 펼쳐서 하나의 list로 만듭니다.
```kotlin
class Book(val title: String, val authors: List<String>)

fun main() {
    val books = listOf(Book("Thursday Next", listOf("Jasper Fforde")),
            Book("Mort", listOf("Terry Pratchett")),
            Book("Good Omens", listOf("Terry Pratchett", "Neil Gaiman")))
    println (books.flatMap { it.authors }.toSet())
}

출력: [Jasper Fforde, Terry Pratchett, Neil Gaiman]
```
위 코드에서는 람다식{it.authors}의 map 결과가 list입니다.

flatMap이기 때문에 list로 나온 결과를 전부 flat 하게 펼칩니다.

그리고 나서 toSet()으로 묶어주면 중복이 제거되어 unique한 author로 구성된 set이 반환됩니다.

## asSequence
중간 결과 없이 원소가 하나씩 전체 flow를 거치고 

그다음 원소, 그다음..형식으로 처리(**원소의 수가 많을 때 사용하면 성능 이득**)

(일단 list가 sequnce로 변환되면, 각 단계별 중간 연산결과를 만들어 내지 않기 때문에 

최종 결과를 요청하는 시점에 모든 계산이 수행 된다.

최종 연산을 요청하는 방법은 마지막에 toList(), toSet()을 붙여 결과를 요청하는 api를 붙이면 된다.)
```kotlin
fun main() {
    val sequence = listOf(1, 2, 3, 4).asSequence()
            .map { print("map($it) "); it * it }
            .filter { print("filter($it) "); it % 2 == 0 }
            .toList()

    print(sequence)
}

출력: map(1) filter(1) map(2) filter(4) map(3) filter(9) map(4) filter(16) [4, 16]
```
위 코드처럼 sequence로 만들어 처리하는 경우 위 코드의 filter와 map을 순서를 바꿨다면 속도는 훨씬 빨라질 것이다.

```kotlin
fun main() {
    val sequence = listOf(1, 2, 3, 4).asSequence()
            .filter { print("filter($it) "); it % 2 == 0 }
            .map { print("map($it) "); it * it }
            .toList()

    print(sequence)
}

출력: filter(1) filter(2) map(2) filter(3) filter(4) map(4) [4, 16]
```
