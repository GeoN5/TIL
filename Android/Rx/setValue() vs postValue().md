# Android Livedata setValue() vs postValue()

LiveData는 저장된 데이터를 갱신하기 위한 공개적으로 사용가능한 메소드가 없다.
MutableLiveData 클래스는 공개적으로 setValue(T), postValue(T) 메소드를 노출하고,
LiveData안에 저장된 데이터를 수정할 필요가 있다면, 반드시 저 메소드들만 이용해야 한다.

일반적으로 수정가능한 MutableLiveData는 ViewModel 내부에서 사용되고,
ViewModel은 수정 불가능한 LiveData로 관찰자에게 노출한다.

---

Main 스레드를 사용하여 데이터를 변경하는 동안에는 MutableLiveData 클래스의 setValue() 메소드를 사용해야하고
백그라운드 스레드를 사용하여 LiveData를 변경하는 동안 MutableLiveData 클래스의 postValue() 메소드를 사용해야합니다.
