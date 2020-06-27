# View Binding [Link](https://developer.android.com/topic/libraries/view-binding)

* View Binding을 사용하면 view와 상호작용하는 코드를 쉽게 작성할 수 있다.

* module에서 enabled된 View Binding은 module에 있는 각 XML layout 파일의 Binding 클래스를 생성한다.

* Binding Class의 instance에는 상응하는 layout에 id가 있는 모든 view의 직접 참조가 포함된다.

* 대부분의 경우에서 View Binding이 `findViewById`를 대체함.
---
## Setup instructions

View Binding은 module별로 사용 설정된다. module에서 View Binding을 사용 설정하려면
module-level단의 `build.gradle`에 ViewBinding build option을 true로 설정한다.
```kotlin
android {
    ...
    buildFeatures {
        viewBinding true
    }
}
```
View Binding Class를 생성하는 동안 layout 파일을 무시하려면 `tools:viewBindingIgnore="true"`속성을
layout 파일의 root view에 추가한다.
```kotlin
<LinearLayout
        ...
        tools:viewBindingIgnore="true" >
    ...
</LinearLayout>
```
---
## Usage

View Binding Class의 이름은 XML layout 파일의 이름을 camel case로 변환하고 끝에 'Binding'이 추가되어 생성된다.

예를 들어 layout 파일 이름이 `result_profile.xml`인 경우 다음과 같다.
```kotlin
<LinearLayout ... >
    <TextView android:id="@+id/name" />
    <ImageView android:cropToPadding="true" />
    <Button android:id="@+id/button"
        android:background="@drawable/rounded_button" />
</LinearLayout>
``` 
생성된 Binding Class이름은 `ResultProfileBinding`이 된다.

해당 Class에는 `name`이라는 `TextView`와 `button`이라는 `Button`의 두 필드가 있다.

layout의 `ImageView`에는 id가 없으므로 Binding Class에 reference가 없다.


또한 모든 Binding Class에는 상응하는 layout 파일의 root view에 관한 직접 참조를 제공하는`getRoot()`메소드가 포함된다.

이 예에서는 `ResultProfileBinding` Class의 `getRoot()`메소드가 `LinearLayout` root view를 반환한다.

### Use view binding in activities

Activity에서 사용할 Binding Class Instance를 설정하려면 Activity의 `onCreate()`메소드에서 아래 단계를 따른다.
1. 생성된 Binding Class에 포함된 정적 메소드인 `inflate()`메소드를 호출하면 Activity에서 사용될 Binding Class Instance가 생성된다.
2. 생성된 instance의 `getRoot()`메소드를 호출하거나 **Kotlin property syntax**를 사용하여 root view reference를 가져온다.
3. root view를 `setContentView()`에 전달하여 화면상의 active view로 만든다.
```kotlin
private lateinit var binding: ResultProfileBinding

override fun onCreate(savedInstanceState: Bundle) {
    super.onCreate(savedInstanceState)
    binding = ResultProfileBinding.inflate(layoutInflater)
    val view = binding.root
    setContentView(view)
}
```
이제 Binding Class Instance를 사용해서 view를 참조할 수 있다.

```kotlin
binding.name.text = viewModel.name
binding.button.setOnClickListener { viewModel.userClicked() }
```
### Use view binding in fragments

Fragment에서 사용할 Binding Class Instance를 설정하려면 Fragment의 `onCreateView()`메소드에서 아래 단계를 따른다.
1. 생성된 Binding Class에 포함된 정적 메소드인 `inflate()`메소드를 호출하면 Fragment에서 사용할 Binding Class Instance가 생성된다.
2. `getRoot()`메소드를 호출하거나 **Kotlin property syntax**를 사용하여 root view reference를 가져온다.
3. `onCreateView()`메소드에서 root view를 반환하여 화면상의 active view로 만든다.

>**참고**: `inflate()`메소드를 사용하려면 layout inflater를 전달해야 한다.
>
>layout이 이미 Inflate되었다면 Binding Class의 정적 메소드인 `bind()`메소드를 호출한다.
```kotlin
private var _binding: ResultProfileBinding? = null
// This property is only valid between onCreateView and
// onDestroyView.
private val binding get() = _binding!!

override fun onCreateView(
    inflater: LayoutInflater,
    container: ViewGroup?,
    savedInstanceState: Bundle?
): View? {
    _binding = ResultProfileBinding.inflate(inflater, container, false)
    val view = binding.root
    return view
}

override fun onDestroyView() {
    super.onDestroyView()
    _binding = null
}
```
이제 Binding Class의 Instance를 사용해서 view를 참조할 수 있다.
```kotlin
    binding.name.text = viewModel.name
    binding.button.setOnClickListener { viewModel.userClicked() }
```
>**참고**: Fragment는 view보다 오래 지속되므로, `onDestroyView()`메소드에서 
>
>Binding Class Instance의 reference를 정리해야 한다.
---
## Differences from findViewById

View Binding에는 `findViewById`를 사용하는 것에 비해서 중요한 장점이 있다.
* **Null safety**: View Binding은 view의 직접 참조를 생성하므로 유효하지 않은 view id로 인해 Null Pointer Exception이 발생할 위험이 없다. 
    또한 layout의 일부 구성에만 view가 있는 경우에는 Binding Class에서 참조를 포함하는 필드가 `@Nullable`로 표시된다.
* **Type safety**: 각 Binding Class에 있는 필드의 type이 XML 파일에서 참조하는 view와 일치한다.
    즉, Class Cast Exception이 발생할 위험이 없다.

이러한 차이점은 layout과 코드 사이의 비호환성으로 인해 runtime이 아닌 compile time에 build가 실패하게 된다는 것을 의미한다.

---
## Comparison with data binding

View Binding과 Data Binding 모두 view를 직접 참조하는데 사용할 수 있는 Binding Class를 생성한다.

하지만 View Binding은 보다 단순한 use cases를 처리하기 위한 것이며 Data Binding에 비해 다음과 같은 이점을 가진다.
* **Faster compilation**: View Binding에는 `annotation processing`이 필요하지 않으므로 compile time이 더 짧다.
* **Ease of use** : View Binding은 특별히 태그된 XML layout 파일이 필요하지 않으므로 앱에서 더 신속하게 채택할 수 있다.
    Module에서 View Binding을 enable하면 Module의 모든 layout에 View Binding이 자동으로 적용된다.

반대로 View Binding에는 Data Binding과 비교할 때 다음과 같은 제한사항이 있다.
* View Binding은 **레이아웃 변수 또는 레이아웃 표현식**을 지원하지 않으므로 XML layout 파일에서 직접 동적 UI 컨텐츠를 선언하는 데 사용할 수 없다.
* View Binding은 **양방향 데이터 결합**을 지원하지 않는다.

>위 사항을 고려할 때 일부 사례에서 프로젝트에 View Binding과 Data Binding을 모두 사용하는 것이 좋다.
>
>고급 기능이 필요한 layout에는 Data Binding을, 고급 기능이 필요 없는 layout에는 View Binding을 사용할 수 있다.
