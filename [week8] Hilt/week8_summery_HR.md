## [Week9] Hilt



### ๐ DI๊ฐ ๋ญ๊ฐ?

Car๊ณผ Engine์์ ๋ก ์ค๋ช ํด์ค.. ๋ค๋ ๊ณต์๋ฌธ์์์ ํ๋ฒ์ฏค ๋ดค์๋ฒํ~.~



### ๐ DI๋ฅผ ํ๋ ๋ฐฉ๋ฒ

1. ์๋
   - ์๋ DI๋ฅผ **์๋น์ค๋ก์ผ์ดํฐ ํจํด**์ด๋ผ๊ณ  ํ๋๋ฐ, ์ํฐํจํด์ 

2. ์๋(๋ผ์ด๋ธ๋ฌ๋ฆฌ)

   - ๋ผ์ด๋ธ๋ฌ๋ฆฌ์๊ฒ ๊ฐ์ฒด๋ฅผ ๋ง๋๋ ๋ฐฉ๋ฒ๋ง ์๋ ค์ฃผ๋ฉด, ๊ฐ์ฒด๋ฅผ ์๋ ค์ค

   - ๊ฐ์ฒด๋ฅผ ์์ฑํ๊ณ  ํ ๋นํ๊ณ  ์ฐ๋๊ฒ - ์๋๋ ์ฐ๋ฆฌ๊ฐ ์ ์ดํ์๋๋ฐ, DI ํด์ ์ฐ๋ฉด ์ด๊ฑธ ์์ฒด์ ์ผ๋ก ํด์ค.

   - ๊ทธ๋ ๋ค๋ฉด ์ด ์ ์ด๊ถ์ ํ๋ก๊ทธ๋๋จธ IOC - Inversion of Control



### ๐ DI Tool์ ์ข๋ฅ

1. Koin
2. Kodein
3. Dagger
4. Hilt
5. Spring Container(Spring์ ์ฉ)

- koin, dagger, hilt ๋ ์ธ์ด ๋ฌด๊ดํ๊ฒ ์ ์ฉ๊ฐ๋ฅํจ. ์๋์์๋ง ์ธ์์๋ค๋ ์๊ฐ์ ์ฐฉ๊ฐ์.



### ๐ DI ๋ฐฉ์

1. **Runtime ๋ฐฉ์**

   - ๋ฐฉ์ : ๊ฐ์ฒด๋ฅผ ์ฌ์ฉํ๋ ์.๊ฐ. ๋ผ์ด๋ธ๋ฌ๋ฆฌ๊ฐ ๊ฐ์ฒด๋ฅผ ์ฐพ์

   - ๋ผ์ด๋ธ๋ฌ๋ฆฌ : Koin, Kodein

   - ์ฅ์ 

     - ๋ฌ๋์ปค๋ธ๊ฐ ๋ฎ์ - Service Locator์ ์ ์ฌํจ
     - ์ปดํ์ผํ์์ Compile ๋ฐฉ์์ ๋นํด ๋น ๋ฆ

   - ๋จ์  

     - ๊ฐ์ฒด๊ฐ ํ์ํ ๋ ์์กด์ฑ ์ถ๊ฐ ๋ก์ง์ ์คํํ๊ธฐ์, ํด๋จผ์๋ฌ๊ฐ ๋ฐ์ํ  ๊ฒฝ์ฐ ๋ฐํ์์ ์๋ฌ๊ฐ ๋จ
     - ์๋์ ์ผ๋ก ๋๋ฆผ - ์คํ์์ ๊ฐ์ฒด๋ฅผ ๋ง๋ค๊ธฐ ๋๋ฌธ์(์์ธํ๋ ์์กด์ฑ ํธ๋ฆฌ๋ฅผ ํตํด ์์๋ด๋ ๋ก์ง)

     

2. **Compile ๋ฐฉ์**

   - ๋ฐฉ์ : ์ ์ด์ build ํ ๋ ๊ฐ์ฒด๋ฅผ ์์ฑํ๊ณ  ๋ฃ๋ ๋ก์ง์, kapt๋ฅผ ํตํด ์์ฑ๋จ
   - ๋ผ์ด๋ธ๋ฌ๋ฆฌ : Hilt, Dagger

   - ์ฅ์ 
     - Dagger์ ๋นํด ์ฌ์์ง cf. Dagger : Learning Curve๊ฐ ๋ฏธ์ณค์, Boiler Plate ์ฝ๋๊ฐ ๋ง์
     - ์ปดํ์ผ ํ์์ ์์กด์ฑ ํธ๋ฆฌ๋ฅผ ๊ณ์ฐํ๊ธฐ ๋๋ฌธ์, ํด๋จผ์๋ฌ๊ฐ ๋ฐ์ํ  ๊ฒฝ์ฐ ์ปดํ์ผ ํ์์ ๋ฏธ๋ฆฌ ํ์ธํ  ์ ์์(๋จ, ์ด๋ ์๊ฐ์  ์ธก๋ฉด์์ ๋จ์ ์ผ๋ก ์ด์ด์ง)
     - ์ปดํ์ผ ํ์์ ๋ง์ ์ผ์ ํ๊ธฐ์, ๋ฐํ์์ด ๋น ๋ฆ - ๊ฐ์ฒด์์ฑํ๋๊ฒ ๋์ 
   - ๋จ์ 
     - ์ปดํ์ผ์ ํ  ๋ kapt๊ฐ ์ฝ๋๋ฅผ ์์ฑํด์ฃผ๋ ๊ณผ์ ์์ ์ปดํ์ผํ์์ด ๋๋ฌด ๋๋ ค์ง



### ๐ SDK ์ถ๊ฐ

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



// ์ ์ค๊ฐ์ ์ฐ๋ฝ์์ ์ํฉ ๋์นจ...... RoomModule ์์ฑ ๋ถํฐ ๋ค์ ๋ค์

์๋ฐ ์ธ์ ํธ์ ์๋ ์ด๋ธํ์ด์์ ์ฌ์ฉํจ

@Module : ๋ ๋ชจ๋์ด์ผ

@InstallIn(SingletoneCompoenet::class) : ์๋ช์ฃผ๊ธฐ ์ด์ผ ํ ๋

@Provides

@Singletone



hilt๋ context๋ฅผ ๋ค ์๋ค

`@ApplicationContext context: Context` ์ด๊ฑฐ ์์ด๋ context ์ฃผ์ ๊ฐ๋ฅ



annotation ๋ง๋ค๊ธฐ (DiariesSource interface๋ฅผ ์์ํ๋ ์น๊ตฌ๋ค์ด 2๊ฐ๊ฐ ์์ผ๋ฏ๋ก ..? ์๋ง๋..?)

```kotlin
@Qualifier
@Retention(AnnotationRetention.BINARY)
annotation class LocalDiariesSource

@Qualifier	// hilt ์ธ์
@Retention(AnnotationRetention.BINARY)
annotation class RemoteDiariesSource
```

์ด๊ฑธ hilt์ ์๋ ค์ค์ผํจ

SourceModule ๋ง๋ค๊ธฐ

- local, remote ๋ฐ๋ก ๋ง๋ค๊ธฐ

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



RepositoryModule ์์ฑ 

```kotlin
@Module
@InstallIn(SingletonComponent::class)
interface RepositoryModule {

    @Binds
    @Singleton
    fun bindsDiariesRepository(repository: RealDiariesRepository): DiariesRepository
}
```



ViewModel์ ๊ฐ์ @hiltViewModel, @Inject constructor ๋ถ์ด๊ธฐ

Activity๊ฐ์ viewModelFactory์ฝ๋ ์ง์ฐ๊ธฐ & @AndroidEntryPoint (viewModel ์ด๊ธฐํ ์ฝ๋๋ ์๋ฐ๋)



DI ๋ฐฉ์ 

constructor Injection

Field Injection (์ด๊ฑธ ์ฌ์ฉํ๋ ค๋ฉด **AndroidEntryPoint**)

Viewmodel์ ์ Field Injection ์ ์ฌ์ฉํ๋?  

-> Activity, Fragment์ ๊ฐ์ ์๋๋ก์ด๋ class์ ๊ฒฝ์ฐ์๋ ์ด์ฉ์ ์์ด ์ฌ์ฉํจ



Application ํ์ผ ์์ฑํ๊ณ  Manifests์ ๋ฑ๋กํด์ผํจ 



### ๐Reference

1. [Koin Docs](https://insert-koin.io/docs/quickstart/kotlin)
2. [Hilt Docs](https://dagger.dev/hilt/quick-start)
