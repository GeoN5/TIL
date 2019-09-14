# MVVM 아키텍처

## 기본적인 MVVM 아키텍처의 동작방식
<img src="https://inducesmile.com/wp-content/uploads/2018/06/mvvm.png" width="800">

1. **View에 입력이 들어오면 ViewModel에 명령을 한다.**
2. **ViewModel은 필요한 데이터를 Model에 요청 한다.**
3. **Model은 ViewModel에 필요한 데이터를 응답 한다.**
4. **ViewModel은 응답 받은 데이터를 가공해서 저장한다.**
5. **View는 ViewModel과의 Data Binding으로 인해 자동으로 갱신 된다.**

이때 View는 ViewModel의 참조를 가지고 있지만, ViewModel은 View의 참조를 가지고 있지 않고,

ViewModel도 Model의 참조를 가지고 있지만, Model은 ViewModel의 참조를 가지고 있지 않습니다.

---

## MVVM – (ViewModel - Databinding - XML 관계)
<img src="https://miro.medium.com/max/1810/1*HpBpwd9E6IyWmlO0jth0Mg.png" width="800">

Databinding을 통해서 ViewModel의 notify와 View가 ViewModel 로 명령을 전달하는 과정들을 UI 코드로 정의하기에

View 와 ViewModel 은 서로의 독립성을 더 높일 수 있습니다.

 =  *(**매개체인 DataBinding 을 통해 View와 ViewModel 이 서로 알지 않아도 되게 한다.**)*



