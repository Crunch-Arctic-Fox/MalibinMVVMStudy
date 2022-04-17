## [Week7] Coroutine



```kotlin
val job : Job = CoroutineScope(Dispatchers.[Main/IO/Default/Unconfined]).launch{
	withContext(Dispatcher.Main){
        // 부분적으로 main thread 에서 실행가능
    }
   	// job을 return함 -> job.cancel() 코드로 스코프 외부에서 중지 가능
    // 즉, 생명주기를 직접 다룰 수 있다
}

```

📌Dispatcher

- main, io -> 안드로이드 여서 붙은 용어다
- backgournd 상태에서 UI 변경 불가능

📌실행

- launch : 실행 결과를 리턴받지 않아도 되는 경우 (주로 사용)
- async : 리턴값이 있는 경우

📌 Scope

- global scope 권장 X (memory leak, resource 낭비)

- viewModelScope 사용

- activity에서는 lifecycleScope 사용



suspend가 붙어있는 메소드가 background에서 실행된다는 것은 오해다!



### AndroidViewModel(application) 걷어내기

viewmodelProvider

viewmodelStore

```kotlin
class EditDiaryViewModel(
    private val diariesDao: DiariesDao,
    private val malibinService: MalibinService
) : ViewModel() {... }

private val editDiaryViewModel: EditDiaryViewModel by viewModels {
    EditDiaryViewModelFactory(this)
}

class EditDiaryViewModelFactory(private val context: Context) : ViewModelProvider.Factory {
    override fun <T : ViewModel> create(modelClass: Class<T>): T {
        return when (modelClass) {
            EditDiaryViewModel::class.java -> EditDiaryViewModel(
                DailyDiaryDatabase.getInstance(context).getDiariesDao(),
                MalibinService.getInstance()
            )
            else
            -> throw IllegalArgumentException("Input Class : ${modelClass.simpleName} cannot create this EditDiaryViewModelFactory")
        } as T
    }
}
```
