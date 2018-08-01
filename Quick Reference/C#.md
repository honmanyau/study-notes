# C# Quick Reference

## Table of Contents
* [Types](#types)
  * [Common Built-in Types](#common-built-in-types)
  * [Implicit Typing](#implicit-typing)
  * [Enums](#enums)
* [Arrays](#arrays)
* [Dictionaries](#dictionaries)
* [`for` Loop](#for-loop)
* [Methods](#methods)


## Types

### Common Built-in Types

```csharp
bool spiffy = true;
char character = 'x'; // Note the single quotation marks
string str = "nyanpasu"; // Note the double quotation marks
int integer = 42;
long longInteger = 84;
float floaty = 4.2f;
double doubly = 4.2;
```

### Implicit Typing

```csharp
var num = 42;

Console.WriteLine(num.GetType()); // System.Int32
```

### Enums

```csharp
public enum Fruits {
  Apple,
  Orange,
  Watermelon,
}

Fruits fruit = Fruits.Apple;
Console.WriteLine (fruit); // Fruits.Apple
Console.WriteLine ((int)fruit); // 0
```

```csharp
public enum Fruits {
  Apple = 1,
  Orange,
  Watermelon,
}

Fruits apple = Fruits.Apple;
Fruits orange = Fruits.Orange;
Console.WriteLine ((int)apple); // 1
Console.WriteLine ((int)orange); // 2
```

## Arrays

```csharp
int[] numbers = { 0, 1, 2, 3, 4 };
int[] emptyIntArray = new int[10];

Console.WriteLine(numbers.Length); // 5
Console.WriteLine(numbers[1]); // 1
Console.WriteLine(numbers.GetType()); // System.Int32[]

emptyIntArray[0] = 42;
Console.WriteLine(emptyIntArray.Length); // 10
Console.WriteLine(emptyIntArray[0]); // 42
Console.WriteLine(emptyIntArray[1]); // 0
```

```csharp
int[,] twoByTwoMatrix = new int[2,2];
int[,] anotherTwoByTwoMatrix = new int[2,2] { {42, 0}, {0, 0} };

twoByTwoMatrix[0, 0] = 42;

Console.WriteLine(twoByTwoMatrix[0, 0]); // 42
Console.WriteLine(twoByTwoMatrix[1, 1]); // 0
Console.WriteLine(anotherTwoByTwoMatrix[0, 0]); // 42
Console.WriteLine(anotherTwoByTwoMatrix[1, 1]); // 0
```

## Lists

```csharp
List<int> numbers = new List<int>(); // The generic syntax defines type of the values that a list can store
int[] nestedArrayOfNumbers = { 0, 42 };

Console.WriteLine(numbers.Count); // 0

numbers.Add(42);
Console.WriteLine(numbers.Count); // 1
Console.WriteLine(numbers[0]); // 42

numbers.AddRange(nestedArrayOfNumbers); // AddRange() is also used for concatenation
Console.WriteLine(numbers.Count); // 3
Console.WriteLine(numbers[1]); // 4
Console.WriteLine(numbers[2]); // 2

numbers.Add(42);
numbers.Remove(42);
Console.WriteLine(numbers.Count); // 3
Console.WriteLine(numbers[0]); // 4
Console.WriteLine(numbers[numbers.Count - 1]); // 42

numbers.RemoveAt(0);
Console.WriteLine(numbers.Count); // 2
Console.WriteLine(numbers[0]); // 2
Console.WriteLine(numbers[1]); // 42
```

## Dictionaries

```csharp
Dictionary<string, int> elements = new Dictionary<string, int>();

    elements.Add("Hydrogen", 1);
    elements.Add("Molybdenum", 42);

    Console.WriteLine(elements.Count); // 2
    Console.WriteLine(elements["Hydrogen"]); // 1
    Console.WriteLine(elements["Molybdenum"]); // 42

    elements.Remove("Molybdenum");
    Console.WriteLine(elements.Count); // 1
```

## Methods

* `ToString()`
* `String.Substring(index, length)`
* `String.Replace(match, substitution)`
* `String.IndexOf(match)`
