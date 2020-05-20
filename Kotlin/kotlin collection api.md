# 코틀린에서 제공하는 collection 관련 API

## size (val size: Int)
collection의 크기를 반환합니다.

```kotlin
val list = arrayListOf("One", "Two", "Three", "Four", "Five")
println(list.size)

출력: 5
```

## max (fun <T: Comparable\<T>> Iterable\<T>.max: T?)
값이 가장 큰 항목을 반환, 항목이 없을 경우 null을 반환한다.

```kotlin
println(list.max())

출력: Two
```

## min (fun <T: Comparable\<T>> Iterable\<T>.min(): T?)
값이 가장 작은 항목을 반환, 항목이 없을 경우 null을 반환한다.

```kotlin
println(list.min())

출력: Five
```

## minus (fun \<T> Iterable\<T>.minus(element: T): List\<T>)
지정된 항목이 제외된 리스트를 반환. remove() 메소드와 달리 새로운 리스트를 만들어 반환한다.

항목 대신 array, collection, range 등이 들어갈 수 있다.

```kotlin
println(list.minus("Five"))

출력: [One, Two, Three, Four]
```

## plus (fun \<T> Iterable\<T>.plus(element: T): List \<T>)
지정된 항목이 추가된 리스트를 반환. add() 메소드와 달리 새로운 리스트를 만들어 반환한다.

항목 대신 array, collection, range 등이 들어갈 수 있다.

```kotlin
println(list.plus(listOf("Six", "Seven")))

출력: [One, Two, Three, Four, Five, Six, Seven]
```

## first (fun \<T> List\<T>.first() :T)
첫 번째 항목을 반환합니다.

```kotlin
println(list.first())

출력: One
```

## Last (fun \<T> List\<T>.Last(): T)
마지막 항목을 반환합니다.

```kotlin
println(list.last())

출력: Five
```

## contains (fun contains(element: E): Boolean)
지정 항목이 collection에 포함되어 있는지를 반환합니다.

```kotlin
println(list.contains("Three"))

출력: true
```

## containsAll (fun \<T> Collection\<T>.containsAll(elements: Collection\<T>): Boolean)
지정된 collection의 모든 원소가 리스트 내에 있는지 반환합니다.

```kotlin
println(list.containsAll(listOf("One", "Two")))

출력: true
```

## get (fun get(index: Int): E)
지정된 인덱스의 항목을 반환합니다.

```kotlin
println(list.get(2))

출력: Three
```

## indexOf (fun indexOf(element: E): Int)
지정된 항목의 인덱스를 반환합니다. 리스트의 처음부터 검색한다.

```kotlin
println(list.indexOf("Three"))

출력: 2
```

## isEmpty (fun isEmpty(): Boolean)
컬렉션이 비어있는지에 대한 여부를 반환합니다.

```kotlin
println(list.isEmpty())

출력: false
```

## shuffled (fun \<T> Iterable\<T>.shuffled(): List\<T>)
순서가 랜덤하게 섞인 새로운 리스트를 반환한다.

```kotlin
println(list.shuffled())

출력: [Two, Four, Five, One, Three]
```

## sorted (fun <T: Comparable\<T>> Iterable\<T>.sorted(): List\<T>)
Comparable 인터페이스에 구현된 순서대로 정렬된 리스트를 반환한다.

sort() 메소드와 달리 새로운 리스트를 반환한다.

```kotlin
println(list.sorted())

출력: [Five, Four, One, Three, Two]
```

## sortedBy (fun <T, R: Comparable\<R>> Iterable\<T>.sortedBy(selector: (T) -> R?): List\<T>)
지정한 람다식에 의한 순서대로 정렬된 리스트를 반환한다.

sortBy() 메소드와 달리 새로운 리스트를 반환한다.

```kotlin
println(list.sortedBy { it.length })

출력: [One, Two, Four, Five, Three]
```

## subList (fun subList(fromIndex: Int, toIndex: Int): List\<E>)
시작 인덱스(포함)부터 끝 인덱스(포함 안함)까지 범위의 리스트를 반환합니다.

반환 되는 리스트는 원본 리스트를 참조하기 때문에 해당 리스트가 변경되면 원본 리스트 역시 변경됩니다.

```kotlin
println(list.subList(1, 3))

출력: [Two, Three]
```

## all (fun \<T> Iterable\<T>.all(predicate: (T) -> Boolean): Boolean)
collection 전체가 주어진 조건을 만족할 경우 true를 반환합니다.

```kotlin
val list = listOf(1, 2, 3)
println(list.all { it == 3 })

출력: false
```

## any (fun \<T> Iterable\<T>.any(predicate: (T) -> Boolean): Boolean) 
하나 이상의 항목이 조건에 만족할 경우 true를 반환합니다.

```kotlin
val list = listOf(1, 2, 3)
println(list.any { it == 3 })

출력: true
```

## asReversed (fun \<T> Iterable\<T>.asReversed(): List\<T>)
리스트의 역순 리스트를 만들어 반환합니다.

```kotlin
println(list.asReversed())

출력: [Five, Four, Three, Two, One]
```

## count (fun \<T> Iterable\<T>.count(predicate: (T) -> Boolean): Int)
주어진 조건을 만족하는 항목의 개수를 반환합니다.

```kotlin
val list = listOf(1, 2, 3)
print("${list.filter { it > 1 }.size} ")
print(list.count { it > 1 })

출력: 2 2
```
collection의 size를 이용해도 값은 동일하지만,

size의 경우 filter의 결과인 새로운 list가 만들어지고 size를 측정하므로

위 경우에는 count를 이용하는게 더 효율적이다. (**count는 중간 결과(list)를 만들지 않는다.**)

## take (fun \<T> Iterable\<T>.take(n: Int): List\<T>)
첫 항목부터 n개의 항목을 반환한다.

```kotlin
println(list.take(2))

출력: [One, Two]
```

## drop (fun \<T> Iterable\<T>.drop(n: Int): List\<T>)
처음 n개의 항목을 제외한 리스트를 반환합니다.

```kotlin
println(list.drop(2))

출력: [Three, Four, Five]
```

## dropLast (fun \<T> List\<T>.dropLast(n: Int): List\<T>)
마지막 n개의 항목을 제외한 리스트를 반환합니다.

```kotlin
println(list.dropLast(2))

출력: [One, Two, Three]
```

## removeAll (fun removeAll(element: Collection\<E>): Boolean)
지정된 컬렉션에 포함된 항목을 모두 제거한다.
```kotlin
list.removeAll(listOf("Two","Four"))
println(list)

출력: [One, Three, Five]
```

## retainAll (fun retainAll(element: Collection\<E>): Boolean)
지정된 컬렉션에 포함된 항목만 남겨두고 모두 제거한다.
```kotlin
list.retailAll(listOf("Two", "Four"))
println(list)

출력: [Two, Four]
```

## filter (fun \<T> Iterable\<T>.filter(predicate: (T) -> Boolean): List\<T>)
조건과 일치하는 항목만 포함하는 리스트를 반환합니다.

```kotlin
println (list.filter { it.startsWith("F") })

출력: [Four, Five]
```

```kotlin
data class Person(val name: String, val age: Int)

fun main() {
    val people = listOf(Person("Alice", 29), Person("Bob", 31))
    println (people.filter { it.age > 30 })
}

출력: [Person(name=Bob, age=31)]
```

## filterNot (fun \<T> Iterable\<T>.filterNot(predicate: (T) -> Boolean): List\<T>)
조건과 일치하지 않는 항목만 포함하는 리스트를 반환한다.

```kotlin
println(list.filterNot { it.startsWith("T") })

출력: [One, Four, Five]
```

## find (fun \<T> Iterable\<T>.find(predicate: (T) -> Boolean): T?
주어진 조건에 만족하는 첫번째 원소를 반환, 조건에 만족하지 않으면 null을 반환한다.

```kotlin
println(list.find { it.startsWith("T") })

출력: Two
```

## findLast (fun \<T> Iterable\<T>.findLast(predicate: (T) -> Boolean): T?
주어진 조건에 만족하는 마지막 항목을 반환, 조건에 만족하지 않으면 null을 반환한다.

```kotlin
println(list.findLast { it.startsWith("T") })
출력: Three
```

## map (fun <T, R> Iterable\<T>.map(transform: (T) -> R): List\<R>)
모든 항목에 지정된 변환 함수가 적용된 결과 리스트를 반환한다.

```kotlin
println(list.map { it + "!" })

출력: [One!, Two!, Three!, Four!, Five!]
```

```kotlin
data class Person(val name: String, val age: Int)

fun main() {
    val people = listOf(Person("Alice", 29), Person("Bob", 31))
    println(people.map { it.name })
}

출력: [Alice, Bob]
```

## mapIndexed (fun <T, R> Iterable\<T>.mapIndexed(transform: (index: Int, T) -> R): List\<R>)
인덱스와 함께 지정된 변환 함수가 적용된 결과 리스트를 반환한다.

```kotlin
println(list.mapIndexed { index, item -> "$item(${index + 1})" })

출력: [One(1), Two(2), Three(3), Four(4), Five(5)]
```

## groupBy
주어진 조건에 따라서 grouping이 된다. 반환타입: ex)Map<Int,List<Person>> , Map<String, List<String>>
    
```kotlin
data class Person(val name: String, val age: Int)

fun main() {
    val people = listOf(Person("Alice", 31),
            Person("Bob", 29), Person("Carol", 31))
    println(people.groupBy { it.age })
}

출력: {31=[Person(name=Alice, age=31), Person(name=Carol, age=31)], 29=[Person(name=Bob, age=29)]}
```
위 코드의 return값은 Map<Int, List<Person>> 이 된다.

## flatMap
주어진 람다로 Map을 만들고 이를 다시 flat하게 만드는 함수

즉 map을 처리하고 난 다음의 결과가 list면 이 list의 원소를 다시 펼쳐서 하나의 list로 만든다.
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
위 코드에서는 람다식{it.authors}의 map 결과가 list다.

flatMap이기 때문에 list로 나온 결과를 전부 flat 하게 펼친다.

그리고 나서 toSet()으로 묶어주면 중복이 제거되어 unique한 author로 구성된 set이 반환된다.

## asSequence
중간 결과 없이 원소가 하나씩 전체 flow를 거치고 

그다음 원소, 그다음..형식으로 처리(**원소의 수가 많을 때 사용하면 성능 이득**)

일단 list가 sequnce로 변환되면, 각 단계별 중간 연산결과를 만들어 내지 않기 때문에 

최종 결과를 요청하는 시점에 모든 계산이 수행 된다.

최종 연산을 요청하는 방법은 마지막에 toList(), toSet()을 붙여 결과를 요청하는 api를 붙이면 된다.

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
