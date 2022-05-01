## [Week9] Hilt



### 📌 DI가 뭔가?

Car과 Engine예제로 설명 해줌.. 다덜 공식문서에서 한번쯤 봤을법한~.~



### 📌 DI를 하는 방법

1. 수동
   - 수동 DI를 **서비스로케이터 패턴**이라고 하는데, 안티패턴임 

2. 자동(라이브러리)

   - 라이브러리에게 객체를 만드는 방법만 알려주면, 객체를 알려줌

   - 객체를 생성하고 할당하고 쓰는것 - 원래는 우리가 제어했었는데, DI 툴을 쓰면 이걸 자체적으로 해줌.

   - 그렇다면 이 제어권을 프로그래머 IOC - Inversion of Control



### 📌 DI Tool의 종류

1. Koin
2. Kodein
3. Dagger
4. Hilt
5. Spring Container(Spring전용)

- koin, dagger, hilt 는 언어 무관하게 적용가능함. 안드에서만 쓸수있다는 생각은 착각임.



### 📌 DI 방식

1. **Runtime 방식**

   - 방식 : 객체를 사용하는 순.간. 라이브러리가 객체를 찾음

   - 라이브러리 : Koin, Kodein

   - 장점

     - 러닝커브가 낮음 - Service Locator와 유사함
     - 컴파일타임은 Compile 방식에 비해 빠름

   - 단점 

     - 객체가 필요할때 의존성 추가 로직을 실행하기에, 휴먼에러가 발생할 경우 런타임에 에러가 남
     - 상대적으로 느림 - 실행시에 객체를 만들기 때문에(자세히는 의존성 트리를 통해 알아내는 로직)

     

2. **Compile 방식**

   - 방식 : 애초에 build 할때 객체를 생성하고 넣는 로직을, kapt를 통해 생성됨
   - 라이브러리 : Hilt, Dagger

   - 장점
     - Dagger에 비해 쉬워짐 cf. Dagger : Learning Curve가 미쳤음, Boiler Plate 코드가 많음
     - 컴파일 타임에 의존성 트리를 계산하기 때문에, 휴먼에러가 발생할 경우 컴파일 타임에 미리 확인할 수 있음(단, 이는 시간적 측면에서 단점으로 이어짐)
     - 컴파일 타임에 많은 일을 하기에, 런타임이 빠름 - 객체생성하는게 끝임 
   - 단점
     - 컴파일을 할 때 kapt가 코드를 생성해주는 과정에서 컴파일타임이 너무 느려짐



### 📌 SDK 추가

Koin

```kotlin
implementation "io.insert-koin:koin-android:3.1.6"
```

Hilt

```kotlin
// App level
plugins {
    id 'dagger.hilt.android.plugin'
}

dependencies {
    implementation 'com.google.dagger:hilt-android:2.39.1'
    kapt 'com.google.dagger:hilt-android-compiler:2.39.1'
}

// Project level
buildscript {
    dependencies {
        classpath 'com.google.dagger:hilt-android-gradle-plugin:2.38.1'
    }
}
```



// 아 중간에 연락와서 상황 놓침...... RoomModule 생성 부터 다시 들음

자바 인젝트에 있는 어노테이션을 사용함

@Module : 난 모듈이야

@InstallIn(SingletoneCompoenet::class) : 생명주기 어케 할래

@Provides

@Singletone



hilt는 context를 다 안다

`@ApplicationContext context: Context` 이거 없어도 context 주입 가능



annotation 만들기 (DiariesSource interface를 상속하는 친구들이 2개가 있으므로 ..? 아마도..?)

```kotlin
@Qualifier
@Retention(AnnotationRetention.BINARY)
annotation class LocalDiariesSource

@Qualifier	// hilt 인식
@Retention(AnnotationRetention.BINARY)
annotation class RemoteDiariesSource
```

이걸 hilt에 알려줘야함

SourceModule 만들기

- local, remote 따로 만들기

```kotlin
@Module
@InstallIn(SingletonComponent::class)
interface LocalSourceModule {

    @Binds
    @Singleton
    @LocalDiariesSource
    fun bindsDiariesLocalSource(source: DiariesLocalSource): DiariesSource
}
```

```kotlin
@Module
@InstallIn(SingletonComponent::class)
interface RemoteSourceModule {

    @Binds
    @Singleton
    @RemoteDiariesSource
    fun bindsDiariesRemoteSource(source: DiariesRemoteSource): DiariesSource
}
```



RepositoryModule 생성 

```kotlin
@Module
@InstallIn(SingletonComponent::class)
interface RepositoryModule {

    @Binds
    @Singleton
    fun bindsDiariesRepository(repository: RealDiariesRepository): DiariesRepository
}
```



ViewModel에 가서 @hiltViewModel, @Inject constructor 붙이기

Activity가서 viewModelFactory코드 지우기 & @AndroidEntryPoint (viewModel 초기화 코드는 안바뀜)



DI 방식 

constructor Injection

Field Injection (이걸 사용하려면 **AndroidEntryPoint**)

Viewmodel은 왜 Field Injection 을 사용하나?  

-> Activity, Fragment와 같은 안드로이드 class의 경우에는 어쩔수 없이 사용함



Application 파일 생성하고 Manifests에 등록해야함 



### 📚Reference

1. [Koin Docs](https://insert-koin.io/docs/quickstart/kotlin)
2. [Hilt Docs](https://dagger.dev/hilt/quick-start)
