# [Week 1] MVVM 무작정 따라하기 스터디



## 0. Introduction

코드 의심하고, 남의 코드 많이봐라, 코드 철학 만들기

<br/>



## 1. MVVM을 왜 사용하는가

### 1-1. 사용 이유

우리는 다른 사람과 함께일을 해야되고, 내가 남의 코드를, 남의 코드를 내가 보게 된다.

협업을 위해서 어느 위치에 어떤 로직이 들어있나를 미리 정의해 놓기에 사용함.



### 1-2. MVVM 장점

- 유지보수 용이) 아키텍처를 적용한 프로젝트는 많은 사람들이 코드를 알아보기 쉬워진다

- 코드의 역할이 잘 분리된다
- 의존 관계가 잘 분리가 된다. 즉, 변화에 유연함



### 1-3. 단점

- 특정 아키텍처로 개발을 시작하면, 바꾸기가 많이 어려움 

  따라서, 프로젝트에 맞지 않는 아키텍처 구조임을 인지하게돼도, 구조 자체를 변경하기 힘들어서 비효율 적인 코드로 개발해야함

- 생산속도가 떨어진다 👉 기능 하나 만들 때마다 파일이 많이 만들어 짐 👉 보일러플레이팅 코드 증가



<br/>


## 2. MVVM이 뭔데?

### 2-1. MVC 

MVC이전에는 스파게티 코드였음.

MVC에서 뷰랑 모델을 분리하고자 했지만, View가 Model로 부터 data 를 받아 띄우기 때문에, 완전한 분리는 불가능

<img src = "https://user-images.githubusercontent.com/59546818/156130259-541a520b-8a94-49e4-a29c-d086a42034fe.png" width="600">


### 2-2. MVP
<img src = "https://user-images.githubusercontent.com/59546818/156130509-667fbea5-2b88-4206-bf8d-08d237f6cd30.png" width="600">


presenter가 모델로 부터 데이터를 가져오고, 이를 가공해서 presenter가 뷰에게 넘기는 구조

View는 그저 Presenter에게 요청만 보낸다. 따라서, View와 Model을 분리 할 수 있다는 장점이 있지만, 단점도 존재한다

앱 규모가 커지다 보면 프리젠터가 엄청 비대해짐

뷰와 프리젠터 간의 양방향 의존성이 생김 -> 1대1로 결합.. 재활용이 안됨(~~안써봐서 이게 무슨말인지 모르겠네~~)

생겨나는 보일러 플레이트 코드와, 콜백 인터페이스들이 많이 생김



### 2-3. MVVM
<img src = "https://user-images.githubusercontent.com/59546818/156130572-eae11d44-b378-4ef9-9dc9-fc48508e8941.png" width="600">

Microsoft에서 발표함

view/ viewmodel / model 로 구성되어 있으나, Binder를 간과할 수 없음

뷰와 뷰모델 사이에 Binder라는 친구가 숨어져있다. 안드로이드에서는 Binder의 역할이 DataBinding이다.

DataBinding을 안쓰면 MVVM이 아니다 (MVP다)

지금까지 말한 것은 MVVM의 ViewModel에 관한 이야기였고, AAC에서 ViewModel이라는 라이브러리를 제공해준다.


<br/>

<br/>




### **❓ AAC의 뷰모델은 뭔데?** 

얘는 더 많은일을 함

Activity, Fragment의 lifecycle의 의존성을 낮추기 위해서 씀

> [예시]  휴대폰에서 가로 뷰 👉 세로 뷰로 회전을 하게 되면 `Configuration Change`가 일어난다 
>
> 이 때 onCreate()가 다시 호출 되기 때문에, Viewmodel이 죽게 된다.
>
> 그러나, 실제로 Activity가 끝났을때만 Viewmodel이 죽게끔 AAC에서 지원을 해준다
>
>
> 또한, Coroutine 쓸 때 viewmodel scope 도 지원해준다
>
> viewmodel() 을 상속받은 안드로이드 viewmodel() -> 얘는 viewmodel이 context를 받음 잘 안씀



MVVM의 가장 큰 목적은 View와 VIewmodel의 연결을 최소화 하는 것.

아래와 같은 코드가 대표적인 예시이지만, 잘못된 점이 있는 코드다.

xml에서 처리를 하는 것이 정석이고, 아래와 같은 코드 비율을 줄이는 방식으로 사용하자.

```kotlin
class OOOActivity() {

	val viewModel: ViewModel
	val textView: TextView

	fun onCreate() {
        // activity 안에서 viewmodel을 호출하면 연결성이 너무 높다
        // xml안에서 처리할 수 있음
        // 아래와 같은 코드를 줄이는 방식으로 쓰되, 완전히 xml로 처리하기는 어렵다는 점..
		viewModel.title.observe(this){
				textview.text = it
		}
	}
}
```

https://tv.naver.com/v/4637223?query=mvvm

***AAC의 viewmodel과 MVVM의 viewmodel은 차이점이 명확하고, 이를 꼭 인지해야한다***




<br/>
<br/>





### ❓ **Google에서 말하는 MVVM?**

Google의 Architecture의 공식문서를 보면 mvvm이라는 단어가 한줄도 없고 권장 아키텍처라고 적혀있음

아래와 같이 용어를 달리 사용한다.

**Presentation Layer**  = View + Viewmodel

**Domain Layer & Data Layer** = Model



| Domain               | Data                                                         |
| -------------------- | ------------------------------------------------------------ |
| Use Case, Repository | DataSourse, Ect(Retrofit, SQLite, Room, Firestore.., realm.. ) |



**Domain** : 프로젝트를 관통하는 핵심 로직

>  로또 번호, 로또번호 생성기, 로또 사기 기능, 로또 번호 판별 로직 등이 그 예시가 될 수 있겠다~

안드로이드는 도메인의  대부분을 서버에서 처리하기에 나머지는 usecase(나중에)

**Use Case** : Repository한테 요청

**Repository** : Local 혹은 Remote 중에서 어디서 왔는지 출처를 알고 있음

**Data** : 데이터의 출처(SharedPreference, room, 메모리 중에서 어디서 온건지), 인터넷이 연결돼있는지 알 필요 없음

**Remote**도 마찬가지 : Backend에서 온건지 firebase에서 온건지 모름


<br/>
<br/>





Interface RemoteDataSourse & Interface LocalDataSource 따로 구현하는 사람들이 많은데

말리빈은 이렇게 안하고 DataSource에 대한 Interface 하나만 있으면 된다 (다형성 이용하자 주의)

Interface DataSource를 만들고 Local과 Remote는 둘다 DataSource를 상속 받으면 된다

> Impl은 Interface의 구현체로, Implementation의 약자임 
> 
> 창환오빠는Impl로 정리해뒀길래


<br/>
<br/>






## 3. DataBinding

호랑이 담배피던 시절, 김효림이 JAVA로 안드로이드를 배울때에는, findViewById()를 썼다.

그런데 아래와 같은 단점이 존재한다.

0. **귀찮음**
1. **Null Unsafe**
   - 실수로 다른 XML에 있는 view의 id값을 불러온 경우  **NP**(Null Point Exception) 발생

2. Type Unsafe
   - xml에서 `@+id/et_contents` 라는 EditText를 만들었다고 가정하자
   - `val etContents : Button = ..findViewById..  ` 와 같이 type지정을 잘못하게 될 수 있기에,  Type UnSafe 하다

👉 이를 개선한게 viewBinding, dataBinding 이다



**결론 :  Null safe, Type safe 하기 때문에 DataBinding, ViewBinding을 사용한다.**

**그리고 MVVM을 사용하기위해 DataBinding을 활용하는것** 



ViewBinding과 DataBinding을 활용하면, 아래 이미지와 같이 자동 타입 추론이 일어나기에, Type Safe 하다.

![image-20220226111621702](C:\Users\Kim Hyo Rim\AppData\Roaming\Typora\typora-user-images\image-20220226111621702.png)


<br/> <br/>




### 📚 DataBinding 사용법

xml에서 layout이라는 Tag를 꼭 붙여줘야 한다 (viewBInding은 안써도 됨)

dataBinding은 xml에서 @{} 쓸 수있는데, ViewBInding은 xml에 세팅할 수 없음

즉, DataBinding 안에 ViewBinding 기능이 있는거다

따라서, dataBinding이 필요 없는 곳에서는 ViewBinding을 사용하면 된다 (구글 공식문서에서 둘다 적절하게 사용하라고 적혀있음)



1) **build.gradle에서 kapt와 buildFeature**

```
plugins {
    id 'com.android.application'
    id 'kotlin-android'
    id 'kotlin-kapt'📌
}

buildFeatures {
    dataBinding true
    viewBinding true
}
```

2) **xml 파일에서 <layout> 태그를 사용한다**



위와 같이 설정 해주면, DataBinding을 사용할 때 kapt라는  annotationProcessesor 가 impl을 생성한다. 

![image](https://user-images.githubusercontent.com/59546818/156131179-bb739394-dca9-4bc0-a4ba-dd3c4908fc79.png)



<br/>


<br/>




### **❓ BindingAdapter의 형식이 내가 사용해오던 방식과는 다른데?**

원래 나는 BindingAdapter.kt를 보면 Object 안에 함수들을 넣어서 사용했는데,

이번 스터디에서는 Object 선언 없이 함수를 최상단레벨에서 작성했는데, 무엇이 다른건가?

👉형식상의 차이점을 말하자면, Object안에 있는 함수를 활용하기 위해서는 JvmStatic annotation을 붙여줘야 한다는 차이점이 있다 `@JvmStatic`

static 함수로 사용하기 위한 annotation이라는 말이 이제야 이해가 된다.


<br/>


<br/>



### **📋 DataBinding 사용 Case**

1. data 객체 연결 :  <layout> 태그안에서 사용

2. Integer DataBinding 사용하는 방법 : `@{String.valueOf(bindingInteger)}`

3. 특정 형식으로 넣고싶을때 (날짜) `@{memo.createDate}` 👉 BInding Adapter로 연결



<br/><br/>


-----
  
<br/>



### Offline 잡설👻👻



- **{기술 exHilt}** Android Studio Project 키워드로 검색하면서 남의 코드 많이보기 ex. 드로이드나이츠 등..

- 디버깅모드활용 (break point)

- 단축키

  - 멀티커서 : alt + shift
  - 더블클릭 효과 : ctrl + w
  - 포커싱되어있는 변수와 같은 모든 부분 선택 : ctrl + alt + shift + j  (ctrl + r이랑 비슷한 느낌인듯?)
  - 직전커서 위치 : ctrl + alt + 방향키
  - 전체 검색: ctrl+ shift + f  // shift 두번씩 갈기면 검색영역이 바뀜
  - presentation mode -> 글자 커짐

- **Compat**이 무엇인가? 👉 버전관리 알아서 해주는애 

  `if (sdk.ver < 23 ) { }` 이런 코드가 있음

  따라서 Context.getColor() 에서 버전 문제가 발생할 경우, ContextCompat.getColor() 를 사용할 수 있음

