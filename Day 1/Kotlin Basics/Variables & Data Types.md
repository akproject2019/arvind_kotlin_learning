Since you're preparing for **6+ years Android interviews**, here's the **Interview Handbook** version of **Kotlin Basics – Variables & Data Types**. It is designed from **beginner → senior**, with definitions, syntax, interview questions, best practices, common mistakes, and Android examples.

---

# Kotlin Basics – Variables & Data Types

## Learning Objectives

After completing this chapter, you should be able to:

* Understand Kotlin variables
* Know the difference between `val` and `var`
* Use Kotlin primitive and reference data types
* Understand type inference
* Know nullable vs non-nullable types
* Write production-ready Kotlin code
* Answer interview questions confidently

---

# 1. What is a Variable?

## Definition

A **variable** is a named memory location used to store data that can be accessed and manipulated throughout the program.

Example:

```kotlin
val name = "Arvind"
```

Here:

* Variable Name → `name`
* Value → `"Arvind"`
* Type → `String`

---

# 2. Variable Declaration

Kotlin has two keywords:

```kotlin
val
```

and

```kotlin
var
```

---

# 3. val (Immutable Variable)

## Definition

`val` means **read-only**.

Once assigned, it **cannot be reassigned**.

```kotlin
val company = "Google"
```

❌ Invalid

```kotlin
company = "Microsoft"
```

Compiler Error

```
Val cannot be reassigned
```

### Use Cases

* Constants
* Configuration values
* Repository objects
* ViewBinding references
* Dependency Injection objects
* Objects that should never change

Android Example

```kotlin
private val repository = UserRepository()
```

---

# 4. var (Mutable Variable)

## Definition

`var` allows changing its value.

```kotlin
var count = 10

count = 20
```

Valid

Android Example

```kotlin
var selectedPosition = -1
```

---

# 5. val vs var

| Feature               | val       | var              |
| --------------------- | --------- | ---------------- |
| Mutable               | ❌ No      | ✅ Yes            |
| Reassign              | ❌         | ✅                |
| Thread Safety         | Better    | Less Safe        |
| Recommended           | ✅ Yes     | Only when needed |
| Android Best Practice | Preferred | Limited Use      |

---

# Interview Question

**Which should you prefer?**

Answer:

Always prefer **val**.

Use **var** only when the value must change.

This follows immutable programming principles.

---

# 6. Type Inference

One of Kotlin's biggest features.

Instead of writing

```kotlin
val age: Int = 25
```

Kotlin automatically detects the type.

```kotlin
val age = 25
```

Compiler infers

```
Int
```

Likewise

```kotlin
val price = 99.99
```

Compiler infers

```
Double
```

---

# Interview Question

**How does Kotlin know the type?**

Answer:

Through **Type Inference**.

The compiler automatically determines the variable type based on the assigned value.

---

# 7. Explicit Type Declaration

Sometimes we specify the type ourselves.

```kotlin
val age: Int = 25
```

Useful when

* API design
* Public methods
* Interfaces
* Generic programming
* Readability

---

# 8. Kotlin Data Types

Kotlin provides the following commonly used data types:

| Type    | Size           | Example   |
| ------- | -------------- | --------- |
| Byte    | 8-bit          | `10`      |
| Short   | 16-bit         | `1000`    |
| Int     | 32-bit         | `100`     |
| Long    | 64-bit         | `100L`    |
| Float   | 32-bit         | `10.5f`   |
| Double  | 64-bit         | `20.45`   |
| Char    | 16-bit Unicode | `'A'`     |
| Boolean | true/false     | `true`    |
| String  | Text           | `"Hello"` |

---

# 9. Integer Types

## Byte

```kotlin
val age: Byte = 25
```

Range

```
-128 to 127
```

---

## Short

```kotlin
val marks: Short = 30000
```

Range

```
-32768 to 32767
```

---

## Int

Most commonly used.

```kotlin
val salary = 50000
```

---

## Long

```kotlin
val population = 1400000000L
```

Notice

```
L
```

at the end.

---

# Interview Question

Why use `L`?

Answer:

Without `L`, Kotlin treats integer literals as `Int` by default.

---

# 10. Floating Types

## Float

```kotlin
val pi = 3.14f
```

Need

```
f
```

---

## Double

```kotlin
val pi = 3.1415926535
```

Double is default.

---

# Interview Question

Why use Float?

Answer:

Consumes less memory.

Suitable for graphics and games.

---

# 11. Char

Stores a single Unicode character.

```kotlin
val grade = 'A'
```

Not

```kotlin
"A"
```

because that is String.

---

# 12. Boolean

```kotlin
val isLogin = true
```

Used for

* Login
* Permission
* Validation
* Feature flags

---

# 13. String

Stores text.

```kotlin
val name = "Arvind"
```

String Template

```kotlin
val age = 25

println("Age = $age")
```

Expression

```kotlin
println("${age + 10}")
```

---

# 14. Nullable Types

By default

```kotlin
String
```

cannot hold null.

```kotlin
val name: String = null
```

Compiler Error

Correct

```kotlin
val name: String? = null
```

---

Android Example

```kotlin
var selectedUser: User? = null
```

---

# Interview Question

Why is Kotlin Null Safe?

Answer:

To eliminate **NullPointerException (NPE)** at compile time by distinguishing nullable (`Type?`) and non-nullable (`Type`) references.

---

# 15. Type Conversion

Kotlin does not perform implicit conversions.

❌

```kotlin
val number: Int = 10
val longValue: Long = number
```

Correct

```kotlin
val longValue = number.toLong()
```

Other conversions

```kotlin
toInt()
toLong()
toFloat()
toDouble()
toByte()
toShort()
toChar()
toString()
```

---

# Interview Question

Does Kotlin automatically convert Int to Long?

Answer:

No.

Kotlin requires explicit conversion to avoid unexpected behavior.

---

# 16. Default Values

Kotlin local variables must be initialized before use.

❌

```kotlin
var age: Int

println(age)
```

Compiler Error

Correct

```kotlin
var age = 0
```

---

# 17. Best Practices

✅ Prefer `val` over `var`

✅ Use meaningful names

```kotlin
val userName
```

Instead of

```kotlin
val x
```

✅ Let Kotlin infer types unless explicit types improve readability.

Avoid nullable variables unless necessary.

Use `Long` for timestamps.

Use `Double` for financial calculations when precision requirements are understood, or consider `BigDecimal` in financial domains.

---

# 18. Common Mistakes

❌ Using `var` everywhere

❌ Forgetting `L` for `Long`

❌ Forgetting `f` for `Float`

❌ Mixing `Char` and `String`

❌ Using unnecessary nullable types

❌ Expecting automatic type conversion

---

# 19. Android Real-World Examples

### User Name

```kotlin
val userName = "Arvind"
```

### RecyclerView Position

```kotlin
var selectedPosition = -1
```

### API URL

```kotlin
val BASE_URL = "https://api.example.com/"
```

### Login Status

```kotlin
var isLoggedIn = false
```

### Room Entity ID

```kotlin
val id: Long
```

### Timestamp

```kotlin
val createdAt = System.currentTimeMillis()
```

---

# 20. Frequently Asked Interview Questions

1. What is a variable in Kotlin?
2. What is the difference between `val` and `var`?
3. What is type inference?
4. When should you explicitly specify a type?
5. What are Kotlin primitive data types?
6. Why does `Long` require an `L` suffix?
7. Why does `Float` require an `f` suffix?
8. What is the default floating-point type in Kotlin?
9. Explain nullable and non-nullable types.
10. Why doesn't Kotlin perform implicit type conversion?
11. How do you convert an `Int` to a `Long`?
12. What is the difference between `Char` and `String`?
13. Why is immutability (`val`) recommended?
14. What causes a `NullPointerException` in Kotlin, and how can it be avoided?
15. Give real-world Android examples where `val` and `var` are appropriate.

---

## Key Takeaways

* Use **`val` by default** and **`var` only when mutation is required**.
* Kotlin's **type inference** reduces boilerplate while keeping code type-safe.
* Understand the differences among Kotlin's built-in data types and when to use each.
* Kotlin's **null safety** is one of its biggest advantages for writing reliable Android apps.
* Explicit **type conversions** are required; Kotlin avoids implicit numeric widening.
* These concepts form the foundation for all advanced Kotlin features, including collections, generics, coroutines, and Android development.
