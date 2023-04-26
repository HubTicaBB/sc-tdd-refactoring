# FizzBuzz - TDD Kata

## Task 1:
Write a function that takes a number as an argument and, based on the scenarios described below, returns `"Fizz"`, `"Buzz"`, `"FizzBuzz"` or that number.

   - If the number is divisible by 3, return `"Fizz"`.
   - If the number is divisible by 5, return `"Buzz"`.
   - If the number is divisible by both 3 and 5, return `"FizzBuzz"`.
   - If none of above applies, return the number as is.

### Sample output:
```console
- 1 => "1"
- 2 => "2"
- 3 => "Fizz"
- 4 => "4"
- 5 => "Buzz"
- 6 => "Fizz"
- 10 => "Buzz"
- 11 => "11"
- 15 => "FizzBuzz"
```

## Task 2: 
Write a function that takes a number as an argument and, based on the same scenarios described above, prints the number.

## Task 3: 
Write a function that takes user input and prints that number, based on the same scenarios described above.
   - If user input is not a number, throw an exception with the message: `"{inputNumber} is not a number"`.
   
## Task 4:
Refactor this code so that adding the new feature is very easy. "Make the change easy, then make the easy change".

### New feature
In addition to Multiples of 3 being "Fizz" and multiples of 5 being "Buzz":

Multiples of 7 are “Whizz”
Multiples of 11 are “Bang”

That means for example that multiples of 3 & 7 are “FizzWhizz”, multiples of 5 & 11 are “BuzzBang” etc.

### Sample output:
```console
- 1 => "1"
- 2 => "2"
- 3 => "Fizz"
- 4 => "4"
- 5 => "Buzz"
- 6 => "Fizz"
- 7 => "Whizz"
- 8 => "8"
- 9 => "Fizz"
- 10 => "Buzz"
- 11 => "Bang"
- 12 => "Fizz"
- 13 => "13"
- 14 => "Whizz"
- 15 => "FizzBuzz"
- 21 => "FizzWhizz"
- 55 => "BuzzBang"
- 105 => "FizzBazzWhizz"
- 165 => "FizzBuzzBang"
- 231 => "FizzWhizzBang"
- 1155 => "FizzBazzWhizzBang"
```

# Cheat Sheet

## Mocking Console

### C#

```csharp
public void LogMessage(Service service, string message)
{
    Console.WriteLine($"Service {service.name}: {message}");
}
```

```csharp
[TestMethod]
public void LogMessage_WritesMessageToConsole()
{
    StringWriter stringWriter = new StringWriter();
    Console.SetOut(stringWriter);
    var expected = "Service AuthService: Login successful";

    LogMessage("AuthService", "Login successful");
    var actuall = stringWriter.ToString().Trim();

    Assert.AreEqual(expected, actuall);
}
```

### JavaScript

```js
const logMessage (service, message) => {
    console.log(`Service ${service.name}: ${message}`);
}
```

```js
test("logMessage writes message to the console", () => {
    const consoleMock = jest.spyOn(console, "log");
    const expected = "Service AuthService: Login successful";
    
    logMessage("AuthService", "Login successful");
    
    expect(consoleMock).toHaveBeenCalledWith(expected);
});
```


## Using parametrized / data-driven tests

If you need to execute the same test with different inputs, instead of writing multiple tests, you can use parametrized tests.

### C#
In C#, you can achieve this by adding `[DataTestMethod]` attribute instead of `TestMethod` and adding multiple `[DataRow]` attributes that provide input data to the test. This will allow the test to run multiple times with different inputs.

```csharp
[DataTestMethod]
[DataRow(1)]
[DataRow(2)]
[DataRow(3)]
public void TestName(int number)
{
    var result = GetNumber(number);
    Assert.AreEqual(number, result);
}
```

If you need your input data to be reusable, you can create a property of type `IEnumerable<object[]>`, and add `[DynamicData]` attribute to your test, to specify where your test will receive data from.

```csharp
public static IEnumerable<object[]> MyNumbers => new[]
{
    new object[] { 1 },
    new object[] { 2 },
    new object[] { 3 },
};

[TestMethod]
[DynamicData(nameof(MyNumbers))]
public void TestName(int number)
{
    var result = GetNumber(number);
    Assert.AreEqual(number, result);
}
```

### JavaScript

In JavaScript, you can create an array, and pass it to your tests by using `test.each(arrayName)`

```js
const myNumbers = [[1], [2], [3]];

test.each(myNumbers)("test description", (number) => {
  const result = getNumber(number);
  expect(result).toEqual(number);
});
```
