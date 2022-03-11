# [Week2] ListAdapter, DiffUtil과 ActivityResultsContracts

<br/>

## 0. `@tools:sample/*` Resourse


> [Android Developer User Guide](https://developer.android.com/studio/write/tool-attributes?hl=ko#toolssample_resources)


```xml
tools:text="@tools:sample/first_names"
```


<br/><br/>


## 1. 기본적인 RecyclerView Adpater 구현

```kotlin
class Adapter: RecyclerView.Adapter<ViewHolder>(){

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder { }
    override fun onBindViewHolder(holder: ViewHolder, position: Int) { }
    override fun getItemCount(): Int { }
}
```

<br/><br/>



## 2. DiffUtil

> [Android Developer User Guide](https://developer.android.com/reference/androidx/recyclerview/widget/DiffUtil) 



### 2-1. 장점

- 변동점에 따라 자동으로 리스트를 변동시켜줌
- 성능이 좋음

<br/>


### 2-2. 단점

- 비교할 item마다 diffutil을 일일이 생성해 줘야 함(보일러 플레이팅 코드 증가)
- 복잡한 리스트를 갱신 할 경우, 수동으로 처리해 줘야 하는 경우가 있음

<br/>

### 2-3 사용방법

 RecyclerView + ListAdapter 사용을 위해서는

1. ViewHolder
2. DiffUtil
3. ListAdapter

```kotlin
    class ViewHolder(
        private val binding: ItemDiaryBinding,  // 생성자
    ) : RecyclerView.ViewHolder(binding.root) {

        fun bind(diary: Diary, onMemoClick: ((Diary) -> Unit)? = null) {
            binding.diary = diary
            binding.root.setOnClickListener { onMemoClick?.invoke(diary) }
        }
    }

    private class DiaryComparator : DiffUtil.ItemCallback<Diary>() {
        override fun areItemsTheSame(oldItem: Diary, newItem: Diary) = oldItem.id == newItem.id
        override fun areContentsTheSame(oldItem: Diary, newItem: Diary) = oldItem == newItem
    }

```

<br/><br/>


### 📚 Scope Function

위 코드는 아래 두 줄을 동시에 실행하는 의미를 가진다

```kotlin
binding.listDiaries.adapter = DiaryAdapter(::onMemoClick).also { diariesAdapter = it }
```

```kotlin
diariesAdapter = DiaryAdapter { onMemoClick(it) }
binding.listDiaries.adapter = diariesAdapter
```

also는 Scope Fucntion의 한 종류로써, kotlin에서는 `apply, run, with, also, let` 의 5가지 함수를 제공한다.

**[Reference]**

[언제 뭘 써야 돼? 헷갈리는 스코프 함수 한 방 정리](https://haero.tistory.com/21)

[Kotlin Scoping Functions apply vs. with, let, also, and run](https://medium.com/@fatihcoskun/kotlin-scoping-functions-apply-vs-with-let-also-run-816e4efb75f5)

[코틀린 의 apply, with, let, also, run 은 언제 사용하는가?](https://medium.com/@limgyumin/%EC%BD%94%ED%8B%80%EB%A6%B0-%EC%9D%98-apply-with-let-also-run-%EC%9D%80-%EC%96%B8%EC%A0%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94%EA%B0%80-4a517292df29)

<br/><br/>


### 📚 Adapter 할당

```kotlin
binding.listDiaries.adapter = DiaryAdapter(::onMemoClick)
binding.listDiaries.adapter = DiaryAdapter({onMemoClick(it)}) 
binding.listDiaries.adapter = DiaryAdapter{onMemoClick(it)}  // convention
```


<br/><br/>


### 📝 ListAdapter를 상속받은 RecyclerView

ListAdapter는 RecyclerViewAdapter를 상속받았다.

DiffUtil이 생겨난 배경은, RecyclerViewAdapter에서 notify 어쩌구들을 일일이 호출하는게 귀찮아서..

이러한 DiffUtil과 환상의 케미를 자랑하는 것이 LIstAdapter이다.

ListAdpater에는 AsyncDiffer가 내장되어 있어서, DiffUtil의 비교 연산을 비동기적으로 처리해준다.

- ListAdapter의 Property로 mutableList가 정의되어있다. 따라서 ListAdpater를 상속받은 RecyclerView를 사용할 때는, mutableList를 개발자가 직접 정의해주지 않아도 된다.
- ListAdapter에는 submitList라는 메소드가 있는데, 이친구도 asyncDiffer에 기반하여 작동하기에 비동기적으로 처리된다.
![image](https://user-images.githubusercontent.com/59546818/157931879-071ce44d-d96d-4f4c-8e9a-deb9331a37b4.png)
- submitList를 Runnable 객체와 함께 사용하는 경우에는, 아래와 같이 사용할 수 있다
![image](https://user-images.githubusercontent.com/59546818/157931936-6bc3dfe5-884b-47b5-a0a7-221d92e94d35.png)



<br/><br/>


## 3. registerForActivityResult

#### A에서 B를 호출하고, B의 결과를 A에서 알아야 하는 경우

> 호출하는 Activity

```kotlin
class DiaryActivity : AppCompatActivity() {
    private lateinit var editDiaryActivityLauncher: ActivityResultLauncher<Intent>

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        
        editDiaryActivityLauncher =
            registerForActivityResult(ActivityResultContracts.StartActivityForResult()) {
                when (it.resultCode) {
                    Activity.RESULT_OK -> showToast("작정 완료!")
                    Activity.RESULT_CANCELED -> showToast("작성 취소.")
                    else -> showToast("문제가 발생했습니다 : $it")
                }
            }
    }
    
    // 실제 실행시 : editDiaryActivityLauncher.launch(intent)
```

<br/>

> 반환해주는 Activity

```kotlin
binding.buttonSubmit.setOnClickListener {
    setResult(Activity.RESULT_OK)
    finish()
}
```

ActivityResultContracts 를 잘 숙지하면, 코드를 깔끔하게 작성할 수 있다고 한다! [말리빈 블로그](https://modelmaker.tistory.com/entry/Android-StartActivityForResult-Deprecated)

buttonSubmit을 누를 시에만 RESULT_OK를 지정했는데, onBackPressed를 통해 이전 뷰로 돌아갈 때에는 default로 RESULT_CANCELED가 호출되는듯 하다!

<br/><br/>


#### cf) Intent의 putExtra는..? A-> B -> A 구조가 아니라, A -> B 구조에서 사용

putExtra( name : String!, value : 자료형 ) 에서 name은
A 혹은 B Activity의 Companion Object로 지정해서 쓰면 유지보수하기 편리하다.

<br/><br/>



## 4. RecyclerView Adapter 생성 시 clickListener를 고차함수로 정의하기

```kotlin
class DiaryAdapter(
    private val onDiaryClick: ((Diary) -> Unit)? = null
) : ListAdapter<Diary, DiaryAdapter.ViewHolder>(DiaryComparator()) {
    
    ...
}
```

1. onDiaryClick() 람다함수를 DiaryAdapter()의 생성자로 지정해준다.
2. 실질적으로 ClickListener를 사용할 코드는, ViewHolder Class의 bind 함수 내에서 사용을 할 것이다.
3. ViewHolder의 bind메소드로 onClick 람다 함수를 받아야 한다
4. 3번의 람다함수는 onBindViewHolder 메소드 내에서 정의해준다
5. 4번에서 넘겨누는 클릭리스너 생성자는, 1번에서 생성한 클릭리스너이다.
