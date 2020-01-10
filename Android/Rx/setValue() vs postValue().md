# Android Livedata setValue() vs postValue()

LiveData는 저장된 데이터를 갱신하기 위한 공개적으로 사용가능한 메소드가 없다.

MutableLiveData 클래스는 공개적으로 setValue(T), postValue(T) 메소드를 노출하고,

LiveData안에 저장된 데이터를 수정할 필요가 있다면, 반드시 저 메소드들만 이용해야 한다.

일반적으로 수정가능한 MutableLiveData는 ViewModel 내부에서 사용되고,

ViewModel은 수정 불가능한 LiveData로 관찰자에게 노출한다.

---

Main 스레드를 사용하여 데이터를 변경하는 동안에는 MutableLiveData 클래스의 setValue() 메소드를 사용해야하고

백그라운드 스레드를 사용하여 LiveData를 변경하는 동안 MutableLiveData 클래스의 postValue() 메소드를 사용해야합니다.

---

setValue()는 메인쓰레드에서 즉시 값을 변경하고 옵저버로 데이터 변경을 알려줍니다.

반면에, postValue()는 Runnable로 데이터 변경을 예약하기 때문에 바로 변경이 되지 않습니다.

메인쓰레드에서 Runnable이 실행되기 전에 postValue()가 여러번 호출되도 마지막으로 변경된 값만 옵저버로 전달됩니다.


```kotlin
override fun onActive() {
    Log.d(TAG, "onActive")
    setValue(1)
    setValue(2)
    setValue(3)
}

onActive
1
2
3
```
```kotlin
override fun onActive() {
    Log.d(TAG, "onActive")
    postValue(1)
    postValue(2)
    postValue(3)
}

onActive
3
```
