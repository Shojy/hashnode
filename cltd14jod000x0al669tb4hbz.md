---
title: "Improve Testing: Fluent Assertions"
datePublished: Sun Mar 03 2024 00:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cltd14jod000x0al669tb4hbz
slug: improve-testing-fluent-assertions
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1709556974506/63ce9956-daf3-4ee6-9b44-e0ff8f079758.png
tags: csharp, unit-testing, testing, net, fluent-assertions

---

Fluent Assertions is a library that provides a more expressive and readable way of writing assertions in your unit tests compared to traditional assertion libraries like NUnit or MSTest. It offers a fluent interface that allows you to chain together various assertions in a natural and intuitive manner.

Key benefits of using Fluent Assertions include:

* **Readability**: Fluent Assertions provides a more natural language syntax for writing assertions, making your tests easier to understand and maintain.
    
* **Clarity**: The fluent syntax allows you to express the intent of your assertions more clearly, leading to more self-explanatory tests.
    
* **Extensibility**: Fluent Assertions comes with built-in support for a wide range of assertions covering different types of objects and scenarios. Additionally, it's extensible, allowing you to create custom assertions tailored to your specific needs.
    
* **Error Messages**: The error messages produced by Fluent Assertions are descriptive and informative, making it easier to diagnose and troubleshoot failing tests.
    

Example of using Fluent Assertions:

```csharp
// Traditional Assertion
Assert.AreEqual(expectedCount, actualList.Count, "The counts should match");

// Fluent Assertion
actualList.Count.Should().Be(expectedCount, "because that's the expected count");
```

By using Fluent Assertions, you can write more expressive and readable tests, leading to improved quality and maintainability of your test suite.

These examples illustrate how Fluent Assertions provides a more readable and expressive syntax for writing assertions in your unit tests, covering various scenarios commonly encountered in test cases. By leveraging Fluent Assertions, you can improve the quality and clarity of your tests, leading to more robust and maintainable test suites.

1. **Asserting Equality**:
    
    ```csharp
    // Traditional Assertion
    Assert.AreEqual(expectedValue, actualValue);
    
    // Fluent Assertion
    actualValue.Should().Be(expectedValue);
    ```
    
2. **Asserting Nullity**:
    
    ```csharp
    // Traditional Assertion
    Assert.IsNull(object);
    
    // Fluent Assertion
    someObject.Should().BeNull();
    ```
    
3. **Asserting Collections**:
    
    ```csharp
    // Traditional Assertion
    CollectionAssert.AreEqual(expectedCollection, actualCollection);
    
    // Fluent Assertion
    actualCollection.Should().BeEquivalentTo(expectedCollection);
    ```
    
4. **Asserting Strings**:
    
    ```csharp
    // Traditional Assertion
    Assert.IsTrue(actualString.Contains(substring));
    
    // Fluent Assertion
    actualString.Should().Contain(substring);
    ```
    
5. **Asserting Exceptions**:
    
    ```csharp
    // Traditional Assertion
    Assert.Throws<Exception>(() => someOperation());
    
    // Fluent Assertion
    someOperation.Should().Throw<Exception>();
    ```
    
6. **Asserting Object Properties**:
    
    ```csharp
    // Traditional Assertion
    Assert.AreEqual(expectedValue, actualObject.Property);
    
    // Fluent Assertion
    actualObject.Should()
        .HavePropertyWithValue(nameof(actualObject.Property), expectedValue);
    ```
    
7. **Asserting Object Types**:
    
    ```csharp
    // Traditional Assertion
    Assert.IsInstanceOfType(object, typeof(ExpectedType));
    
    // Fluent Assertion
    someObject.Should().BeOfType<ExpectedType>();
    ```
    
8. **Asserting Numerical Values**:
    
    ```csharp
    // Traditional Assertion
    Assert.IsTrue(actualValue > minValue && actualValue < maxValue);
    
    // Fluent Assertion
    actualValue.Should().BeInRange(minValue, maxValue);
    ```
    
9. **Asserting Collections Length**:
    
    ```csharp
    // Traditional Assertion
    Assert.AreEqual(expectedCount, actualCollection.Count);
    
    // Fluent Assertion
    actualCollection.Should().HaveCount(expectedCount);
    ```