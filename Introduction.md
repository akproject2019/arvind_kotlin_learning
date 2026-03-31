---
title: Introduction to Kotlin
description: Get started with Kotlin — a modern, concise, and safe programming language.
section: basics
order: 1
tags: [kotlin, introduction, basics]
---

# Introduction to Kotlin

Kotlin is a modern, statically typed programming language developed by JetBrains. It runs on the JVM, can be compiled to JavaScript, and also supports native compilation. Kotlin is officially supported by Google for Android development.

## Why Kotlin?

Kotlin offers several advantages over Java and other JVM languages:

- **Concise** — Dramatically reduces boilerplate code
- **Safe** — Null safety built into the type system
- **Interoperable** — 100% compatible with Java
- **Tool-friendly** — Excellent IDE support via IntelliJ IDEA

## Your First Kotlin Program

Let's write the classic "Hello, World!" program:

```kotlin
fun main() {
    println("Hello, World!")
}
```

That's it! No class required, no semicolons, just clean and readable code.

## Kotlin vs Java

Here's a quick comparison showing how Kotlin reduces boilerplate:

**Java:**
```java
public class Person {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public String getName() { return name; }
    public int getAge() { return age; }
}
```

**Kotlin:**
```kotlin
data class Person(val name: String, val age: Int)
```

Same result — one line in Kotlin vs 15+ lines in Java!

## Setting Up Your Environment

### Option 1: IntelliJ IDEA (Recommended)

1. Download [IntelliJ IDEA](https://www.jetbrains.com/idea/)
2. Create a new Kotlin project
3. Start coding!

### Option 2: Kotlin Playground

Visit [play.kotlinlang.org](https://play.kotlinlang.org) to run Kotlin code directly in your browser.

### Option 3: Command Line

```bash
# Install via SDKMAN
sdk install kotlin

# Run a Kotlin file
kotlinc hello.kt -include-runtime -d hello.jar
java -jar hello.jar
```

## Basic Program Structure

A typical Kotlin program looks like this:

```kotlin
// Package declaration (optional)
package com.example.myapp

// Import statements
import kotlin.math.sqrt

// Top-level function — entry point
fun main(args: Array<String>) {
    val name = "Kotlin"
    println("Welcome to $name!")
    
    val result = sqrt(16.0)
    println("Square root of 16 = $result")
}
```

## Key Concepts Overview

| Concept | Keyword | Example |
|---------|---------|---------|
| Immutable variable | `val` | `val x = 5` |
| Mutable variable | `var` | `var y = 10` |
| Function | `fun` | `fun greet() {}` |
| Class | `class` | `class Dog {}` |
| Interface | `interface` | `interface Flyable {}` |

## Summary

Kotlin is a powerful, expressive language that makes development more enjoyable. In the following topics, we'll explore variables, functions, classes, and more. Let's get started!
