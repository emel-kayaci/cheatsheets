# Java Test Cheatsheet

# Unit Test

## Test Type Annotations
<table>
   <tr>
      <th>Type</th>
      <th>Definition</th>
   </tr>
   <tr>
      <td>
         Test
      </td>
      <td>Marks a method as a test method.</td>
   </tr>
   <tr>
      <td>
         Repeated Test
      </td>
      <td>
         <ul>
<p>

```java
@RepeatedTest(numOfRepetitions)
@RepeatedTest(value = 10, failureThreshold = 2)
```

</p>
            <li>Set the number of times a test repetition can fail before the remaining repetitions are skipped automatically.</li>
            <li>During each execution, the <code>@BeforeEach</code> and <code>@AfterEach</code> methods will be called, if defined.</li>
         </ul>
      </td>
   </tr>
   <tr>
      <td>Parameterized Test</td>
      <td>
         Execute a single test method multiple times with different parameters.
         <ul>
            <li>
               Single Value: 
<p>

```java
@ParameterizedTest
@ValueSource(ints = {1, 3, 5, -3, 15, Integer.MAX_VALUE})
void test(int param) {...}
```

</p>
            </li>
            <li>
               Multiple Values: 
<p>

```java
@ParameterizedTest(name = "weight={0}, height={1}")
@CsvSource({"70.0, 1.82", "80.0, 1.80"})
void test(int paramWeight, int paramHeight) {...}
```

</p>
            </li>
            </li>
            <li>
               Multiple Values from CSV file: 
<p>

```java
@ParameterizedTest
@CsvFileSource(resources = "/data.csv", numLinesToSkip = 1)
```

</p>    
            </li>
            <li>
               Enum Value: 
       <p>

```java
@ParameterizedTest
@EnumSource(EnumClass.class)
```

</p>        
            </li>
               <li>
               Null Value (parameter could be any reference type): 
       <p>

```java
@ParameterizedTest
@NullSource
void test(String input) {...}
```

</p>        
            </li>
                           <li>
               Empty Value (parameter also could be collection type or array):
       <p>

```java
@ParameterizedTest
@EmptySource
void test(String input) {...}
```

</p>        
            </li>
             <li>
               Both Null and Empty Values:
       <p>

```java
@ParameterizedTest
@NullAndEmptySource
void test(String input) {...}
```

</p>        
            </li>
         </ul>
      </td>
   </tr>
</table>

### Testing Exceptions

```java
  @Test
        void should_ThrowArithmeticException_When_Param2Zero() {

            // given
            double param1 = 50.0;
            double param2 = 0.0;

            // when
            Executable executable = () -> SampleClass.sampleMethod(param1, param2);

            // then
            assertThrows(ArithmeticException.class, executable);

        }
```

## Clean Test Tips

### Disabled 
Instead of wrapping everything in comments use the `@Disabled` annotation. When applied at the class level, all test methods within that class are automatically disabled as well.

### Nested 
- To reduce complexity when the test class grows use `@Nested`. It groups multiple test methods inside multiple nested classes of a main(outer) test class. Each nested class can have its own `@BeforeEach` and `@AfterEach`.

### DisplayName 
- `@DisplayName` could be used at the class and method level. The key benefit is that it can provide information about the test methods that show up in reporting and can be easily understood by any non-technical user.

### AssertAll 
- Use `@AssertAll` when checking multiple assertions. In code with multiple assertions, the test stops when the first of these assertions fails.

Without `@AssertAll`:

```java
@Test
void should_ReturnObjectWithWorstMetric_When_ObjectListNotEmptyWithoutAssertAll() {
    // given
    List<SomeObject> objects = new ArrayList<>();
    objects.add(new SomeObject(1.80, 60.0));
    objects.add(new SomeObject(1.82, 98.0));
    objects.add(new SomeObject(1.82, 64.7));

    // when
    Optional<SomeObject> objectWithWorstMetric = MetricCalculator.findObjectWithWorstMetric(objects);

    // then
    assertTrue(objectWithWorstMetric.isPresent());
    assertEquals(1.82, objectWithWorstMetric.get().getHeight()); // TEST FAILS HERE AND DO NOT EXECUTES NEXT ASSERTION 
    assertEquals(98.0, objectWithWorstMetric.get().getWeight());
}
```


With `@AssertAll`:

  ```java
  @Test
  void should_ReturnObjectWithWorstMetric_When_ObjectListNotEmpty() {
    // given
    List<SomeObject> objects = new ArrayList<>();
    objects.add(new SomeObject(1.80, 60.0));
    objects.add(new SomeObject(1.82, 98.0));
    objects.add(new SomeObject(1.82, 64.7));

    // when
    Optional<SomeObject> objectWithWorstMetric = MetricCalculator.findObjectWithWorstMetric(objects);

    // then
    assertAll(
            () -> assertTrue(objectWithWorstMetric.isPresent()),
            () -> assertEquals(1.82, objectWithWorstMetric.get().getHeight()), // TEST FAILS HERE 
            () -> assertEquals(98.0, objectWithWorstMetric.get().getWeight())  // STILL EXECUTES NEXT ASSERTION 
    );
  }

  ```

## Mockito 

### Annotations

Annotation | Usage
--- | --- 
`@ExtendWith(MockitoExtension.class)` | Mandatory annotation that must be added at the beginning of the class to use annotations.
`@Mock` | Marks a field as a mock object. Mockito will automatically create a mock instance of the annotated field when the test class is instantiated. 
`@Spy` | Mockito will create a <b>partial mock</b> of the annotated field, allowing you to retain the original behavior of the object unless overridden.
`@InjectMocks` | Injects mock or spy fields into the target object being tested. 
`@Captor` | Captures arguments passed to mock objects during method calls. Useful for verifying the arguments passed to methods on mocked objects.

### When & Do 

Example | Use With | Additional Information
--- | --- | ---
`when(...).thenReturn(...)` | Mock | Configures the behavior of a mock object when a specific method is called with particular arguments.
`when(...).thenThrow(...)` | Mock | Throw an exception when a specific method is called with particular arguments.
`when(...).thenAnswer(...)` | Mock | Allows `custom behavior` to be defined for a mock object when a specific method is called with particular arguments.
`doThrow(Exception.class).when(mockedService).someMethod()` | Void method or Spy | Cannot use `when` when throwing an exception with void methods or spies. Should use `doThrow` instead.
`doReturn().when(spy).method()` | Spy | Equivalent of `when(mock.method()).thenReturn()` for spies.

#### Multiple When
```java
@Test
void should_CountAvailablePlaces_WhenCalledMultipleTimes() {
  List<Room> roomList = Arrays.asList(
          new Room("ABC1", 10),
          new Room("DEF2", 30));

  when(roomService.getAvailableRooms())
          .thenReturn(roomList)
          .thenReturn(Collections.emptyList());

  int expectedFirstCall = 40;
  int expectedSecondCall = 0;

  int actualFirst = bookingService.getAvailablePlaceCount();
  int actualSecond = bookingService.getAvailablePlaceCount();

  assertAll(
          () -> assertEquals(expectedFirstCall, actualFirst),
          () -> assertEquals(expectedSecondCall, actualSecond)
  );
}
```

### Verify 

Example 
--- | 
`verify(...).methodCall(...)` 
`verify(..., times(n)).methodCall(...)` 
`verify(..., never()).methodCall(...)`
`verify(..., atLeastOnce()).methodCall(...)`	
`verify(..., atLeast(n)).methodCall(...)`
`verify(..., atMost(n)).methodCall(...)`
`verify(..., atMost(n)).methodCall(...)`
`verifyNoMoreInteractions(mockedObject)`

### Argument Matchers

Used when we want to mock the behavior for any argument of the given type.

- Cannot combine matchers with raw values, use eq or other additional matchers instead.

- All parameters can be raw values if the method call does not contain a matcher.

- Cannot use matchers as a return value. Return value must be exact.

```java
when(anyService.anyMethod(any(), 100)); // ERROR (raw and matcher value combined)
when(anyService.anyMethod(any(), eq(100))); // CORRECT USAGE (both matcher)
when(anyService.anyMethod(someObject, 100)); // CORRECT USAGE (both raw)
```
- Cannot use `any()` for primitive types. Use specialized versions of `any()` such as `anyDouble()`, `anyLong()` etc.

- `anyString()` does not match null string, `any()` should be used if there is a case where a null value is tested

#### Additional Matchers

Matcher | Full Name 
--- | --- 
eq() | Equals
gt() | Greater Than
lt() | Less Than
geq()	| Greater Than or Equals
leq()	| Less Than or Equals
startsWith() | String Starts With
endsWith() | String Ends With

### Argument Captor

- Allows us to capture an argument passed to a method to inspect it.
  
- Especially useful when we can’t access the argument outside of the method we’d like to test.

1. Add ArgumentCaptor Field

   ```java
    @Captor
    private ArgumentCaptor<Double> doubleCaptor;
   ```

2. Capture the Argument
   
   ```java
   verify(mockObject, times(1)).methodCall(any(), captor.capture()); // method is called once
   verify(mockObject, times(2)).methodCall(any(), captor.capture()); // method is called twice
   ```
   
3. Inspect the Captured Value
   
   ```java
   Double capturedArgument = captor.getValue(); // one value captured
   List<Double> capturedArguments = captor.getAllValues(); // multiple values captured
   ```

### Spy (Partial Mock)

Spy partially mocks a real object, while a mock creates a full mock object. Spies retain original behavior unless overridden, mocks don't.

For example, adding an element into the mocked list doesn’t add anything – it just calls the method with no other side-effect.

```java
@Test
public void whenCreateMock_thenCreated() {
    List mockedList = Mockito.mock(ArrayList.class);

    mockedList.add("one");
    Mockito.verify(mockedList).add("one");

    assertEquals(0, mockedList.size()); // True
}
```

A spy on the other hand will behave differently – it will call the real implementation of the add method and add the element to the underlying list:

```java
@Test
public void whenCreateSpy_thenCreate() {
    List spyList = Mockito.spy(new ArrayList());
    spyList.add("one");
    Mockito.verify(spyList).add("one");

    assertEquals(1, spyList.size());
}
```

### BDD Style

BDD (Behavior-Driven Development) style in Mockito emphasizes writing tests in a human-readable format that focuses on the behavior of the system, using methods like `given()` and `then()`.

- This spelling is only an alias to the form we use in normal Mockito such as when and verify.
  
Mockito | BDD Mockito
--- | --- 
`when(mockObject.methodCall()).thenReturn(value);` | `given(mockObject.methodCall()).willReturn(value);`
`verify(mockObject, times(1)).methodCall(argument1, argument2);` | `then(mockObject).should(times(1)).methodCall(argument1, argument2);`
