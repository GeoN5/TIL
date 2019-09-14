# Rxjava + Retrofit + Scheduler concept


```kotlin
ex) service.listRepos("USER")
        .subscribeOn(Schedulers.io())
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe({
            // handle success
        }, {
            // handle fail
        }))
```
**subscribeOn()** : 데이터를 강에 흘려보내는 스레드(스케줄러)를 지정

정확하게는 데이터를 발행(연산)하는 스케줄러를 지정하는 것이라고 할 수 있다.

지금 네트워킹작업은 IO작업이니까 Schedulers.io()로 설정해주었다.

IO 스케쥴러 말고도 메인스케쥴러, 뉴 스케쥴러등이 있다.

```kotlin
ex) subscribeOn(Schedulers.io())
```

---

**observeOn()** : 데이터를 줍는 스레드(스케줄러)를 지정

즉, 데이터를 구독하는 스케줄러를 지정하는 것이다.

메인스레드인 이유는, 안드로이드 UI작업을 해주기 위해서이다.

```kotlin
ex)  AndrodiSchedulers.mainThread()
```

---

**subscribe()** : 주워서 쓰는 부분

```kotlin
ex) service.listRepos("USER")
        .subscribeOn(Schedulers.io())
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe({
            name_text_view = it.name
        }, {
            Log.d(TAG, "ERROR message : ${it.message}")
        }))
```

---

**Single** : 간단하게 싱글은 '싱글' 입니다. 데이터를 한번 뿌린다는 뜻.

REST API로 호출한 데이터는 서버로부터 딱 한번 받고 끝이기 때문에 Single을 이용하게 된다.

---

## 스케줄러 [http://reactivex.io/documentation/scheduler.html](http://reactivex.io/documentation/scheduler.html)

Observable 연산자 체인에 멀티스레딩을 적용하고 싶다면, 특정 스케줄러를 사용해서 연산자

(**또는 특정 Observable**)를 실행하면 된다.

ReactiveX의 일부 Observable 연산자는 사용할 스케줄러를 파라미터로 전달 받기도 하는데,

이 연산자들은 자신이 처리할 연산의 일부 또는 전체를 전달된 스케줄러 내에서 실행한다.

기본적으로, Observable과 연산자 체인은 이처럼 스케줄러를 통해 동작하고

**Subscribe 메서드**가 호출되는 스레드를 사용해서 옵저버에게 알림을 보낸다.

**SubscribeOn 연산자**는 다른 스케줄러를 지정해서 Observable이 처리해야 할 연산자들을 실행 시킨다.

그리고, **ObserveOn 연산자**는 Observable이 옵저버에게 알림을 보낼때 사용 할 스케줄러를 명시한다.

**SubscribeOn 연산자**는 Observable이 연산을 위해 사용할 스레드를 지정하며,

연산자 체인 중 아무 곳에서 호출해도 문제되지 않는다.

하지만, **ObserveOn 연산자**는 연산자 체인 중 Observable이 사용할 스레드가

호출 체인 중 어느 시점에서 할당되는지에 따라 그 후에 호출되는 연산자는 영향을 받는다.

그렇기 때문에, 어쩌면 특정 연산자를 별도의 스레드에서 실행 시키기 위해

연산자 체인 중 한 군데 이상에서 **ObserveOn**을 호출하게 될 것이다.
