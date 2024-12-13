---
layout: default
title: Android Test
nav_order: 2
parent: Quality Management
grand_parent: Performance & Quality Management
---

## **Androidテストの種類とテストコードパターン**

---

### **単体テスト (Unit Tests)**

- **説明**: 最も基本的で頻繁に書かれるテストです。実行速度が速いことが重要です。  
- **例**: 関数、メソッド、クラス単位のテスト  

---

### **統合テスト (Integration Tests)**

- **説明**: 複数のモジュールやコンポーネントが正しく連携して動作するかを確認します。  
- **例**: ViewModelとRepository間のデータフローのテスト  

---

### **UIテスト (UI Tests)**

- **説明**: ユーザーインターフェースの全体的なフローを検証します。  
- **例**: ログイン、決済プロセスなど  

---

~~~java
# JUnit
junit = { group = "junit", name = "junit", version = "4.13.2" }

# Mockito
mockito-core = { group = "org.mockito", name = "mockito-core", version = "4.5.1" }
mockito-kotlin = { group = "org.mockito.kotlin", name = "mockito-kotlin", version = "4.0.0" }
mockk = {group = "io.mockk", name="mockk", version="1.13.8"}

# Espresso
espresso-core = { group = "androidx.test.espresso", name = "espresso-core", version = "3.5.1" }
espresso-contrib = { group = "androidx.test.espresso", name = "espresso-contrib", version = "3.5.1" }
espresso-intents = { group = "androidx.test.espresso", name = "espresso-intents", version = "3.5.1" }
runner = {group = "androidx.test", name = "runner", version = "1.6.2"}
rules = {group = "androidx.test", name = "rules", version="1.6.1"}

# UIAutomator
uiautomator = { group = "androidx.test.uiautomator", name = "uiautomator", version = "2.2.0" }

// JUnit
testImplementation(libs.junit)

// Mockito
testImplementation(libs.mockito.core)
testImplementation(libs.mockito.kotlin)
testImplementation(libs.mockk)

// Espresso
androidTestImplementation(libs.espresso.core)
androidTestImplementation(libs.espresso.contrib)
androidTestImplementation(libs.espresso.intents)

// UIAutomator
androidTestImplementation(libs.uiautomator)
~~~

---

### Espresso Test Recorder
~~~java
// Espresso
androidTestImplementation(libs.espresso.core)
androidTestImplementation(libs.espresso.contrib)
// UIAutomator
androidTestImplementation(libs.uiautomator)
// AndroidX test library
androidTestImplementation(libs.runner)
androidTestImplementation(libs.rules)
~~~

---
## **テストコードパターン**

### **Given-When-Thenパターン**

#### **説明**

**Given-When-Then**は、**振る舞い駆動開発 (Behavior-Driven Development, BDD)**で使われるテストパターンです。テストをシナリオ形式で記述することで、ユーザーの行動と期待する結果を明確に表現できます。

---

#### **構成要素**

1. **Given (前提条件)**  
   - テストを実行する前に必要な**状態や状況**を設定します。  

2. **When (行動)**  
   - テスト対象の**動作やアクション**を実行します。  

3. **Then (検証)**  
   - 期待する結果が得られたかを**検証**します。  

---

### **サンプルコード**
~~~java
@Test
fun `given valid credentials when login is called then user is logged in`() {
    // Given
    val viewModel = LoginViewModel()
    val username = "testUser"
    val password = "testPass"

    // When
    val result = viewModel.login(username, password)

    // Then
    assertTrue(result.isSuccessful)
}
~~~

## **AAAパターン (Arrange-Act-Assert)**

---

### **説明**

**AAA (Arrange-Act-Assert)**は、テストを論理的な3つのステップに分けるパターンです。  
主に**単体テスト (Unit Testing)**で広く使われ、テストコードの構造をシンプルで明確にします。

---

### **構成要素**

1. **Arrange (準備)**  
   - テストで使用するオブジェクトやデータを設定します。  

2. **Act (実行)**  
   - テスト対象のメソッドや関数を実行します。  

3. **Assert (検証)**  
   - 実行結果が期待通りか検証します。  

---

### **サンプルコード**
~~~java
@Test
fun testLoginSuccess() {
    // Arrange
    val user = User("testUser", "password123")
    val loginViewModel = LoginViewModel()

    // Act
    val result = loginViewModel.login(user)

    // Assert
    assertTrue(result.isSuccessful)
}
~~~

{: .fs-6 .fw-300 }
