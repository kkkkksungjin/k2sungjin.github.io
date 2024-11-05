---
layout: default
title: About DI
parent: Android
grand_parent: Google
nav_order: 1
---
# About DI

<hr/>

## __Dagger2 vs Hilt__{: style="color: #00c0ef"}


| 特性                    | **Dagger 2**                                  | **Hilt**                                |
|-------------------------|-----------------------------------------------|-----------------------------------------|
| **設定とコードの作成**   | 複雑で設定が多い                              | 簡単で設定が自動化されている               |
| **柔軟性**               | 高い柔軟性                                    | 制限された柔軟性                           |
| **ViewModelの注入**     | ViewModelFactoryが必要                       | @HiltViewModelで簡略化                    |
| **Android依存性**       | 独立（Androidと無関係）                       | Android専用                               |
| **ビルド速度**          | 遅い                                        | 速い                                      |
| **テストサポート**      | 設定が必要                                    | Hiltが自動でサポート                       |
| **複雑な依存関係の管理**  | 大規模プロジェクトに適している                  | シンプルな依存関係の注入に適している        |
| **ランタイム性能**       | コンパイル時に依存関係を注入し、性能に有利     | 一部ランタイムでの注入があり、性能への影響がある可能性 |

HiltはAndroidプロジェクト向けに使いやすく、Dagger 2は柔軟性が高く大規模プロジェクトにも適しています。

## Common
~~~java
// User.kt
@Entity(tableName = "user_table")
data class User(
    @PrimaryKey val id: Int,
    val name: String,
    val age: Int
)
~~~

~~~java
// UserDao.kt
@Dao
interface UserDao {
    @Query("SELECT * FROM user_table")
    fun getAllUsers(): Flow<List<User>>

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertUser(user: User)
}
~~~

~~~java
// UserRepository.kt
class UserRepository @Inject constructor(
    private val userDao: UserDao
) {
    fun getAllUsers(): Flow<List<User>> = userDao.getAllUsers()
    suspend fun addUser(user: User) = userDao.insertUser(user)
}
~~~

## Dagger2
~~~java
// UserViewModel.kt
class UserViewModel @Inject constructor(
    private val userRepository: UserRepository
) : ViewModel() {

    val users: LiveData<List<User>> = userRepository.getAllUsers().asLiveData()

    fun addUser(user: User) {
        viewModelScope.launch {
            userRepository.addUser(user)
        }
    }
}
~~~

~~~java
// ViewModelFactory.kt
class UserViewModelFactory @Inject constructor(
    private val userViewModel: Provider<UserViewModel>
) : ViewModelProvider.Factory {

    override fun <T : ViewModel> create(modelClass: Class<T>): T {
        if (modelClass.isAssignableFrom(UserViewModel::class.java)) {
            return userViewModel.get() as T
        }
        throw IllegalArgumentException("Unknown ViewModel class")
    }
}
~~~

~~~java
// AppModule.kt
@Module
class AppModule {

    @Provides
    @Singleton
    fun provideDatabase(application: Application): UserDatabase =
        Room.databaseBuilder(application, UserDatabase::class.java, "user_database").build()

    @Provides
    fun provideUserDao(database: UserDatabase): UserDao = database.userDao()

    @Provides
    fun provideUserRepository(userDao: UserDao): UserRepository = UserRepository(userDao)
}
~~~

~~~java
// AppComponent.kt
@Singleton
@Component(modules = [AppModule::class])
interface AppComponent {
    fun inject(activity: MainActivity)
}
~~~

~~~java
// MainActivity.kt
class MainActivity : AppCompatActivity() {

    @Inject
    lateinit var viewModelFactory: ViewModelProvider.Factory
    private lateinit var userViewModel: UserViewModel

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        (application as MyApplication).appComponent.inject(this)

        userViewModel = ViewModelProvider(this, viewModelFactory).get(UserViewModel::class.java)
    }
}
~~~

~~~java
// UserFragment.kt
class UserFragment : Fragment() {

    private lateinit var userViewModel: UserViewModel

    override fun onAttach(context: Context) {
        super.onAttach(context)
        userViewModel = ViewModelProvider(this, (activity as MainActivity).viewModelFactory).get(UserViewModel::class.java)
    }
}
~~~


## Hilt 

~~~java
// UserViewModel.kt
@HiltViewModel
class UserViewModel @Inject constructor(
    private val userRepository: UserRepository
) : ViewModel() {
    val users: LiveData<List<User>> = userRepository.getAllUsers().asLiveData()
    fun addUser(user: User) {
        viewModelScope.launch {
            userRepository.addUser(user)
        }
    }
}
~~~

~~~java
// AppModule.kt
@Module
@InstallIn(SingletonComponent::class)
object AppModule {

    @Provides
    @Singleton
    fun provideDatabase(@ApplicationContext context: Context): UserDatabase {
        return Room.databaseBuilder(context, UserDatabase::class.java, "user_database").build()
    }

    @Provides
    fun provideUserDao(database: UserDatabase): UserDao = database.userDao()

    @Provides
    fun provideUserRepository(userDao: UserDao): UserRepository = UserRepository(userDao)
}
~~~

~~~java
// MainActivity.kt
@AndroidEntryPoint
class MainActivity : AppCompatActivity() {
    private val userViewModel: UserViewModel by viewModels()
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        // userViewModel can use
    }
}
~~~

~~~java
// UserFragment.kt
@AndroidEntryPoint
class UserFragment : Fragment() {
    private val userViewModel: UserViewModel by viewModels()
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        // userViewModel can use
    }
}

~~~



## __Koin vs Hilt__{: style="color: #00c0ef"}

| 特性               | **Koin**                                   | **Hilt**                                 |
|--------------------|--------------------------------------------|------------------------------------------|
| **設定方式**        | Kotlin DSLベース、簡単な設定が可能           | アノテーションベース、自動コード生成       |
| **注入方式**        | ランタイム注入、動的注入に適している          | コンパイル時注入、性能と安定性に有利       |
| **ライフサイクル管理** | 手動設定が必要                               | Androidライフサイクルと自動統合            |
| **ビルド速度**      | 速い（コンパイル時のコード生成なし）           | やや遅い（コンパイル時のコード生成あり）   |
| **柔軟性**          | 動的注入など高い柔軟性                       | 限定的な柔軟性、Dagger 2ベースの制限あり   |
| **テストサポート**    | 別途設定が必要                               | 自動設定、テスト環境に最適化               |
| **ランタイム性能**    | 大規模プロジェクトでは性能低下の可能性        | コンパイル時注入で大規模プロジェクトに適している |
| **互換性**          | Kotlin専用、マルチプラットフォーム対応可能    | Android専用、Java/Kotlinプロジェクトに対応 |


## Koin
~~~java
// MyApplication.kt
class MyApplication : Application() {
    override fun onCreate() {
        super.onCreate()
        startKoin {
            androidContext(this@MyApplication)
            modules(appModule)
        }
    }
}
~~~

~~~java
// UserViewModel.kt
class UserViewModel(private val userRepository: UserRepository) : ViewModel() {
    val users: LiveData<List<User>> = userRepository.getAllUsers().asLiveData()
    fun addUser(user: User) {
        viewModelScope.launch {
            userRepository.addUser(user)
        }
    }
}
~~~

~~~java
// AppModule.kt
val appModule = module {
    // Database
    single {
        Room.databaseBuilder(get(), UserDatabase::class.java, "user_database").build()
    }
    // UserDao 
    single { get<UserDatabase>().userDao() }
    // Repository 
    single { UserRepository(get()) }
    // ViewModel
    viewModel { UserViewModel(get()) }
}
~~~


### コンパイルタイム (Compile-time)
コンパイルタイムは、コードが機械が理解できる形式に変換される時点を指します。 <br/>
時間の順序: Android Studioでビルドを開始した直後からAPKが生成されるまでがコンパイルタイムです。

### ランタイム (Runtime)
ランタイムは、アプリが実際に実行される時点を指します。<br/>
時間の順序: ユーザーがアプリを起動した直後からアプリが終了するまでがランタイムです。

| 特性            | **コンパイルタイム注入**                   | **ランタイム注入**                   |
|-----------------|------------------------------------------|--------------------------------------|
| **注入のタイミング** | コンパイル時                            | ランタイム時                          |
| **メリット**     | 高いパフォーマンス、コンパイル中にエラーが検出される | 設定が簡単で柔軟性が高い               |
| **デメリット**   | 設定がやや複雑でコンパイル時間が長くなる    | ランタイムに注入エラーが発生する可能性  |
| **代表的なライブラリ** | Dagger 2, Hilt                        | Koin                                 |


### 一部ランタイムHilt
1. ApplicationContext 
~~~java
@Module
@InstallIn(SingletonComponent::class)
object AppModule {

    @Provides
    fun provideExampleDependency(@ApplicationContext context: Context): ExampleDependency {
        return ExampleDependency(context)
    }
}
~~~

2. ViewModelにSavedStateHandle主入
~~~java
@HiltViewModel
class MyViewModel @Inject constructor(
    private val repository: MyRepository,
    private val savedStateHandle: SavedStateHandle // ランタイム注入
) : ViewModel() {
    // ViewModel ロジック
}
~~~

3. Dynamic Injection - Assisted Injection<br/>
~~~java
@AssistedFactory
interface MyViewModelFactory {
    fun create(id: String): MyViewModel
}
~~~
~~~java
@HiltViewModel
class MyViewModel @AssistedInject constructor(
    private val repository: MyRepository,
    @Assisted private val id: String // ランタイム設定変数
) : ViewModel() {
    // ViewModel ロジック
}
~~~

4. EntryPoint 主入<br/>
~~~java
// EntryPoint インターフェース定義
@EntryPoint
@InstallIn(SingletonComponent::class)
interface MyEntryPoint {
    fun getRepository(): MyRepository
}
~~~
~~~java
// Hilt 管理外部クラスに注入
class SomeClass {
    fun doSomething(context: Context) {
        val entryPoint = EntryPointAccessors.fromApplication(context, MyEntryPoint::class.java)
        val repository = entryPoint.getRepository() // ランタイム主入
        repository.performAction()
    }
}
~~~