# Java Test Cheatsheet

## Unit Test

### Test Type Annotations
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

### Clean Test Tips

- Instead of wrapping everything in comments use the `@Disabled` annotation. When applied at the class level, all test methods within that class are automatically disabled as well.
- To reduce complexity when the test class grows use `@Nested`. It groups multiple test methods inside multiple nested classes of a main(outer) test class. Each nested class can have its own `@BeforeEach` and `@AfterEach`.
- `@DisplayName` could be used at the class and method level. The key benefit is that it can provide information about the test methods that show up in reporting and can be easily understood by any non-technical user.
