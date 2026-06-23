# 🟢 Kotlin `val` vs `var` — Complete Developer Guide

> **Level:** Beginner → Intermediate &nbsp;|&nbsp; **Language:** Kotlin &nbsp;|&nbsp; **Focus:** Android Development

---

## 📌 Introduction

In Kotlin, every variable is declared using one of two keywords:

- `val` — **value** → read-only, immutable reference
- `var` — **variable** → read-write, mutable reference

> Think of `val` as a **permanent marker** and `var` as a **pencil** — one you can't erase, one you can.

---

## 🚀 Why This Is Important

Understanding `val` vs `var` is **foundational** to writing good Kotlin and Android code.

- ✅ It affects **thread safety** — `val` is safer in multi-threaded environments
- ✅ It enables **smart casts** — Kotlin compiler trusts `val` more than `var`
- ✅ It communicates **intent** — `val` tells readers "this won't change"
- ✅ It prevents **bugs** — accidental reassignment is a common source of errors
- ✅ It impacts **performance** — `const val` is inlined at compile time

> 📱 **Android context:** In MVVM architecture, `val` is used for exposed LiveData/StateFlow, and `var` for internal mutable state — this is a pattern you'll see in every production app.

---

## 🧠 Concept Explanation

### What is `val`?

- Declares an **immutable reference** — cannot be reassigned after initialization
- Equivalent to Java's `final` keyword
- Does **not** mean the object itself is immutable — only the reference is fixed

```
val items ──→ [ MutableList ]    ← contents CAN change
   ↑
   └── this arrow cannot point elsewhere
```

### What is `var`?

- Declares a **mutable reference** — can be reassigned any number of times
- The **type is still fixed** — you can change the value, not the type
- Should be used **only when reassignment is genuinely needed**

```
var score ──→ 0   →   10   →   25   →   100
   ↑
   └── this arrow CAN point to new values
```

### Key Differences at a Glance

| Feature | `val` | `var` |
|---|---|---|
| Reassignable | ❌ No | ✅ Yes |
| Type fixed | ✅ Yes | ✅ Yes |
| Works with `lateinit` | ❌ No | ✅ Yes |
| Works with `by lazy{}` | ✅ Yes | ❌ No |
| Smart cast support | ✅ Always | ⚠️ Limited |
| Thread safe | ✅ Safer | ❌ Needs care |
| Custom getter only | ✅ Yes | ✅ Yes |
| Custom getter + setter | ❌ No | ✅ Yes |
| Use `const` | ✅ Yes | ❌ No |
| Default recommendation | ✅ Prefer this | Use when needed |

---

## 💻 Code Examples

### 1️⃣ Basic Declaration

```kotlin
// val — cannot be reassigned
val appName: String = "MyApp"
val maxRetries: Int = 3

// appName = "OtherApp"  ❌ Compile error!

// var — can be reassigned
var loginAttempts: Int = 0
loginAttempts = 1
loginAttempts += 1  // now 2 ✅
```

---

### 2️⃣ Type Inference (No need to write the type!)

```kotlin
val city = "Ahmedabad"    // compiler infers: String
val isLoading = false     // compiler infers: Boolean
val price = 99.99         // compiler infers: Double

var counter = 0           // inferred: Int
counter = 5               // ✅ allowed
// counter = "hello"      // ❌ type mismatch — type is still fixed!
```

---

### 3️⃣ val Reference vs Mutable Object ⚠️

```kotlin
// val means the REFERENCE is fixed, not the object!
val items = mutableListOf("Apple", "Banana")

items.add("Cherry")    // ✅ mutating the list — ALLOWED
items.remove("Apple")  // ✅ still allowed

// items = mutableListOf()  ❌ reassigning val — NOT allowed
```

> ⚠️ **This is the #1 interview trap.** `val` does NOT mean the object is frozen.

---

### 4️⃣ val vs const val

```kotlin
// val — evaluated at RUNTIME
val timestamp = System.currentTimeMillis()
val screenWidth = Resources.getSystem().displayMetrics.widthPixels

// const val — evaluated at COMPILE TIME (inlined by compiler)
const val BASE_URL = "https://api.example.com"
const val TIMEOUT_SECONDS = 30L
const val MAX_LOGIN_ATTEMPTS = 5

// const val ONLY works for: Int, Long, Double, Float, Boolean, String
// const val list = listOf(1,2,3)  ❌ Not allowed!
```

---

### 5️⃣ lateinit var (Android ViewBinding)

```kotlin
class MainActivity : AppCompatActivity() {

    // Can't initialize here — context isn't ready yet
    private lateinit var binding: ActivityMainBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        // Safe to use now ✅
        binding.tvTitle.text = "Welcome!"
    }
}
```

> 💡 `lateinit` is only allowed with `var` — because you're assigning it *after* declaration (which is mutation).

---

### 6️⃣ val with by lazy{} (Efficient Initialization)

```kotlin
class UserRepository(private val context: Context) {

    // Created only when first accessed — not at object creation
    val database: AppDatabase by lazy {
        Room.databaseBuilder(
            context,
            AppDatabase::class.java,
            "user_database"
        ).build()
    }
}

// First access → lambda runs, DB is created
// All subsequent accesses → same DB instance returned instantly
```

> 💡 `by lazy{}` is only for `val` — the value is set once and never changes.

---

### 7️⃣ val + var in a Data Class (Room Entity)

```kotlin
@Entity(tableName = "notes")
data class Note(
    @PrimaryKey(autoGenerate = true)
    val id: Int = 0,        // val — ID never changes once created

    var title: String,      // var — user can edit the title
    var content: String,    // var — user can edit the content

    val createdAt: Long = System.currentTimeMillis()  // val — creation time is permanent
)
```

> 📱 **Real Android pattern:** IDs and timestamps are `val`, user-editable fields are `var`.

---

### 8️⃣ ViewModel — Private var, Public val (MVVM Pattern)

```kotlin
class UserViewModel : ViewModel() {

    // Internal — mutable, private to ViewModel
    private val _users = MutableLiveData<List<User>>()
    private val _isLoading = MutableStateFlow(false)

    // External — immutable reference exposed to UI
    val users: LiveData<List<User>> = _users
    val isLoading: StateFlow<Boolean> = _isLoading.asStateFlow()

    fun loadUsers() {
        _isLoading.value = true
        viewModelScope.launch {
            val result = repository.getUsers()  // val — used once
            _users.postValue(result)
            _isLoading.value = false
        }
    }
}
```

> 📱 **This is standard MVVM Android architecture.** The `val` exposed to UI ensures fragments/activities cannot accidentally modify state.

---

### 9️⃣ val + when Expression (Idiomatic Kotlin)

```kotlin
// ❌ Non-idiomatic — unnecessary var
var statusMessage: String
if (code == 200) {
    statusMessage = "Success"
} else if (code == 404) {
    statusMessage = "Not Found"
} else {
    statusMessage = "Error"
}

// ✅ Idiomatic — clean val with when expression
val statusMessage = when (code) {
    200  -> "Success"
    201  -> "Created"
    400  -> "Bad Request"
    401  -> "Unauthorized"
    404  -> "Not Found"
    500  -> "Server Error"
    else -> "Unknown"
}
```

---

### 🔟 var with Custom Getter & Setter (Validation)

```kotlin
class UserProfile {

    var age: Int = 0
        get() = field                          // field = backing field
        set(value) {
            field = if (value in 1..120) value else 0
        }

    var email: String = ""
        set(value) {
            field = value.trim().lowercase()   // sanitize before storing
        }
}

val profile = UserProfile()
profile.age = -10           // rejected → age stays 0
profile.age = 25            // accepted ✅
profile.email = "  USER@Gmail.COM  "
println(profile.email)      // "user@gmail.com" ✅
```

---

### 1️⃣1️⃣ val in Jetpack Compose

```kotlin
@Composable
fun LoginScreen() {
    // var — mutable state that triggers recomposition
    var email by remember { mutableStateOf("") }
    var password by remember { mutableStateOf("") }

    // val — derived/computed value — use val!
    val isButtonEnabled = email.isNotBlank() && password.length >= 6
    val buttonText = if (isButtonEnabled) "Login" else "Enter credentials"

    Column {
        TextField(value = email, onValueChange = { email = it })
        TextField(value = password, onValueChange = { password = it })
        Button(
            onClick = { /* login */ },
            enabled = isButtonEnabled
        ) {
            Text(buttonText)
        }
    }
}
```

> 📱 **Compose rule:** User input = `var by remember`. Anything derived from it = `val`.

---

### 1️⃣2️⃣ var with Delegates.observable (RecyclerView Adapter)

```kotlin
class ProductAdapter : RecyclerView.Adapter<ProductViewHolder>() {

    var products: List<Product> by Delegates.observable(emptyList()) { _, old, new ->
        // Automatically called when products is reassigned
        DiffUtil.calculateDiff(ProductDiffCallback(old, new))
            .dispatchUpdatesTo(this)
    }
}

// Usage in Fragment:
viewModel.products.observe(viewLifecycleOwner) { list ->
    adapter.products = list  // triggers DiffUtil automatically! ✅
}
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1 — Thinking `val` means the object is immutable

```kotlin
val list = mutableListOf(1, 2, 3)
list.add(100)   // This WORKS — val only freezes the reference!
```

✅ **Fix:** If you want a truly immutable list, use `listOf()` or `persistentListOf()` from kotlinx-collections-immutable.

---

### ❌ Mistake 2 — Using `var` when `val` is possible

```kotlin
// Bad
var greeting = "Hello"
println(greeting)   // greeting is never reassigned — why var?

// Good
val greeting = "Hello"
println(greeting)
```

✅ **Rule:** Default to `val`. Switch to `var` only when you need to reassign.

---

### ❌ Mistake 3 — Using `lateinit` with `val`

```kotlin
lateinit val name: String   // ❌ Compile Error!
lateinit var name: String   // ✅ Correct
```

✅ **Reason:** `lateinit` means "assign later" — which requires mutability (`var`).

---

### ❌ Mistake 4 — Using `const val` with non-primitive types

```kotlin
const val timeout = 30          // ✅ Int
const val name = "Android"      // ✅ String
const val pi = 3.14             // ✅ Double

const val list = listOf(1,2,3)  // ❌ Not a primitive!
const val time = Date()         // ❌ Not compile-time!
```

---

### ❌ Mistake 5 — Reassigning a `var` when `copy()` is better

```kotlin
// Bad — mutating a var field directly
data class User(val name: String, var age: Int)
val user = User("Raj", 25)
user.age = 26   // works but mixes mutability

// Good — use copy() for immutable updates
data class User(val name: String, val age: Int)
val user = User("Raj", 25)
val updatedUser = user.copy(age = 26)  // ✅ immutable update
```

---

## 💡 Pro Tips

- 💡 **Always start with `val`** — switch to `var` only when the compiler or logic forces you to
- 💡 **Use `const val`** inside `companion object` or at the top level for string/int constants — it's faster (inlined at compile time)
- 💡 **Replace `var` + `if/when` reassignment** with `val` + `when` expression — cleaner and idiomatic
- 💡 **`lateinit var`** — always check `::property.isInitialized` before accessing if you're unsure
- 💡 **`by lazy{}`** is perfect for expensive objects (Database, Retrofit, Analytics) — computed once, cached forever
- 💡 **In MVVM**, always expose `val LiveData` / `val StateFlow` from ViewModel — never expose `MutableLiveData` directly
- 💡 **In Compose**, use `var by remember { mutableStateOf() }` for UI state, and `val` for everything derived from it
- 💡 **`@Volatile var`** is needed for singleton patterns accessed across threads (double-checked locking)
- 💡 **Data class with all `val` fields** gets you predictable equality, hashCode, and thread safety for free

---

## 🎯 Interview Questions & Answers

---

### Q1. What is the difference between `val` and `var` in Kotlin?

> **Answer:**
> - `val` declares an **immutable reference** — it cannot be reassigned after initialization (like Java's `final`)
> - `var` declares a **mutable reference** — it can be reassigned any number of times
> - In both cases, the **type is fixed** at declaration — you cannot change the type of either

---

### Q2. Does `val` mean the value can never change?

> **Answer:**
> **No — this is the most common misconception.**
> `val` makes the **reference** immutable, not the object.
> ```kotlin
> val list = mutableListOf(1, 2, 3)
> list.add(4)   // ✅ This works! The object is mutable, only the reference is fixed.
> ```

---

### Q3. Why is `lateinit` only allowed with `var` and not `val`?

> **Answer:**
> `lateinit` means "I will assign this property after declaration." That delayed assignment is itself a **write/reassignment operation**, which requires mutability. `val` cannot be written to after declaration, making `lateinit val` a contradiction.

---

### Q4. What is the difference between `val` with `by lazy{}` and `lateinit var`?

> **Answer:**

| Feature | `val by lazy{}` | `lateinit var` |
|---|---|---|
| Keyword | `val` | `var` |
| Initialized by | Lambda (auto on first access) | Developer (manually) |
| Thread safe | ✅ Yes (by default) | ❌ No |
| Can re-initialize | ❌ No | ✅ Yes |
| Null check needed | ❌ No | ✅ `isInitialized` check |
| Best for | Heavy objects (DB, Retrofit) | Views, DI-injected fields |

---

### Q5. Can you override a `var` with `val` in a subclass?

> **Answer:**
> **No.** You cannot narrow a `var` to `val` in a subclass — it would violate the Liskov Substitution Principle by removing the setter.
> However, you **can** override a `val` with `var` (adding a setter is allowed — widening the contract).
> ```kotlin
> open class Base { open val x = 0 }
> class Child : Base() { override var x = 10 }  // ✅ val → var is OK
>
> open class Base2 { open var y = 0 }
> class Child2 : Base2() { override val y = 10 } // ❌ var → val NOT allowed
> ```

---

### Q6. What is the difference between `val` and `const val`?

> **Answer:**

| | `val` | `const val` |
|---|---|---|
| Evaluated | At runtime | At compile time |
| Allowed types | Any | Primitives + String only |
| Generates getter | ✅ Yes | ❌ No (value is inlined) |
| Can be in a class | ✅ Yes | ⚠️ Only in companion object / top-level |
| Performance | Slightly slower | Faster (zero overhead) |

---

### Q7. Why in MVVM do we use `private val _state = MutableStateFlow(...)` and expose `val state`?

> **Answer:**
> This is the **encapsulation pattern**:
> - Only the **ViewModel** should update state — so the mutable version is `private`
> - The **UI layer** (Fragment/Activity) only needs to observe — so a read-only `val StateFlow` is exposed
> - This enforces **unidirectional data flow** and prevents the UI from accidentally modifying state

---

### Q8. When does Kotlin's smart cast NOT work with `var`?

> **Answer:**
> Smart cast fails for **class-level `var` properties** because the compiler cannot guarantee another thread hasn't changed the value between the type check and usage.
> ```kotlin
> class Example {
>     var name: Any = "hello"
>
>     fun print() {
>         if (name is String) {
>             println(name.length)  // ❌ Smart cast impossible — name is a var member
>         }
>         val temp = name           // ✅ Copy to local val first
>         if (temp is String) println(temp.length)
>     }
> }
> ```

---

### Q9. What is the `field` keyword inside a `var` setter?

> **Answer:**
> `field` is the **backing field** — the actual storage location for the property's value. Inside a getter or setter, you use `field` to read/write the stored value, avoiding infinite recursion (calling `get()` or `set()` inside themselves).
> ```kotlin
> var age: Int = 0
>     set(value) {
>         field = if (value >= 0) value else 0  // field = backing field
>         // age = value  ❌ would call set() recursively → StackOverflow
>     }
> ```

---

### Q10. What happens if `by lazy{}` throws an exception?

> **Answer:**
> In the default **SYNCHRONIZED** mode, if the lazy lambda throws an exception, the value is **not stored**. The next access will **re-run the lambda** — giving it another chance to initialize. This differs from `PUBLICATION` mode. Always handle exceptions inside the lazy block if initialization can fail.

---

## ✅ Summary

```
╔══════════════════════════════════════════════════════════╗
║              val vs var — Quick Reference                ║
╠══════════════════════════════════════════════════════════╣
║  val  →  Immutable reference  →  Cannot be reassigned   ║
║  var  →  Mutable reference    →  Can be reassigned      ║
║                                                          ║
║  Both → Type is ALWAYS fixed after declaration           ║
║  val  ≠ Immutable object (the object can still mutate)   ║
╚══════════════════════════════════════════════════════════╝
```

### Core Rules to Remember

- ✅ **Default to `val`** — use `var` only when you must reassign
- ✅ **`const val`** for primitive/String constants in companion objects
- ✅ **`lateinit var`** for properties initialized after declaration (Views, DI)
- ✅ **`val by lazy{}`** for expensive objects initialized on first use
- ✅ **`val` = immutable reference**, not immutable object — know the difference
- ✅ **In MVVM**, expose `val` LiveData/StateFlow, keep `MutableLiveData` private
- ✅ **In Compose**, `var by remember { mutableStateOf() }` for state, `val` for derived values
- ✅ **Replace `var` + conditional reassignment** with `val` + `when`/`if` expression

### Decision Flowchart

```
Need a variable?
       │
       ▼
Will it ever be reassigned after initialization?
       │
      YES ──────────────────────────────► var
       │
       NO
       │
       ▼
Is it a primitive/String constant?
       │
      YES ──────────────────────────────► const val
       │
       NO
       │
       ▼
Is it expensive to compute and used rarely?
       │
      YES ──────────────────────────────► val by lazy{}
       │
       NO
       │
       ▼
                                          val ✅
```

---

> 📚 **Next Topic:** `lateinit` vs `by lazy` in depth · Nullable types & null safety · Data classes

---

---

# 💯 100 Real `val` & `var` Examples

> Organized from **Beginner → Intermediate → Advanced → Tricky**
> Each example is self-contained with explanation and Android context where applicable.

---

## 🟢 Beginner — Examples 1 to 25

---

### 01 · Basic `val` — String constant

```kotlin
val appName: String = "MyAndroidApp"
// appName = "Other"  ❌ Compile error — val cannot be reassigned
```

> `val` binds the name to a value once. That binding is permanent.

---

### 02 · Basic `var` — mutable counter

```kotlin
var clickCount: Int = 0
clickCount++        // 1
clickCount += 4     // 5
clickCount = 0      // reset ✅
```

> `var` lets you reassign as many times as needed.

---

### 03 · Type inference with `val`

```kotlin
val language = "Kotlin"   // inferred: String
val version  = 1.9        // inferred: Double
val isStable = true       // inferred: Boolean
```

> Kotlin infers the type from the assigned value — you rarely need to write the type explicitly.

---

### 04 · Type inference with `var`

```kotlin
var retryCount = 0        // inferred: Int
retryCount = 3            // ✅ same type
// retryCount = "three"   ❌ type mismatch — type is fixed after inference!
```

> Type is always fixed — even for `var`. You change the *value*, never the *type*.

---

### 05 · `val` for a computed result

```kotlin
val sum = 10 + 20 + 30   // 60
val greeting = "Hello, " + "World!"
```

> Use `val` whenever the result of an expression is used once and not updated.

---

### 06 · `var` for user input tracking

```kotlin
var userInput: String = ""
userInput = "John"
userInput = "Johnny"   // user edited the name
```

> Any state that reflects what a user types or selects should be `var`.

---

### 07 · `val` for Boolean flags (read-only)

```kotlin
val isDebugBuild: Boolean = BuildConfig.DEBUG
val isTablet: Boolean = resources.getBoolean(R.bool.isTablet)
```

> These are set once from system/config and never change — perfect `val`.

---

### 08 · `var` for Boolean toggle state

```kotlin
var isFavorite: Boolean = false
isFavorite = !isFavorite   // toggle on heart button tap
```

> Any UI toggle — favorite, bookmark, expand/collapse — needs `var`.

---

### 09 · `val` for a list (immutable reference)

```kotlin
val colors = listOf("Red", "Green", "Blue")
// colors.add("Yellow")  ❌ listOf is read-only
// colors = listOf()     ❌ val cannot be reassigned
```

> `val` + `listOf` = truly read-only — neither reference nor contents can change.

---

### 10 · `var` for a reassignable list

```kotlin
var searchResults = listOf<String>()
searchResults = listOf("Kotlin", "Android", "Jetpack")  // new search
searchResults = emptyList()   // cleared
```

> When you replace the whole dataset (common in search/filter), use `var`.

---

### 11 · `val` — mutable list, immutable reference

```kotlin
val tags = mutableListOf("android", "kotlin")
tags.add("jetpack")     // ✅ mutating the object is fine
// tags = mutableListOf()  ❌ reassigning val is NOT fine
```

> ⚠️ The most common interview trap — `val` only freezes the reference, not the object.

---

### 12 · `const val` for API base URL

```kotlin
object NetworkConfig {
    const val BASE_URL    = "https://api.myapp.com/v1/"
    const val TIMEOUT     = 30L
    const val MAX_RETRIES = 3
}
```

> `const val` is inlined at compile time — no getter overhead, perfect for string/int constants.

---

### 13 · `val` for context-dependent value

```kotlin
val screenDensity = resources.displayMetrics.density
val deviceLocale  = Locale.getDefault().language
```

> These are runtime values (not compile-time), so `val` — not `const val`.

---

### 14 · `var` with null initial value

```kotlin
var currentUser: User? = null
currentUser = User(id = 1, name = "Riya")   // logged in
currentUser = null                            // logged out
```

> Nullable `var` is the right choice for "optional presence" state.

---

### 15 · `val` in a function scope

```kotlin
fun calculateDiscount(price: Double, percent: Int): Double {
    val multiplier = (100 - percent) / 100.0
    val discounted  = price * multiplier
    return discounted
}
```

> Local intermediate values inside functions should always be `val`.

---

### 16 · `var` as a loop accumulator

```kotlin
var total = 0.0
val prices = listOf(49.99, 19.99, 99.99)
for (price in prices) {
    total += price
}
println("Total: ₹$total")
```

> Classic accumulator pattern — `var` is the right tool here.

---

### 17 · Replacing `var` with functional style

```kotlin
// ❌ Unnecessary var
var sum = 0
for (i in 1..100) sum += i

// ✅ Use val with functional operator
val sum = (1..100).sum()
```

> Kotlin's standard library often lets you replace `var` loops with `val` expressions.

---

### 18 · `val` in string templates

```kotlin
val firstName = "Arjun"
val lastName  = "Mehta"
val welcome   = "Welcome back, $firstName $lastName! 👋"
println(welcome)   // Welcome back, Arjun Mehta! 👋
```

> Composed string values should be `val` — they're built once and used as-is.

---

### 19 · `var` for retry logic

```kotlin
var attempts = 0
val maxAttempts = 3

while (attempts < maxAttempts) {
    try {
        callApi()
        break
    } catch (e: Exception) {
        attempts++
    }
}
```

> `attempts` changes every iteration — `var`. `maxAttempts` never changes — `val`.

---

### 20 · `val` in `companion object`

```kotlin
class LoginFragment : Fragment() {
    companion object {
        const val TAG            = "LoginFragment"
        const val ARG_REDIRECT   = "redirect_url"
        const val MAX_PIN_LENGTH = 6
    }
}
```

> Always use `const val` in companion objects for primitive/string constants.

---

### 21 · `val` for data class — immutable model

```kotlin
data class Product(
    val id: String,
    val name: String,
    val price: Double,
    val imageUrl: String
)
val product = Product("p1", "Laptop", 55999.0, "https://...")
// product.price = 49999.0  ❌ val property
```

> API response models should be all-`val` — they are immutable snapshots.

---

### 22 · `var` in data class — editable model

```kotlin
data class CartItem(
    val productId: String,
    var quantity: Int,           // user can change qty
    var isSelected: Boolean = true
)
val item = CartItem("p1", 1)
item.quantity = 3    // ✅ var property on val reference
```

> The *reference* `item` is `val`, but `var` properties inside can be mutated.

---

### 23 · `val` with `if` expression

```kotlin
val greeting = if (hour < 12) "Good Morning 🌅" else "Good Evening 🌙"
```

> `if` is an expression in Kotlin — assign directly to `val` instead of using `var` + branches.

---

### 24 · `val` with `when` expression

```kotlin
val networkType = when (connectivityManager.activeNetwork?.type) {
    ConnectivityManager.TYPE_WIFI   -> "WiFi"
    ConnectivityManager.TYPE_MOBILE -> "Mobile Data"
    else                            -> "No Connection"
}
```

> `when` as expression + `val` = clean, readable, and safe.

---

### 25 · `var` in a `do-while` loop

```kotlin
var pageNumber = 1
do {
    val page = api.fetchPage(pageNumber)
    processPage(page)
    pageNumber++
} while (pageNumber <= page.totalPages)
```

> Page cursor state needs `var` — it's updated on every iteration.

---

## 🟡 Intermediate — Examples 26 to 60

---

### 26 · `lateinit var` — ViewBinding in Activity

```kotlin
class ProfileActivity : AppCompatActivity() {
    private lateinit var binding: ActivityProfileBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityProfileBinding.inflate(layoutInflater)
        setContentView(binding.root)
        binding.tvName.text = "Aarav Shah"
    }
}
```

> `lateinit var` is the standard for ViewBinding — initialized in `onCreate`, not at declaration.

---

### 27 · `lateinit var` — isInitialized safety check

```kotlin
private lateinit var analyticsTracker: AnalyticsTracker

fun trackEvent(name: String) {
    if (::analyticsTracker.isInitialized) {
        analyticsTracker.log(name)
    } else {
        Log.w(TAG, "Tracker not ready")
    }
}
```

> Always use `::property.isInitialized` before accessing `lateinit var` if you're unsure.

---

### 28 · `val by lazy{}` — Retrofit instance

```kotlin
val apiService: ApiService by lazy {
    Retrofit.Builder()
        .baseUrl(BASE_URL)
        .addConverterFactory(GsonConverterFactory.create())
        .build()
        .create(ApiService::class.java)
}
// Created only on first network call — not at app start
```

> `by lazy` is ideal for Retrofit, Room, OkHttp — heavy objects you don't always need immediately.

---

### 29 · `val by lazy{}` — Room Database

```kotlin
val database: AppDatabase by lazy {
    Room.databaseBuilder(
        applicationContext,
        AppDatabase::class.java,
        "app_database.db"
    )
    .fallbackToDestructiveMigration()
    .build()
}
```

> Database creation is expensive — defer it with `by lazy` until first query.

---

### 30 · `val by lazy{}` — SharedPreferences

```kotlin
val prefs: SharedPreferences by lazy {
    getSharedPreferences("user_prefs", Context.MODE_PRIVATE)
}
```

> Preferences are accessed from multiple places — lazy ensures a single instance.

---

### 31 · Private `var` with public `val` getter

```kotlin
class SessionManager {
    private var _authToken: String? = null
    val authToken: String? get() = _authToken

    fun login(token: String) { _authToken = token }
    fun logout() { _authToken = null }
}
```

> External code can only read `authToken` — only `SessionManager` can write to `_authToken`.

---

### 32 · ViewModel — `MutableLiveData` vs `LiveData`

```kotlin
class ProductViewModel : ViewModel() {
    private val _products = MutableLiveData<List<Product>>()
    val products: LiveData<List<Product>> = _products

    fun loadProducts() {
        viewModelScope.launch {
            _products.postValue(repository.getAll())
        }
    }
}
```

> Standard MVVM pattern — private mutable state, public read-only exposure.

---

### 33 · ViewModel — `MutableStateFlow` vs `StateFlow`

```kotlin
class HomeViewModel : ViewModel() {
    private val _uiState = MutableStateFlow<UiState>(UiState.Loading)
    val uiState: StateFlow<UiState> = _uiState.asStateFlow()

    fun refresh() {
        _uiState.value = UiState.Loading
        viewModelScope.launch {
            _uiState.value = UiState.Success(repository.getData())
        }
    }
}
```

> `asStateFlow()` returns a read-only view — UI cannot accidentally push state.

---

### 34 · `var` with `private set` — controlled mutation

```kotlin
class GameEngine {
    var score: Int = 0
        private set     // external code can READ but not WRITE

    fun addPoints(pts: Int) {
        score += pts    // only GameEngine can modify
    }
}
```

> `private set` is the sweet spot — external read access, internal write control.

---

### 35 · `val` in a `sealed class`

```kotlin
sealed class ApiResult<out T> {
    data class Success<T>(val data: T) : ApiResult<T>()
    data class Error(val code: Int, val message: String) : ApiResult<Nothing>()
    object Loading : ApiResult<Nothing>()
}
```

> All payload fields are `val` — state objects should be immutable snapshots.

---

### 36 · `var` in a `sealed class` (editable state)

```kotlin
sealed class FormState {
    data class Editing(
        var name: String = "",
        var email: String = "",
        var isValid: Boolean = false
    ) : FormState()

    object Submitted : FormState()
}
```

> `var` inside sealed class makes sense when fields are actively updated before submission.

---

### 37 · `val` for coroutine Job reference

```kotlin
class SyncViewModel : ViewModel() {
    private var syncJob: Job? = null

    fun startSync() {
        syncJob?.cancel()
        syncJob = viewModelScope.launch {
            val result = repository.sync()   // val — used once
            _syncState.value = result
        }
    }
}
```

> `result` inside the coroutine is `val` — fetched once, used once.

---

### 38 · `var` for debounced search

```kotlin
private var searchJob: Job? = null

fun onSearchTextChanged(query: String) {
    searchJob?.cancel()          // cancel previous
    searchJob = viewModelScope.launch {
        delay(300)               // debounce
        performSearch(query)
    }
}
```

> `searchJob` is replaced on each keystroke — needs `var`.

---

### 39 · `val` in a `data class` with `copy()`

```kotlin
data class UserSettings(
    val isDarkMode: Boolean = false,
    val fontSize: Int = 14,
    val language: String = "en"
)

val defaults = UserSettings()
val darkModeEnabled = defaults.copy(isDarkMode = true)
// defaults is unchanged ✅
```

> All-`val` data class + `copy()` = clean immutable updates. Preferred in Kotlin.

---

### 40 · `val` in interface

```kotlin
interface Identifiable {
    val id: String   // abstract val — must be provided by implementors
    val createdAt: Long
}

data class Message(
    override val id: String,
    override val createdAt: Long,
    val text: String
) : Identifiable
```

> Interface `val` properties define read-only contracts for implementors.

---

### 41 · `var` in interface (mutable contract)

```kotlin
interface Stateful {
    var isActive: Boolean
    var lastUpdated: Long
}

class LiveSession : Stateful {
    override var isActive: Boolean = true
    override var lastUpdated: Long = System.currentTimeMillis()
}
```

> Interface `var` requires implementing class to provide both getter and setter.

---

### 42 · `val` for Hilt-injected dependency

```kotlin
@HiltViewModel
class OrderViewModel @Inject constructor(
    private val orderRepository: OrderRepository,
    private val userRepository: UserRepository,
    private val analyticsService: AnalyticsService
) : ViewModel()
```

> Constructor-injected dependencies are always `val` — set once, never replaced.

---

### 43 · `val` in Room Entity

```kotlin
@Entity(tableName = "transactions")
data class Transaction(
    @PrimaryKey val txId: String,
    val amount: Double,
    val currency: String,
    val timestamp: Long = System.currentTimeMillis()
)
```

> Financial records must be immutable — all fields `val`.

---

### 44 · `var` in Room Entity (editable fields)

```kotlin
@Entity(tableName = "tasks")
data class Task(
    @PrimaryKey(autoGenerate = true) val id: Int = 0,
    val createdAt: Long = System.currentTimeMillis(),
    var title: String,
    var isCompleted: Boolean = false,
    var dueDate: Long? = null
)
```

> `id` and `createdAt` are permanent (`val`). Task details can be edited (`var`).

---

### 45 · `val` with `let` scope function

```kotlin
val currentUser: User? = sessionManager.getUser()

currentUser?.let { user ->
    val displayName = "${user.firstName} ${user.lastName}"
    binding.tvName.text = displayName
}
```

> `displayName` inside `let` is `val` — computed once per `let` invocation.

---

### 46 · `val` with `apply` scope function

```kotlin
val intent = Intent(this, DetailActivity::class.java).apply {
    putExtra("product_id", product.id)
    putExtra("product_name", product.name)
    flags = Intent.FLAG_ACTIVITY_SINGLE_TOP
}
startActivity(intent)
```

> The `Intent` reference is `val` — you mutate its properties via `apply`, but never replace the object.

---

### 47 · `var` with `Delegates.observable`

```kotlin
class UserAdapter : RecyclerView.Adapter<UserViewHolder>() {
    var users: List<User> by Delegates.observable(emptyList()) { _, old, new ->
        val diff = DiffUtil.calculateDiff(UserDiffCallback(old, new))
        diff.dispatchUpdatesTo(this)
    }
}
// adapter.users = newList → DiffUtil runs automatically ✅
```

> `Delegates.observable` fires a callback every time `var` is reassigned.

---

### 48 · `var` with `Delegates.vetoable` (validation)

```kotlin
var rating: Int by Delegates.vetoable(0) { _, _, newValue ->
    newValue in 1..5   // only accept 1-5, reject otherwise
}

rating = 3   // ✅ accepted
rating = 7   // ❌ rejected — rating stays 3
rating = -1  // ❌ rejected
```

> `Delegates.vetoable` blocks the assignment if the lambda returns false.

---

### 49 · `val` for NavController

```kotlin
class HomeFragment : Fragment() {
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        val navController = findNavController()
        binding.btnGoToProfile.setOnClickListener {
            navController.navigate(R.id.action_home_to_profile)
        }
    }
}
```

> `navController` reference never changes — `val` is correct.

---

### 50 · `val` in RecyclerView ViewHolder

```kotlin
class ProductViewHolder(
    private val binding: ItemProductBinding
) : RecyclerView.ViewHolder(binding.root) {

    fun bind(product: Product) {
        val formattedPrice = "₹${product.price}"
        binding.tvName.text  = product.name
        binding.tvPrice.text = formattedPrice
    }
}
```

> `formattedPrice` is computed once per `bind()` call — always `val`.

---

### 51 · `val` in WorkManager Worker

```kotlin
class ImageUploadWorker(
    context: Context,
    params: WorkerParameters
) : CoroutineWorker(context, params) {

    override suspend fun doWork(): Result {
        val imageUri = inputData.getString("image_uri") ?: return Result.failure()
        val uploadResult = repository.uploadImage(imageUri)
        return if (uploadResult.isSuccess) Result.success() else Result.retry()
    }
}
```

> `imageUri` and `uploadResult` are used once — both `val`.

---

### 52 · `var` in WorkManager for retry tracking

```kotlin
override suspend fun doWork(): Result {
    var retries = inputData.getInt("retry_count", 0)
    return try {
        api.upload(data)
        Result.success()
    } catch (e: Exception) {
        retries++
        if (retries < 3) Result.retry() else Result.failure()
    }
}
```

> `retries` is incremented — needs `var`.

---

### 53 · `val` for Kotlin Serialization model

```kotlin
@Serializable
data class WeatherResponse(
    @SerialName("temp_c")     val tempCelsius: Double,
    @SerialName("humidity")   val humidity: Int,
    @SerialName("condition")  val condition: WeatherCondition,
    @SerialName("updated_at") val updatedAt: String
)
```

> JSON deserialized models are immutable snapshots — always all-`val`.

---

### 54 · `val` in Paging 3

```kotlin
class UserViewModel(repository: UserRepository) : ViewModel() {
    val pagedUsers: Flow<PagingData<User>> = Pager(
        config = PagingConfig(pageSize = 20, enablePlaceholders = false)
    ) {
        UserPagingSource(repository)
    }.flow.cachedIn(viewModelScope)
}
```

> The `Pager` and `Flow` reference are `val` — created once and cached.

---

### 55 · `val` for Compose `derivedStateOf`

```kotlin
@Composable
fun SearchScreen(allItems: List<String>) {
    var query by remember { mutableStateOf("") }

    val filteredItems by remember(query) {
        derivedStateOf { allItems.filter { it.contains(query, ignoreCase = true) } }
    }

    LazyColumn {
        items(filteredItems) { Text(it) }
    }
}
```

> `filteredItems` is derived from `query` — `val` via `derivedStateOf` avoids unnecessary recompositions.

---

### 56 · `val` for `@Parcelize` model

```kotlin
@Parcelize
data class OrderSummary(
    val orderId: String,
    val items: List<String>,
    val totalAmount: Double,
    val deliveryDate: String
) : Parcelable
```

> `@Parcelize` with all-`val` data classes generates safe, predictable Parcelable code.

---

### 57 · `var` for animation state tracking

```kotlin
class AnimatedHeaderView @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null
) : View(context, attrs) {

    var expansionProgress: Float = 0f
        set(value) {
            field = value.coerceIn(0f, 1f)
            invalidate()   // triggers redraw
        }
}
```

> Custom setter coerces value and triggers `invalidate()` — clean reactive pattern.

---

### 58 · `val` in `object` singleton

```kotlin
object AppConfig {
    const val VERSION_NAME  = "2.4.1"
    const val VERSION_CODE  = 241
    val buildTime: Long     = System.currentTimeMillis()
    val supportedLocales    = listOf("en", "hi", "gu", "mr")
}
```

> `object` singletons use `const val` for compile-time and `val` for runtime constants.

---

### 59 · `val` in extension function

```kotlin
fun String.toSlug(): String {
    val lower   = this.lowercase()
    val trimmed = lower.trim()
    val slug    = trimmed.replace(Regex("[^a-z0-9]+"), "-")
    return slug
}
// "Hello World!".toSlug()  →  "hello-world"
```

> Each transformation step is a new `val` — readable pipeline of immutable values.

---

### 60 · `var` in a state machine

```kotlin
enum class PlayerState { IDLE, BUFFERING, PLAYING, PAUSED, ERROR }

class MediaPlayerController {
    var state: PlayerState = PlayerState.IDLE
        private set

    fun play()  { state = PlayerState.PLAYING  }
    fun pause() { state = PlayerState.PAUSED   }
    fun stop()  { state = PlayerState.IDLE     }
}
```

> `state` changes based on player lifecycle events — `var` with `private set`.

---

## 🔴 Advanced — Examples 61 to 80

---

### 61 · `@Volatile var` — thread-safe singleton

```kotlin
object DatabaseProvider {
    @Volatile
    private var instance: AppDatabase? = null

    fun getInstance(context: Context): AppDatabase {
        return instance ?: synchronized(this) {
            instance ?: Room.databaseBuilder(
                context.applicationContext,
                AppDatabase::class.java,
                "app.db"
            ).build().also { instance = it }
        }
    }
}
```

> `@Volatile` ensures all threads see the latest write immediately — essential for double-checked locking.

---

### 62 · `val` in `value class` (inline class)

```kotlin
@JvmInline
value class UserId(val raw: String) {
    init { require(raw.isNotBlank()) { "UserId cannot be blank" } }
}

@JvmInline
value class EmailAddress(val raw: String) {
    init { require(raw.contains("@")) { "Invalid email" } }
}

val id    = UserId("user_abc_123")
val email = EmailAddress("dev@example.com")
// Compiler enforces you can't pass email where userId is expected ✅
```

> `val` inside `value class` = zero-cost type-safe wrapper — great for domain-driven design.

---

### 63 · `val` — override `val` with `var` in subclass

```kotlin
open class BaseRepository {
    open val cacheEnabled: Boolean = false
}

class UserRepository : BaseRepository() {
    override var cacheEnabled: Boolean = true   // widened to var ✅
}
// You CAN widen val → var in subclass
// You CANNOT narrow var → val in subclass
```

> Widening a `val` to `var` adds a setter — safe. Narrowing `var` to `val` removes a setter — breaks callers.

---

### 64 · `val` with property delegation — Map backing

```kotlin
class ServerConfig(private val config: Map<String, Any>) {
    val host: String by config
    val port: Int    by config
    val timeout: Int by config
}

val cfg = ServerConfig(mapOf("host" to "localhost", "port" to 8080, "timeout" to 30))
println(cfg.host)   // localhost
```

> Properties delegated to a `Map` — key name matches property name. Useful for config/JSON parsing.

---

### 65 · `var` with property delegation — MutableMap

```kotlin
class MutableConfig(private val map: MutableMap<String, Any> = mutableMapOf()) {
    var baseUrl: String by map
    var debugMode: Boolean by map
}

val cfg = MutableConfig()
cfg.baseUrl   = "https://api.dev.example.com"
cfg.debugMode = true
println(cfg.map)   // {baseUrl=https://..., debugMode=true}
```

> Writing to the delegated `var` property writes directly to the backing map.

---

### 66 · `val` in coroutine `async/await`

```kotlin
suspend fun loadDashboard(): DashboardData {
    return coroutineScope {
        val userDeferred    = async { userRepository.getProfile() }
        val statsDeferred   = async { statsRepository.getSummary() }
        val notifDeferred   = async { notifRepository.getUnread() }

        val user    = userDeferred.await()
        val stats   = statsDeferred.await()
        val notifs  = notifDeferred.await()

        DashboardData(user, stats, notifs)
    }
}
```

> All three deferred results and final values are `val` — they're set once from `await()`.

---

### 67 · `val` for reified generic helper

```kotlin
inline fun <reified T : Any> Bundle.getParcelableCompat(key: String): T? {
    val clazz = T::class.java
    return if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.TIRAMISU) {
        getParcelable(key, clazz)
    } else {
        @Suppress("DEPRECATION")
        getParcelable(key) as? T
    }
}
```

> `clazz` is a `val` derived from the reified type — immutable class reference.

---

### 68 · `var` in a DSL builder

```kotlin
class NotificationBuilder {
    var title: String   = ""
    var message: String = ""
    var channelId: String = "default"
    var isAutoCancel: Boolean = true
}

fun buildNotification(block: NotificationBuilder.() -> Unit): NotificationCompat.Builder {
    val config = NotificationBuilder().apply(block)
    return NotificationCompat.Builder(context, config.channelId)
        .setContentTitle(config.title)
        .setContentText(config.message)
        .setAutoCancel(config.isAutoCancel)
}

// Usage:
val notification = buildNotification {
    title   = "New Message"
    message = "You have 3 unread messages"
}
```

> Builder DSL uses `var` so callers can set fields inside the `apply` lambda.

---

### 69 · `val` in functional interface (SAM)

```kotlin
fun interface InputValidator {
    fun validate(input: String): Boolean
}

val nonEmpty   = InputValidator { it.isNotBlank() }
val minLength  = InputValidator { it.length >= 8 }
val hasDigit   = InputValidator { it.any { c -> c.isDigit() } }

fun validatePassword(pwd: String): Boolean =
    listOf(nonEmpty, minLength, hasDigit).all { it.validate(pwd) }
```

> SAM conversions stored as `val` = immutable strategy objects — composable and reusable.

---

### 70 · `var` in Flow `scan` operator

```kotlin
val runningTotal: Flow<Int> = itemPricesFlow
    .scan(0) { accumulator, price ->
        accumulator + price   // accumulator acts as a var internally
    }
```

> `scan` emits running totals — the accumulated value mutates internally per emission.

---

### 71 · `val` for `CompositionLocal`

```kotlin
val LocalAppTheme = staticCompositionLocalOf { AppTheme.Light }
val LocalUserPrefs = compositionLocalOf { UserPreferences() }

@Composable
fun App() {
    val theme = LocalAppTheme.current
    val prefs = LocalUserPrefs.current
    // theme and prefs are val — the local itself is stable
}
```

> CompositionLocals are `val` — the provider changes the *value*, not the reference.

---

### 72 · `val` in Channel communication

```kotlin
class EventBus {
    private val _events = Channel<AppEvent>(Channel.BUFFERED)
    val events: Flow<AppEvent> = _events.receiveAsFlow()

    suspend fun emit(event: AppEvent) {
        _events.send(event)
    }
}
```

> Channel reference is `val` — you send/receive through it, never replace it.

---

### 73 · `val` for `LaunchedEffect` key

```kotlin
@Composable
fun UserProfileScreen(userId: String) {
    val viewModel: UserViewModel = hiltViewModel()

    LaunchedEffect(userId) {   // userId is stable — treated as val by Compose
        viewModel.loadUser(userId)
    }
}
```

> Keys in `LaunchedEffect` should be stable values — `val` or immutable parameters.

---

### 74 · `var` in Compose — `rememberSaveable`

```kotlin
@Composable
fun CounterScreen() {
    var count by rememberSaveable { mutableStateOf(0) }
    // count survives configuration changes (rotation, etc.)

    Button(onClick = { count++ }) {
        Text("Tapped $count times")
    }
}
```

> `rememberSaveable` persists `var` state across recompositions and config changes.

---

### 75 · `val` in `produce` coroutine builder

```kotlin
@OptIn(ExperimentalCoroutinesApi::class)
fun CoroutineScope.locationUpdates(
    provider: LocationProvider
): ReceiveChannel<Location> = produce {
    val listener = LocationListener { location ->
        trySend(location)
    }
    provider.addListener(listener)
    awaitClose { provider.removeListener(listener) }
}
```

> `listener` inside `produce` is `val` — created once, removed on close.

---

### 76 · `val` — `mapNotNull` reducing to `val`

```kotlin
fun parseUserIds(rawIds: List<String?>): List<Int> {
    val validIds = rawIds
        .mapNotNull { it?.trim() }
        .filter { it.isNotEmpty() }
        .mapNotNull { it.toIntOrNull() }
    return validIds
}
```

> Each step returns a new immutable list — chained transformations, all `val`.

---

### 77 · `val` in `withContext`

```kotlin
suspend fun fetchAndSaveUser(id: String) {
    val rawData = withContext(Dispatchers.IO) {
        api.getUser(id)   // network call on IO dispatcher
    }
    val user = rawData.toDomainModel()
    withContext(Dispatchers.IO) {
        dao.insert(user)
    }
}
```

> Results from `withContext` are used-once — both `val`.

---

### 78 · `var` in `buildString` DSL

```kotlin
val report = buildString {
    var sectionIndex = 1
    sections.forEach { section ->
        appendLine("## $sectionIndex. ${section.title}")
        appendLine(section.content)
        appendLine()
        sectionIndex++
    }
}
```

> `sectionIndex` is a counter inside `buildString` — needs `var`. The outer `report` is `val`.

---

### 79 · `val` in `measureTimeMillis`

```kotlin
val elapsed = measureTimeMillis {
    val result = heavyComputation()
    processResult(result)
}
Log.d(TAG, "Completed in ${elapsed}ms")
```

> `elapsed` and `result` are both `val` — set once, used once.

---

### 80 · `val` in `runCatching`

```kotlin
val result = runCatching {
    val json = fileReader.readText()
    val parsed = Gson().fromJson(json, Config::class.java)
    parsed
}

result.onSuccess { config -> applyConfig(config) }
result.onFailure { error  -> showError(error.message) }
```

> `result`, `json`, and `parsed` are all `val` — a clean, exception-safe pipeline.

---

## ⚡ Tricky — Examples 81 to 100

---

### 81 · `val` custom getter — NOT a constant!

```kotlin
val timestamp: Long
    get() = System.currentTimeMillis()

println(timestamp)   // 1704067200000
Thread.sleep(100)
println(timestamp)   // 1704067200100 — different value!
```

> ⚠️ A `val` with a custom getter and no backing `field` re-evaluates on EVERY access. It is **not** a constant.

---

### 82 · Smart cast fails for class-level `var`

```kotlin
class Processor {
    var input: Any = "hello"

    fun process() {
        if (input is String) {
            // println(input.length)  ❌ Smart cast impossible
            // Another thread could change input between check and use!

            val safe = input   // ✅ copy to local val first
            if (safe is String) println(safe.length)
        }
    }
}
```

> Smart cast works for `val` always, and for local `var`. Fails for class-member `var` due to thread-safety concerns.

---

### 83 · `val` inside a loop — valid!

```kotlin
for (index in 0 until items.size) {
    val item = items[index]       // new val binding each iteration ✅
    val formatted = formatItem(item)
    display(formatted)
}
```

> `val` inside a loop is perfectly legal — each iteration creates a **new scope** with a new binding.

---

### 84 · Loop variable is implicitly `val`

```kotlin
for (item in listOf(1, 2, 3)) {
    // item = 10   ❌ Compile error! Loop variable is implicitly val
    println(item * 2)   // ✅ reading is fine
}
```

> The loop iteration variable is always implicitly `val` — create a separate `var` if you need mutation.

---

### 85 · `val` overrides `var` — ILLEGAL direction

```kotlin
open class Widget {
    open var isVisible: Boolean = true
}

class StaticWidget : Widget() {
    // override val isVisible = true   ❌ Error! Cannot narrow var → val
    override var isVisible: Boolean = true   // ✅ must stay var
}
```

> Narrowing `var` → `val` would remove the setter from a subtype — violates Liskov Substitution Principle.

---

### 86 · `val` in `init` block — ordering matters!

```kotlin
class Config {
    val doubled = base * 2   // base not initialized yet! doubled = 0 * 2 = 0
    val base = 10
}
val c = Config()
println(c.doubled)  // 0, not 20! ⚠️
```

> Properties are initialized top-to-bottom. `doubled` sees `base = 0` (default Int). Always declare dependencies before use.

---

### 87 · Open `val` in `init` — dangerous!

```kotlin
open class Animal {
    open val sound: String = "..."
    init { println("Sound: $sound") }   // ⚠️ reads overridden property
}

class Dog : Animal() {
    override val sound: String = "Woof"
}

val dog = Dog()
// Prints: "Sound: ..." — NOT "Sound: Woof"!
// Dog's override is not initialized when Animal's init runs
```

> ⚠️ Never read `open` properties inside `init` blocks — the override isn't active yet.

---

### 88 · `val` + `lazy` — exception retries

```kotlin
var initCount = 0

val riskyValue: String by lazy {
    initCount++
    if (initCount < 3) throw RuntimeException("Not ready")
    "Finally initialized"
}

repeat(4) {
    try { println(riskyValue) }
    catch (e: Exception) { println("Failed: ${e.message}") }
}
// Failed: Not ready
// Failed: Not ready
// Finally initialized   ← 3rd attempt succeeds
// Finally initialized   ← cached from here on
```

> In SYNCHRONIZED mode, `lazy` retries on exception. Once it succeeds, value is cached forever.

---

### 89 · `val` shadowing outer `val`

```kotlin
val message = "outer"

run {
    val message = "inner"   // shadows outer val — both are valid ✅
    println(message)         // "inner"
}

println(message)   // "outer" — unchanged
```

> Inner scope `val` can shadow outer `val`. The outer binding is untouched.

---

### 90 · `data class` equality with `var` fields

```kotlin
data class Session(val id: String, var token: String)

val s1 = Session("abc", "token1")
val s2 = Session("abc", "token1")
println(s1 == s2)   // true

s1.token = "token2"
println(s1 == s2)   // false ⚠️ — var field changed equality!
```

> `data class` equals includes ALL constructor parameters (val and var). Mutating `var` fields changes `equals()` and `hashCode()`. Prefer all-`val` for stable equality.

---

### 91 · `const val` vs `val` in bytecode

```kotlin
const val COMPILE_CONST = 42
val RUNTIME_VAL = 42

// In Java bytecode:
// COMPILE_CONST → literal 42 inlined everywhere (no method call)
// RUNTIME_VAL   → FileKt.getRUNTIME_VAL() method call at each usage
```

> `const val` is faster in hot paths — no getter method overhead. Use it for all primitive/String constants.

---

### 92 · `var` in companion — shared mutable state (use with care!)

```kotlin
class ApiClient {
    companion object {
        var instanceCount = 0   // shared across ALL instances!
    }

    init { instanceCount++ }
}

val a = ApiClient()
val b = ApiClient()
println(ApiClient.instanceCount)   // 2 — both instances share this var
```

> `var` in `companion object` is like Java's `static` field — shared mutable state. Use with caution in multithreaded code.

---

### 93 · `var` backed by `SharedPreferences`

```kotlin
class PrefsManager(private val prefs: SharedPreferences) {

    var isDarkMode: Boolean
        get()         = prefs.getBoolean("dark_mode", false)
        set(value)    = prefs.edit().putBoolean("dark_mode", value).apply()

    var userName: String
        get()         = prefs.getString("user_name", "") ?: ""
        set(value)    = prefs.edit().putString("user_name", value).apply()
}

prefs.isDarkMode = true   // writes to SharedPreferences ✅
println(prefs.isDarkMode) // reads from SharedPreferences ✅
```

> Property delegation to SharedPreferences via custom getter/setter — elegant encapsulation.

---

### 94 · `val` for immutable nested state update

```kotlin
data class Address(val city: String, val pin: String)
data class User(val name: String, val address: Address)

val user = User("Kavya", Address("Surat", "395001"))

// Update nested field — create new instances:
val updatedUser = user.copy(
    address = user.address.copy(pin = "395007")
)
// user is UNCHANGED ✅
```

> All-`val` data classes with `copy()` = safe, predictable, deeply immutable updates.

---

### 95 · `val` — delegated property stores in external object

```kotlin
class UserPrefs(private val map: MutableMap<String, Any> = mutableMapOf()) {
    var theme: String by map
    var fontSize: Int  by map
}

val prefs = UserPrefs()
prefs.theme = "dark"   // stores into map!

val map = prefs.map
println(map["theme"])   // "dark" ⚠️ — external object was mutated via var delegation
```

> Delegated `var` stores its value in the delegate object. Changes are visible externally.

---

### 96 · `val` — multiple `val` vs one `var` (prefer `val`)

```kotlin
// ❌ one var that gets reassigned
var text = ""
text = if (user.isPremium) "Premium Member 🌟" else "Free User"
text = text.uppercase()

// ✅ multiple vals — each step is clear and immutable
val memberLabel = if (user.isPremium) "Premium Member 🌟" else "Free User"
val displayText = memberLabel.uppercase()
```

> Chaining `val` declarations is cleaner and more debuggable than reusing one `var`.

---

### 97 · `val` in `sequence` builder

```kotlin
val fibonacci: Sequence<Long> = sequence {
    var a = 0L
    var b = 1L
    while (true) {
        yield(a)
        val next = a + b   // val — computed once per step
        a = b
        b = next
    }
}

println(fibonacci.take(10).toList())
// [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

> `next` inside the sequence is `val` — computed per iteration. `a` and `b` are `var` — they update the sequence state.

---

### 98 · `val` for cached network response

```kotlin
class WeatherRepository(
    private val api: WeatherApi,
    private val cache: Cache
) {
    suspend fun getWeather(city: String): Weather {
        val cached = cache.get(city)
        if (cached != null) return cached

        val fresh = api.fetchWeather(city)
        cache.put(city, fresh)
        return fresh
    }
}
```

> `cached` and `fresh` are both `val` — fetched or received once, never mutated.

---

### 99 · `var` in a drag-and-drop touch handler

```kotlin
class DragDropView(context: Context) : View(context) {
    private var dragStartX = 0f
    private var dragStartY = 0f
    private var isDragging = false

    override fun onTouchEvent(event: MotionEvent): Boolean {
        when (event.action) {
            MotionEvent.ACTION_DOWN -> {
                dragStartX = event.x
                dragStartY = event.y
                isDragging = true
            }
            MotionEvent.ACTION_UP -> isDragging = false
        }
        return true
    }
}
```

> Touch coordinates and drag state change on every touch event — all `var`.

---

### 100 · `val` — the ultimate best practice summary

```kotlin
// ✅ All of these are correct idiomatic Kotlin:

const val API_VERSION   = "v2"                    // compile-time constant
val baseUrl             = "https://api.app.com"   // runtime constant
val database by lazy    { buildDatabase() }       // lazy expensive object
lateinit var binding    : ActivityMainBinding     // set in onCreate
var counter             = 0                       // genuinely needs mutation
var state by remember   { mutableStateOf(false) } // Compose UI state

// The golden rule:
// → Start with val
// → The compiler will tell you if you need var
```

> `val` is not just a keyword — it's a **communication tool**. It tells every developer
> reading your code: *"This value is stable. You can trust it."*

---

## 📊 Complete 100 Examples — Category Overview

| Category | Range | Focus |
|---|---|---|
| 🟢 Beginner | 01–25 | Basic syntax, type inference, loops, data classes, companion object |
| 🟡 Intermediate | 26–60 | lateinit, lazy, ViewModel, LiveData, StateFlow, Room, Hilt, Delegates |
| 🔴 Advanced | 61–80 | @Volatile, value class, coroutines, DSL, SAM, Paging, Compose advanced |
| ⚡ Tricky | 81–100 | Gotchas, smart cast, init ordering, equality, bytecode, shadowing |

---

*Generated for Android Developer Interview Preparation · Kotlin Beginner → Intermediate → Advanced*
