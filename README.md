# ๐ Week 1



๐ฃ **ํ๋ฉด ์ ํ๊ณผ ๋ฐ์ดํฐ ๋ณด๋ด๊ธฐ**

- SignInActivity -> HomeActivity

```kotlin
val intent = Intent(this, HomeActivity::class.java)
startActivity(intent)
```

- SignInActivity -> SignUpActivity

```kotlin
private val signUpActivityLauncher = registerForActivityResult(
    ActivityResultContracts.StartActivityForResult()
) {
    // ๋ฐ์ดํฐ๋ฅผ ๋ฐ์์ ํ  ์ผ์ด ๋ค์ด๊ฐ๋ ์นธ!
}

val intent = Intent(this, SignUpActivity::class.java)
signUpActivityLauncher.launch(intent)
```

- SignUpActivity -> SignInActivity

```kotlin
val intent = Intent()
intent.putExtra("name", binding.edtName.text.toString())
intent.putExtra("id", binding.etId.text.toString())
intent.putExtra("password", binding.etPassword.text.toString())
setResult(Activity.RESULT_OK, intent)
finish()
```



๐ฃ **์๋ช์ฃผ๊ธฐ์ Log๋์ฐ๊ธฐ**

![KakaoTalk_20210411_190958314](https://user-images.githubusercontent.com/70002218/114304910-9ebe8480-9b10-11eb-86b9-a8f89eba13ad.png)



๐ฃ **์ถ๊ฐ**

๊ณต๋ถํด์ผ ํ  ๋ด์ฉ์ด ๋ง๋ค.. ๋ณต์ต ํ์!





# ๐ฅ Week 2  

  

๐ **LEVEL-1**  



๐ฃ **๋ ํฌ์งํฐ๋ฆฌ ๋ฆฌ์คํธ ๋์ฐ๊ธฐ(RecyclerView)**  

- **item_repo.xml**

  - ์กฐ๊ฑด 2๊ฐ์ง

  - ๋ ํฌ์งํฐ๋ฆฌ ์ด๋ฆ, ์ค๋ช์ด ๊ธด ๊ฒฝ์ฐ ๋ค์ ... ๋ถ์ด๊ธฐ

  - **ellipsize, maxLines** ์ด์ฉ

```kotlin
<TextView
        android:id="@+id/tv_repo_name"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:textSize="18sp"
        android:textStyle="bold"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        android:ellipsize="end" //๋ค์ ... ๋ถ์ด๊ธฐ
        android:maxLines="1" //์ต๋ 1์ค
        tools:text="@string/repo_name"/>
```

- **RepoInfo** data class ์ ์

```kotlin
data class RepoInfo (
        val repoName: String,
        val repoDisc: String,
        val repoLanguage: String
)
```

- **RepoListAdapter** ์ ์

```kotlin
class RepoListAdapter : RecyclerView.Adapter<RepoListAdapter.RepoViewHolder> () {

    val repoList = mutableListOf<RepoInfo>() //๋ฐ์ดํฐ์ ๋ณด

    //๋ทฐํ๋์๊ฒ ํ์ฌ ์ํฉ ์๋ ค์ฃผ๊ธฐ
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): RepoViewHolder { 
        val binding = ItemRepoBinding.inflate(
            LayoutInflater.from(parent.context),
            parent,
            false
        ) 
        return RepoViewHolder(binding)
    }

    override fun getItemCount(): Int = repoList.size //๋ฐ์ดํฐ์ ์ด ๊ฐ์๋ฅผ ์๋ ค์ค

    override fun onBindViewHolder(holder: RepoViewHolder, position: Int) {
        holder.onBind(repoList[position]) //๋ฐ์ดํฐ๋ฅผ ์ํ๋ ์์น๋ถํฐ ๋ทฐ์ ๋ฌถ๊ธฐ
    }

    class RepoViewHolder(
            private val binding: ItemRepoBinding
    ) : RecyclerView.ViewHolder(binding.root) {
        fun onBind(repoInfo: RepoInfo) {
            binding.tvRepoName.text = repoInfo.repoName
            binding.tvRepoDisc.text = repoInfo.repoDisc
            binding.tvRepoLanguage.text = repoInfo.repoLanguage
        }
    }

}
```

- HomeActivity์์ **initRepoListRecyclerView()**ํจ์
  - repoList์ ๋ฐ์ดํฐ๋ฅผ ๋ด์ Adapter์ ์ ๋ฌ

```kotlin
private fun initRepoListRecyclerView() {
        repoListAdapter = RepoListAdapter()

        binding.rvRepoList.adapter = repoListAdapter

        repoListAdapter.repoList.addAll(
            listOf<RepoInfo>(
                RepoInfo(
                    repoName = "REPO 1 : And-Potter\\hufflepuff_onpyeong",
                    repoDisc = "SOPT 28๊ธฐ ์๋๋ก์ด๋ 3์กฐ ํํํธํ ๊ธฐ์์ฌ์ ๊ณผ์  ์ ์ถํจ" +
                            "\n#YB #Android #hufflepuff #Seminar #Assignment #๋ฐฐ์๊ฐ๋ ์ค ",
                    repoLanguage = "kotlin"
                ),
                RepoInfo(
                    repoName = "REPO 2 : mdb1217\\Baekjoon_Jojeo",
                    repoDisc = "์ผ์ฃผ์ผ ๋ฐฑ์ค 2๋ฌธ์  ํ๊ธฐ ์คํฐ๋" +
                            "\n#BOJ #Challenge #Study",
                    repoLanguage = "c++  java"
                ),
                RepoInfo(
                    repoName = "REPO 3 : onpyeong\\BOJ_Challenge",
                    repoDisc = "๋ฐฑ์ค ํ๋ฃจ ํ๋ฌธ์  ์ด์ ํ๊ธฐ ์ฑ๋ฆฐ์ง" +
                            "\n#BOJ #Challenge #Class3",
                    repoLanguage = "c++"
                ),
                RepoInfo(
                    repoName = "REPO 4 : onpyeong\\gitbootest",
                    repoDisc = "๊น๋ฟ ์คํฐ๋ ์ฐ์ต์ฉ" +
                            "\n#Git #Github #Study",
                    repoLanguage = "kotlin"
                ),
                RepoInfo(
                    repoName = "REPO 5 : BackEndBoostCamp\\Assignment",
                    repoDisc = "๋ฐฑ์๋ ์คํฐ๋ ๊ณผ์  ์ ์ถํจ" +
                            "\n#BackEnd #Study",
                    repoLanguage = "java"
                )
            )
        )
    }
```
  


๐ฃ **more๋ฒํผ ๋๋ฅด๋ฉด UserInfoActivity๋ก ์ด๋ํ๊ธฐ**  

- **userInfoActivityLauncher** ์ ์

```kotlin
private val userInfoActivityLauncher = registerForActivityResult(
            ActivityResultContracts.StartActivityForResult()
    ) {
        // ๋ฐ์ดํฐ๋ฅผ ๋ฐ์์ ํ  ์ผ์ด ๋ค์ด๊ฐ๋ ์นธ!
    }
```

- **moreButtonClickEvent()**

```
private fun moreButtonClickEvent() {
    binding.btnMore.setOnClickListener {
        val intent = Intent(this, UserInfoActivity::class.java)
        userInfoActivityLauncher.launch(intent)
    }
}
```





# ๐ฅ Week 4



๐ **LEVEL-1**



๐ฃ **PostMan test**

![postman](https://user-images.githubusercontent.com/70002218/118391260-601b6d00-b66e-11eb-9a16-f43e18f8f7a6.png)



๐ฃ **ํ์๊ฐ์ ์๋ฃ gif**

![signup2](https://user-images.githubusercontent.com/70002218/118391230-306c6500-b66e-11eb-972a-301bb934a10f.gif)



๐ฃ **๋ก๊ทธ์ธ ์๋ฃ gif**

![login](https://user-images.githubusercontent.com/70002218/118391209-192d7780-b66e-11eb-8c64-460161593985.gif)



๐ฃ **๋ก๊ทธ์ธ/ ํ์๊ฐ์ ์๋ฒํต์ **



**1. Retrofit Interface ์ค๊ณ**

```kotlin
interface SoptService {
    @POST("/login/signin")
    fun postLogin(
        @Body body: RequestLoginData
    ) : Call<ResponseLoginData>

    @POST("/login/signup")
    fun postSignUp(
        @Body body: RequestSignUpData
    ) : Call<ResponseSignUpData>
}
```



**2. ์๋ฒ Request, Response ๊ฐ์ฒด ์ค๊ณ**

- json๊ณผ ๋์ผํ๊ฒ data class๋ก ๋ง๋ฆ

- RequestLoginData

```kotlin
data class RequestLoginData(
    @SerializedName("email")
    val id: String,
    val password: String
)
```

- ResponseLoginData

```kotlin
data class ResponseLoginData(
    val success: Boolean,
    val message: String,
    val data: LoginData?
)

data class  LoginData(
    @SerializedName("UserId")
    val userId: Int,
    val user_nickname: String,
    val token: String
)
```



**3. Retrofit Interface ๊ตฌํ์ฒด**

- Singleton ํจํด

```kotlin
object ServiceCreator {
    private const val BASE_URL = "http://cherishserver.com"

    private val retrofit: Retrofit = Retrofit.Builder()
        .baseUrl(BASE_URL)
        .addConverterFactory(GsonConverterFactory.create())
        .build()

    val soptService: SoptService = retrofit.create(SoptService::class.java)
}
```



**4. callback ๋ฑ๋ก, ์๋ฒ ํต์  ๊ตฌํ**

- Call<Type> : ๋๊ธฐ ํน์ ๋น๋๊ธฐ์ ์ผ๋ก Type๋ฅผ ์๋ฒ์์ ๋ฐ์์ค๋ ๊ฐ์ฒด

- Callback<Type> : Type ๊ฐ์ฒด๋ฅผ ๋น๋๊ธฐ์ ์ผ๋ก ๋ฐ์์์๋, ํ๋ก๊ทธ๋๋จธ๊ฐ ํ  ํ๋

  

- **๋ก๊ทธ์ธ** 

```kotlin
binding.btnLogin.setOnClickListener {
            // ์๋ฒ๋ก ๋ณด๋ผ ๋ก๊ทธ์ธ ๋ฐ์ดํฐ ์์ฑ
            val requestLoginData = RequestLoginData(
                id = binding.etId.text.toString(),
                password = binding.etPassword.text.toString()
            )
            // ํ์ฌ ์ฌ์ฉ์์ ์ ๋ณด๋ฅผ ๋ฐ์์ฌ ๊ฒ์ ๋ช์
            // ์๋ฒ ํต์ ์ I/O ์์์ด๋ฏ๋ก ๋น๋๊ธฐ์ ์ผ๋ก ๋ฐ์์ฌ Callback ๋ด๋ถ ์ฝ๋๋ ๋์ค์
            // ๋ฐ์ดํฐ๋ฅผ ๋ฐ์์ค๊ณ  ์คํ๋จ

            /* enqueue ํจ์๋ฅผ ์ด์ฉํด Call์ด ๋น๋๊ธฐ ์์ ์ดํ ๋์ํจ Callback์ ๋ฑ๋กํ  ์ ์์
            * ํด๋น ํจ์ ํธ์ถ์ Callback์ ๋ฑ๋ก๋ง ํ๊ณ 
            * ์ค์  ์๋ฒ ํต์ ์ ์์ฒญ์ดํ ํต์  ๊ฒฐ๊ณผ๊ฐ ๋์์ ๋ ์คํ๋จ*/
            // object ํค์๋๋ก Callback์ ๊ตฌํํ  ์ต๋ช ํด๋์ค ์์ฑ

            val call: Call<ResponseLoginData> = ServiceCreator.soptService
                .postLogin(requestLoginData)
            call.enqueue(object : Callback<ResponseLoginData> {
                // ๋คํธ์ํฌ ํต์  Response๊ฐ ์๋ ๊ฒฝ์ฐ ํด๋น ํจ์๋ฅผ retrofit์ด ํธ์ถ
                override fun onResponse(
                    call : Call<ResponseLoginData>,
                    response: Response<ResponseLoginData>
                ) {
                    // ๋คํธ์ํฌ ํต์ ์ ์ฑ๊ณตํ ๊ฒฝ์ฐ status ์ฝ๋๊ฐ 200~300์ผ ๋ ์คํ
                    if(response.isSuccessful) {
                        // response body ์์ฒด๊ฐ nullable ๋ฐ์ดํฐ, ์๋ฒ์์ ์ค๋ data๋ nullable
                        val data = response.body()?.data
                        
                        // ํ ํ๋ฉด์ผ๋ก ๋์ด๊ฐ
                        val intent = Intent(this@SignInActivity, HomeActivity::class.java)
                        startActivity(intent)
                        Toast.makeText(this@SignInActivity, "๋ก๊ทธ์ธ ์ฑ๊ณต", Toast.LENGTH_SHORT).show()

                    } else {
                        // ์๋ฒ ํต์  status๊ฐ 200~300์ด ์๋ ๊ฒฝ์ฐ
                        Toast.makeText(this@SignInActivity, "์์ด๋/๋น๋ฐ๋ฒํธ๋ฅผ ํ์ธํด์ฃผ์ธ์!", Toast.LENGTH_SHORT).show()
                    }
                }

                // ๋คํธ์ํฌ ํต์  ์์ฒด๊ฐ ์คํจํ ๊ฒฝ์ฐ ํด๋น ํจ์๋ฅผ retrofit์ด ์คํ
                override fun onFailure(call: Call<ResponseLoginData>, t: Throwable) {
                    Log.d("NetworkTest", "error:$t")
                }
            })
```


    
# ๐ฅ Week 7



๐ **LEVEL-1**



๐ฃ  **์๋ ๋ก๊ทธ์ธ gif**

![autologin](https://user-images.githubusercontent.com/70002218/121028409-c49b9900-c7e2-11eb-8e36-60a5b5a0495b.gif)



๐ฃ **์๋ ๋ก๊ทธ์ธ ๊ธฐ๋ฅ**



**1. SharedPreference์ ID/PW๊ฐ ์๋ค๋ฉด ์๋ ๋ก๊ทธ์ธ**

- ํ๋ฉด ์ ํ

```kotlin
private fun searchUserAuthStorage() {
        if(SoptUserAuthStorage.getAutoLogin()) {
            val intent = Intent(this@SignInActivity, HomeActivity::class.java)
            startActivity(intent)
            toast("๋ก๊ทธ์ธ ์ฑ๊ณต")
        }
    }
```



**2. ๋ก๊ทธ์ธ ์ฑ๊ณตํ๋ฉด SharedPreference์ true๊ฐ ๋ฃ๊ธฐ**

- loginButtonClickEvent() 

```kotlin
private fun loginButtonClickEvent() {
        binding.btnLogin.setOnClickListener {
            val requestLoginData = RequestLoginData(
                id = binding.etId.text.toString(),
                password = binding.etPassword.text.toString()
            )
            requestLogin(requestLoginData)
        }
    }
```

- requestLogin()

```kotlin
private fun requestLogin(requestLoginData: RequestLoginData) {
        // ํ์ฌ ์ฌ์ฉ์์ ์ ๋ณด๋ฅผ ๋ฐ์์ฌ ๊ฒ์ ๋ช์
        // ์๋ฒ ํต์ ์ I/O ์์์ด๋ฏ๋ก ๋น๋๊ธฐ์ ์ผ๋ก ๋ฐ์์ฌ Callback ๋ด๋ถ ์ฝ๋๋ ๋์ค์
        // ๋ฐ์ดํฐ๋ฅผ ๋ฐ์์ค๊ณ  ์คํ๋จ

        /* enqueue ํจ์๋ฅผ ์ด์ฉํด Call์ด ๋น๋๊ธฐ ์์ ์ดํ ๋์ํจ Callback์ ๋ฑ๋กํ  ์ ์์
        * ํด๋น ํจ์ ํธ์ถ์ Callback์ ๋ฑ๋ก๋ง ํ๊ณ 
        * ์ค์  ์๋ฒ ํต์ ์ ์์ฒญ์ดํ ํต์  ๊ฒฐ๊ณผ๊ฐ ๋์์ ๋ ์คํ๋จ*/
        // object ํค์๋๋ก Callback์ ๊ตฌํํ  ์ต๋ช ํด๋์ค ์์ฑ

        val call: Call<ResponseLoginData> = ServiceCreator.soptService
            .postLogin(requestLoginData)
        call.enqueue(object : Callback<ResponseLoginData> {
            // ๋คํธ์ํฌ ํต์  Response๊ฐ ์๋ ๊ฒฝ์ฐ ํด๋น ํจ์๋ฅผ retrofit์ด ํธ์ถ
            override fun onResponse(
                call : Call<ResponseLoginData>,
                response: Response<ResponseLoginData>
            ) {
                // ๋คํธ์ํฌ ํต์ ์ ์ฑ๊ณตํ ๊ฒฝ์ฐ status ์ฝ๋๊ฐ 200~300์ผ ๋ ์คํ
                if(response.isSuccessful) {
                    // response body ์์ฒด๊ฐ nullable ๋ฐ์ดํฐ, ์๋ฒ์์ ์ค๋ data๋ nullable
                    val data = response.body()?.data
                    // ํต์  ์ฑ๊ณต ์ ์ ์  ๋๋ค์์ ๋ณด์ฌ์ค
                    toast(data?.user_nickname)
                    if(!SoptUserAuthStorage.getAutoLogin()){
                        SoptUserAuthStorage.saveAutoLogin(true)
                    }
                    // ํ ํ๋ฉด์ผ๋ก ๋์ด๊ฐ
                    val intent = Intent(this@SignInActivity, HomeActivity::class.java)
                    startActivity(intent)
                    toast("๋ก๊ทธ์ธ ์ฑ๊ณต")

                } else {
                    // ์๋ฒ ํต์  status๊ฐ 200~300์ด ์๋ ๊ฒฝ์ฐ
                    // ์๋ฌ๊ฐ ๋ฌ๋ค๋ฉด ์ด๋ป๊ฒ ํด์ผ ํ ์ง...
                        Log.d("์คํจ", "${requestLoginData.id} ${requestLoginData.password}")
                        toast("์์ด๋/๋น๋ฐ๋ฒํธ๋ฅผ ํ์ธํด์ฃผ์ธ์!")
                }
            }

            // ๋คํธ์ํฌ ํต์  ์์ฒด๊ฐ ์คํจํ ๊ฒฝ์ฐ ํด๋น ํจ์๋ฅผ retrofit์ด ์คํ
            override fun onFailure(call: Call<ResponseLoginData>, t: Throwable) {
                Log.d("NetworkTest", "error:$t")
            }
        })
    }
```



**3. ๋ก๊ทธ์์ํ๋ฉด SharedPreference clearํ๊ธฐ**

```kotlin
 private fun logoutButtonClickEvent() {
        binding.btnLogout.setOnClickListener{
            SoptUserAuthStorage.clearAutoLogin()
            navigateSignIn()
            toast("๋ก๊ทธ์์")
        }
    }
```


